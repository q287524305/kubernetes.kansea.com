---
assignees:
- bprashanth

---

Kubernetes 的[`Pod`](/docs/user-guide/pods)的寿命是有限的。它们出生然后死亡，它们不会复活。[`ReplicationController`](/docs/user-guide/replication-controller)是特别用来动态的创建和销毁`Pods`（如：动态伸缩或者执行[rolling updates](/docs/user-guide/kubectl/kubectl_rolling-update)中动态的创建和销毁pod）。尽管每个`Pod`有自己的IP地址，随着时间变化随着时间推移即使这些IP地址也不能被认为是可靠的。这带来一个问题：如果一些`Pod`的集合（让我们称之为backends）为集群中的其他的`Pod`提供了一些功能（让我们称它们为frontends）,这些frontends应该如何找到并一直知道哪些backends在这样的集合中呢？

欢迎进入`Service`的世界。

一个Kubernetes的`Service`是一种抽象，它定义了一个`Pod`的逻辑集合和一个用于访问它们的策略 - 有的时候被称之为微服务。一个`Service`的目标`Pod`集合通常是由[`Label
Selector`](/docs/user-guide/labels/#label-selectors) 来决定的（下面有讲一个没有选择器的`Service` 有什么用处）。

举个例子，想象一个处理图片的后端运行了三个副本。这些副本都是可以替代的 - 前端不关心它们使用的是哪一个后端。尽管实际组成后端集合的`Pod`可能会变化，前端的客户端却不需要知道这个变化，也不需要自己有一个列表来记录这些后端服务。`Service`抽象能让你达到这种解耦。

对于那些Kubernetes原生的应用，Kubernetes提供了一个简单的`Endpoints` API，会在`Service`中的`Pod`集合发生改变的时候更新。对于非Kubernetes原生的应用，Kubernetes为Service提供了一种基于虚拟IP的桥接方式使其重定向到后端的Pods。


* TOC
{:toc}

## 定义一个`Service`

Kubernetes中的`Service`是一个REST对象，这点与`Pod`类似。正如所有的REST对象一样，向apiserver　POST一个`Service`的定义就能创建一个新的实例。例如，假设你有一组`Pods`，每一个Pod都开放了9376端口，并且都有一个`"app=MyApp"`的标签。

```json
{
    "kind": "Service",
    "apiVersion": "v1",
    "metadata": {
        "name": "my-service"
    },
    "spec": {
        "selector": {
            "app": "MyApp"
        },
        "ports": [
            {
                "protocol": "TCP",
                "port": 80,
                "targetPort": 9376
            }
        ]
    }
}
```

这个定义会创建一个新的`Service`对象，名字为"my-service"，它指向所有带有"app=MyApp"标签的Pod上面的9376端口。这个`Service`同时也会被分配一个IP地址（有时被称作"cluster ip"），它会被服务的代理所使用（见下面）。这个`Service`的选择器，会不断的对Pod进行筛选，并将结果POST到名字同样为“my-service”的Endpoints对象。


注意一个`Service`能将一个来源的端口映射到任意的`targetPort`。默认情况下，`targetPort`会被设置成与`port`字段一样的值。可能更有意思的地方在于，`targetPort`可以是一个字符串，能引用一个后端Pod中定义的端口名。实际指派给该名称的端口号在每一个`Pod`中可能会不同。这为部署和更新你的`Service`提供了很大的灵活性。例如，你可以在你的后端的下一个版本中更改开放的端口，而无需导致客户出现故障。

Kubernetes的`Service`支持`TCP`和`UDP`协议。默认是`TCP`。

### 没有选择器的Service

Service一般是用来对Kubernetes　Pod的访问进行抽象，但是也可以用来对其他类型的后端进行抽象，如：

  * 你想在生产环境中有一个外部的数据库集群，但是在测试环境中你想使用自己的数据库
  * 你想把你的service指向位于另外一个[`Namespace`](/docs/user-guide/namespaces)或者集群下的service
  * 你正在把你的workload迁移到Kubernetes，并且一些backend后端运行在Kubernetes之外

如果你碰到这些场景之一，那么你可以定义一个没有选择器的Service

```json
{
    "kind": "Service",
    "apiVersion": "v1",
    "metadata": {
        "name": "my-service"
    },
    "spec": {
        "ports": [
            {
                "protocol": "TCP",
                "port": 80,
                "targetPort": 9376
            }
        ]
    }
}
```


因为没有选择器，相应的`Endpoints`对象不会被创建。你可以手动将service映射到自己特定的endpoint。

```json
{
    "kind": "Endpoints",
    "apiVersion": "v1",
    "metadata": {
        "name": "my-service"
    },
    "subsets": [
        {
            "addresses": [
                { "ip": "1.2.3.4" }
            ],
            "ports": [
                { "port": 9376 }
            ]
        }
    ]
}
```

注意：Endpoint的IP不能是本地回环接口 (127.0.0.0/8)，本地链路地址(169.254.0.0/16)或者本地链路多播地址((224.0.0.0/24)。

不用选择器来访问一个`Service`的表现就像它有一个选择器一样。流量会被路由到用户定义的endpoint（在这个例子是：`1.2.3.4:9376`）。

## 虚拟IP和服务代理

每一个Kubernetes集群中的node都运行了一个`kube-proxy`
Every node in a Kubernetes cluster runs a `kube-proxy`.  This application
is responsible for implementing a form of virtual IP for `Service`s.  In
Kubernetes v1.0 the proxy was purely in userspace.  In Kubernetes v1.1 an
iptables proxy was added, but was not the default operating mode.  Since
Kubernetes v1.2, the iptables proxy is the default.

As of Kubernetes v1.0, `Services` are a "layer 3" (TCP/UDP over IP) construct.
In Kubernetes v1.1 the `Ingress` API was added (beta) to represent "layer 7"
(HTTP) services.

### Proxy-mode: userspace

In this mode, kube-proxy watches the Kubernetes master for the addition and
removal of `Service` and `Endpoints` objects. For each `Service` it opens a
port (randomly chosen) on the local node.  Any connections to this "proxy port"
will be proxied to one of the `Service`'s backend `Pods` (as reported in
`Endpoints`).  Which backend `Pod`  to use is decided based on the
`SessionAffinity` of the `Service`.  Lastly, it installs iptables rules which
capture traffic to the `Service`'s `clusterIP` (which is virtual) and `Port`
and redirects that traffic to the proxy port which proxies the backend `Pod`.

The net result is that any traffic bound for the `Service`'s IP:Port is proxied
to an appropriate backend without the clients knowing anything about Kubernetes
or `Services` or `Pods`.

By default, the choice of backend is round robin.  Client-IP based session affinity
can be selected by setting `service.spec.sessionAffinity` to `"ClientIP"` (the
default is `"None"`).

![Services overview diagram for userspace proxy](/images/docs/services-userspace-overview.svg)

### Proxy-mode: iptables

In this mode, kube-proxy watches the Kubernetes master for the addition and
removal of `Service` and `Endpoints` objects. For each `Service` it installs
iptables rules which capture traffic to the `Service`'s `clusterIP` (which is
virtual) and `Port` and redirects that traffic to one of the `Service`'s
backend sets.  For each `Endpoints` object it installs iptables rules which
select a backend `Pod`.

By default, the choice of backend is random.  Client-IP based session affinity
can be selected by setting `service.spec.sessionAffinity` to `"ClientIP"` (the
default is `"None"`).

As with the userspace proxy, the net result is that any traffic bound for the
`Service`'s IP:Port is proxied to an appropriate backend without the clients
knowing anything about Kubernetes or `Services` or `Pods`. This should be
faster and more reliable than the userspace proxy. However, unlike the
userspace proxier, the iptables proxier cannot automatically retry another
`Pod` if the one it initially selects does not respond, so it depends on
having working [readiness probes](/docs/user-guide/production-pods/#liveness-and-readiness-probes-aka-health-checks).

![Services overview diagram for iptables proxy](/images/docs/services-iptables-overview.svg)

## Multi-Port Services

Many `Services` need to expose more than one port.  For this case, Kubernetes
supports multiple port definitions on a `Service` object.  When using multiple
ports you must give all of your ports names, so that endpoints can be
disambiguated.  For example:

```json
{
    "kind": "Service",
    "apiVersion": "v1",
    "metadata": {
        "name": "my-service"
    },
    "spec": {
        "selector": {
            "app": "MyApp"
        },
        "ports": [
            {
                "name": "http",
                "protocol": "TCP",
                "port": 80,
                "targetPort": 9376
            },
            {
                "name": "https",
                "protocol": "TCP",
                "port": 443,
                "targetPort": 9377
            }
        ]
    }
}
```

## 选择自己的IP地址

你可以在`Service`创建的请求中指定自己的集群IP地址。做法就是设置`spec.clusterIP`字段。例如，如果你已经有了一个DNS条目，并且想替换它，或者一个已经配置到了一个特定IP地址的老系统，并且难以重新配置。用户选择的IP地址必须是一个可用的IP地址，并且必须在API Server的`service-cluster-ip-range`启动参数所指定的CIDR范围内。如果设置了一个非法的IP地址，API Server将会返回422 HTTP状态码来指明这是一个无效值。


### 为什么不使用轮询式DNS？

一个经常不时冒出来的问题是，为什么我选择用虚拟IP而不是使用标准的轮询式DNS做法。这有一些原因：

  * DNS库不遵守DNS TTLs并且对命名的查询进行缓存已经有很长的历史了
  * 很多应用只做一次DNS查询然后缓存查询的结果
  * 即使应用和库做了正确的重新解析，每一个客户端反反复复的DNS重新解析会难以管理

我们试着让用户不要做会伤害自己的事情。尽管如此，如果有足够的人想要这个特性，我们可能会实现它作为一个备选方案。

## 发现服务

Kubernetes支持两种主要的发现`Service`的模式 - 环境变量和DNS。

### 环境变量
当一个`Pod`运行在一个`Node`上的时候，kubelet会为每一个活跃的Service添加一组环境变量。它同时支持[Docker links
compatible](https://docs.docker.com/userguide/dockerlinks/)变量和简单的如`{SVCNAME}_SERVICE_HOST`和`{SVCNAME}_SERVICE_PORT`一样的变量，在后边的这些变量名中服务的名称是大写的，然后横线（`-`）会被转换成下划线（`_`）。

例如，服务`"redis-master"`开放了6379的TCP端口，并且被分配了10.0.0.11的集群IP地址，那么生成的变量如下：

```shell
REDIS_MASTER_SERVICE_HOST=10.0.0.11
REDIS_MASTER_SERVICE_PORT=6379
REDIS_MASTER_PORT=tcp://10.0.0.11:6379
REDIS_MASTER_PORT_6379_TCP=tcp://10.0.0.11:6379
REDIS_MASTER_PORT_6379_TCP_PROTO=tcp
REDIS_MASTER_PORT_6379_TCP_PORT=6379
REDIS_MASTER_PORT_6379_TCP_ADDR=10.0.0.11
```

*这确实隐含了一个对顺序的要求* - 任何一个`Pod`想要访问的`Service`必须在`Pod`自身被创建之前被创建好，否则环境变量无法被设置到这个Pod中去。DNS没有这种限制。

### DNS

一个可选的（也是我们强烈建议的）[cluster
add-on](http://releases.k8s.io/{{page.githubbranch}}/cluster/addons/README.md)是一个DNS服务器。DNS会监控Kubernetes的新的`Service`并且会为其创建一组DNS记录。如果DNS在整个集群中都被启用的话，那所有的`Pod`应该能自动对`Service`进行命名解析。

例如，如果你有一个叫做`my-service`的`Service`，其位于`"my-ns"`的`Namespace`下，那么对于`"my-service.my-ns"`就会有一个DNS的记录被创建。位于名为`"my-ns"` `Namespace`下的`Pod`可以通过简单地使用`"my-service"`进行查找。而位于其他`Namespaces`的`Pod`必须把查找名称指定为`"my-service.my-ns"`。这些命名查找的结果是一个集群的IP地址。

对于命名的端口，Kubernetes也支持DNS SRV (service)记录。如果名为`"my-service.my-ns"`的`Service` 有一个协议为`TCP`的名叫`"http"`的端口，你可以对`"_http._tcp.my-service.my-ns"`做一次DNS SRV查询来发现"http"的端口号。

## Headless services

有时你不需要一个单独的服务IP地址，也不需要做负载均衡。在这种情况下，你可以创建一个"headless"的Service，只需要把集群IP(`spec.clusterIP`)指定为`"None"`即可。

对于这种类型的`Service`，没有集群IP地址的分配。也不会有DNS的配置来为一个`Service`名称返回多个A记录，这些记录会直接指向支撑这个`Service`的`Pod`。另外，kube-proxy不会处理这些service，并且平台不会为它们做负载均衡或者代理。endpoints controller仍然会在API中创建`Endpoints`的记录。

这个选项让开发者可以减少对Kubernetes系统的耦合度，在他们想要的时候，能让他们自由决定如何用自己的方式去发现这些服务。应用仍然使用一个自注册的模式，并且其它服务发现的系统的适配器可以轻易的基于这个API被构建出来。

For such `Services` a cluster IP is not allocated, the kube proxy does not handle
these services, and there is no load balancing or proxying done by the platform
for them. How DNS is automatically configured depends on if the service has
selectors or not.

### With selectors

For headless services that define selectors, the endpoints controller creates
`Endpoints` records in the API, and modifies the DNS configuration to return A
records (addresses) which point directly to the `Pods` backing the `Service`.

### Without selectors

For headless services that do not define selectors, the endpoints controller does
not create `Endpoints` records. However, the DNS system looks for and configures
A records for any `Endpoints` that share a name with the service.
<<<<<
## 发布 services - service的类型

Kubernetes的`ServiceTypes`能让你指定你想要哪一种服务。默认的和基础的是`ClusterIP`，这会开放一个服务可以在集群内部进行连接。`NodePort` 和`LoadBalancer`是两种会将服务开放给外部网络的类型。

`ServiceType`字段的合法值是：

  * `ClusterIP`: 仅仅使用一个集群内部的IP地址 - 这是默认值，在上面已经讨论过。选择这个值意味着你只想这个服务在集群内部才可以被访问到。
  * `NodePort`: 在集群内部IP的基础上，在集群的每一个节点的端口上开放这个服务。你可以在任意`<NodeIP>:NodePort`地址上访问到这个服务。
  * `LoadBalancer`: 在使用一个集群内部IP地址和在NodePort上开放一个服务之外，向云提供商申请一个负载均衡器，会让流量转发到这个在每个节点上以`<NodeIP>:NodePort`的形式开放的服务上。

  在使用一个集群内部IP地址和在NodePort上开放一个Service的基础上，还可以向云提供者申请一个负载均衡器，将流量转发到已经以NodePort形式开发的Service上。

  注意尽管`NodePort`可以是TCP或者UDP的，对于Kubernetes 1.0来说，`LoadBalancer`还支持TCP。

### NodePort类型

如果你把`type`字段设置为`"NodePort"`，Kubernetes的master就会从由启动参数配置的范围（默认是：30000-32767）中分配一个端口，然后每一个Node都会将这个端口（在每一个Node上相同的端口）代理到你的`Service`。这个端口会被写入你的Service的`spec.ports[*].nodePort`字段中。

如果你想要一个特定的端口号，你可以在`nodePort`字段中指定一个值，确保系统能为你分配这个端口，否则API请求将会失败（例如你需要自己处理可能出现的端口冲突）。你指定的值必须在节点端口配置的范围内。

这给了开发者了设置他们自己的负载均衡器的自由，配置那些没有被Kubernetes完全支持的云环境，或者甚至可以直接开放一个或者多个节点的IP。

注意这种Service可以同时以`<NodeIP>:spec.ports[*].nodePort`和`spec.clusterIp:spec.ports[*].port`的形式访问。

### LoadBalancer类型

在那些支持外部负载均衡器的云提供者上面，将`type`字段设置为`"LoadBalancer"`会为你的`Service`设置好一个负载均衡器。该负载均衡器的实际的创建是异步进行的，并且该设置好均衡器会在该`Service`的`status.loadBalancer`字段中显示出来。例如：


```json
{
    "kind": "Service",
    "apiVersion": "v1",
    "metadata": {
        "name": "my-service"
    },
    "spec": {
        "selector": {
            "app": "MyApp"
        },
        "ports": [
            {
                "protocol": "TCP",
                "port": 80,
                "targetPort": 9376,
                "nodePort": 30061
            }
        ],
        "clusterIP": "10.0.171.239",
        "loadBalancerIP": "78.11.24.19",
        "type": "LoadBalancer"
    },
    "status": {
        "loadBalancer": {
            "ingress": [
                {
                    "ip": "146.148.47.155"
                }
            ]
        }
    }
}
```

从外部负载均衡器的流量将会被引到后端的`Pod`，然而具体这个如何实现则要看云提供商。一些云提供商允许指定`loadBalancerIP`。在这种场景，负载均衡器将随用户指定的`loadBalancerIP`一起创建。如果字段`loadBalancerIP`没有指定，该负载均衡器会被指定一个短暂性的IP。如果指定了`loadBalancerIP`，但是云提供商不支持这个特性，这个字段会被忽略。

#### SSL support on AWS
For partial SSL support on clusters running on AWS, starting with 1.3 two
annotations can be added to a `LoadBalancer` service:

```
    "metadata": {
        "name": "my-service",
        "annotations": {
            "service.beta.kubernetes.io/aws-load-balancer-ssl-cert": "arn:aws:acm:us-east-1:123456789012:certificate/12345678-1234-1234-1234-123456789012"
        }
    },
```

The first specifies which certificate to use. It can be either a
certificate from a third party issuer that was uploaded to IAM or one created
within AWS Certificate Manager.

```
    "metadata": {
        "name": "my-service",
        "annotations": {
            "service.beta.kubernetes.io/aws-load-balancer-backend-protocol=": "(https|http|ssl|tcp)"
        }
    },
```

The second annotation specificies which protocol a pod speaks. For HTTPS and
SSL, the ELB will expect the pod to authenticate itself over the encrypted
connection.

HTTP and HTTPS will select layer 7 proxying: the ELB will terminate
the connection with the user, parse headers and inject the `X-Forwarded-For`
header with the user's IP address (pods will only see the IP address of the
ELB at the other end of its connection) when forwarding requests.

TCP and SSL will select layer 4 proxying: the ELB will forward traffic without
modifying the headers.

### 外部IP

如果有外部的IP路由到一个或者更多的集群节点，Kubernetes的服务可以在这些`externalIPs`（外部端口）上开放出来。那些通过外部IP（作为目的IP）进入到集群的的流量，且是在服务的端口上，将会被路由到服务的一个endpoint上。`externalIPs`不是由Kubernetes管理的，这个责任属于集群的管理者。

在ServiceSpec（服务的定义）中，`externalIPs`可以伴随任意的`ServiceTypes`指定。在下面的例子中，my-service可以被客户端通过80.11.12.10:80（externalIP:port）访问到。


```json
{
    "kind": "Service",
    "apiVersion": "v1",
    "metadata": {
        "name": "my-service"
    },
    "spec": {
        "selector": {
            "app": "MyApp"
        },
        "ports": [
            {
                "name": "http",
                "protocol": "TCP",
                "port": 80,
                "targetPort": 9376
            }
        ],
        "externalIPs" : [
            "80.11.12.10"
        ]
    }
}
```

## 缺点


我们认为使用iptables和userspace来处理虚拟IP能应付小或者中等的规模，但是可能对于那些有成千的服务的非常大的集群来说可能不行。更多细节请参见[the original design proposal for portals](http://issue.k8s.io/1107) 。

使用kube-proxy会混淆访问一个`Service`的包的来源IP。这让一些类型的防火墙规则无法实现。

负载均衡器只支持TCP，不支持UDP。

`Type`字段被设计成一种嵌套的功能 - 每一个层级都会像前一个进行添加。不是所有的云提供商都这样严格要求（如Google Compute Engine不需要分配`NodePort`就能让`LoadBalancer`工作，但是AWS需要），但对于目前的API来说是必需的。


## 未来的工作


在未来我们可以展望代理策略会更加细致而不只是简单的轮询式的负载均衡，例如选主式的或者分片式的。我们也能预见一些`Service`将会有真正的负载均衡器，这样VIP仅仅是将数据包运送到此。

有一个[提案](http://issue.k8s.io/3760) 提出取消用户空间的代理，然后全部用iptables取而代之。这样性能会更好，而且也会消除来源IP不明的问题，尽管这较之随意的用户空间代码来说灵活性下降了。

我们想添加L7（HTTP）的`Service`作为作为首选的支持。

我们想有更多灵活的Service入口模式，包含目前的`ClusterIP`， `NodePort`， `LoadBalancer`和更多其他模式。


## 虚拟IP残酷的细节

之前的信息对于很多只想使用`Service`的人来说应该足够了。然而，背后还有很多细节值得理解。

### 防止冲突

Kubernetes的一个主要哲学是用户不应该被暴露在可能因为非自身原因而导致行为失败的环境中。这这种情形之下，我们要说说网络的端口 - 用户不应该选择一个可能会与其他用户冲突的端口号。这是一个隔离性的错误。

为了让用户为自己的`Service`选择一个端口，我们必须确保任意两个服务的端口不会发生冲突。我们通过给每一个Service分配自己的IP地址来完成这一点。

为了保证每一个Service都能获得一个唯一的IP，一个内部的分配器会自动更新一个etcd中的全局分配表（map）。这个map对象必须存在于注册表中才能让sercvie获得IP，否则创建就会失败并且提示不能分配IP。一个后台控制器负责创建这个map（从老版本的Kubernetes在内存中保存的方式迁移）同时检查因为管理员的干预而出现的无效的分配，并且清理任何已经分配的且没有任何Service正在使用的IP。

### IPs and VIPs

Unlike `Pod` IP addresses, which actually route to a fixed destination,
`Service` IPs are not actually answered by a single host.  Instead, we use
`iptables` (packet processing logic in Linux) to define virtual IP addresses
which are transparently redirected as needed.  When clients connect to the
VIP, their traffic is automatically transported to an appropriate endpoint.
The environment variables and DNS for `Services` are actually populated in
terms of the `Service`'s VIP and port.

We support two proxy modes - userspace and iptables, which operate slightly
differently.

#### Userspace

As an example, consider the image processing application described above.
When the backend `Service` is created, the Kubernetes master assigns a virtual
IP address, for example 10.0.0.1.  Assuming the `Service` port is 1234, the
`Service` is observed by all of the `kube-proxy` instances in the cluster.
When a proxy sees a new `Service`, it opens a new random port, establishes an
iptables redirect from the VIP to this new port, and starts accepting
connections on it.

When a client connects to the VIP the iptables rule kicks in, and redirects
the packets to the `Service proxy`'s own port.  The `Service proxy` chooses a
backend, and starts proxying traffic from the client to the backend.

This means that `Service` owners can choose any port they want without risk of
collision.  Clients can simply connect to an IP and port, without being aware
of which `Pods` they are actually accessing.

#### Iptables

Again, consider the image processing application described above.
When the backend `Service` is created, the Kubernetes master assigns a virtual
IP address, for example 10.0.0.1.  Assuming the `Service` port is 1234, the
`Service` is observed by all of the `kube-proxy` instances in the cluster.
When a proxy sees a new `Service`, it installs a series of iptables rules which
redirect from the VIP to per-`Service` rules.  The per-`Service` rules link to
per-`Endpoint` rules which redirect (Destination NAT) to the backends.

When a client connects to the VIP the iptables rule kicks in.  A backend is
chosen (either based on session affinity or randomly) and packets are
redirected to the backend.  Unlike the userspace proxy, packets are never
copied to userspace, the kube-proxy does not have to be running for the VIP to
work, and the client IP is not altered.

This same basic flow executes when traffic comes in through a node-port or
through a load-balancer, though in those cases the client IP does get altered.

## API对象

在Kubernetes中的REST API中，Service是一个顶层的资源。更多细节可以参见[Service API
object](/docs/api-reference/v1/definitions/#_v1_service)。

## For More Information

Read [Service Operations](/docs/user-guide/services/operations/).
