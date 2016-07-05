---
---

**停住你的脚步。现在开始，在本地环境下推荐使用[利用 Minikube 运行 Kubernetes](../minikube/) 了。**

* TOC
{:toc}

### 环境需求

#### Linux

没有运行Linux？考虑下使用[vagrant](https://www.vagrantup.com/)在虚拟机中运行Linux，或者像[Google Compute Engine](/docs/getting-started-guides/gce)这样的云提供商上运行。

#### Docker

至少 [Docker](https://docs.docker.com/installation/#installation)
1.8.3+. 。确保Docker守护进程一直运行，并确保能交互（比如`docker
ps`）。一些Kubernetes组件需要root权限运行，这样这些组件才能和Docker正常、良好地工作。

#### etcd

你需要配置 [etcd](https://github.com/coreos/etcd/releases) 到环境变量，请确保安装 etcd 并且正确配置到 ``$PATH``中。

#### go

[go](https://golang.org/doc/install) 版本至少 1.3+，请确保安装好go并配置好 ``$PATH``。

### 启动集群

首先, 你需要 [下载 Kubernetes](/docs/getting-started-guides/binary_release/). 新打开一个单独的终端，
运行下面的指令（由于启动/停止kubernetes守护进程需要root权限，所以使用root权限运行整个脚本会使操作更加容易）：:

```shell
cd kubernetes
hack/local-up-cluster.sh
```

这个操作将会构建和启动一个轻量的本地集群，包含一个master和一个节点。输入ctrl+C关闭集群。

可以使用脚本`cluster/kubectl.sh` 来与本地群集交互。`hack/local-up-cluster.sh` 会让`kubectl`指向本地群集。


### 运行容器

成功运行集群后，我想你很定迫不及待地想启动你的容器了。

现在你可以使用`cluster/kubectl.sh`脚本中的命令来和本地集群交互了：

```shell
export KUBERNETES_PROVIDER=local
cluster/kubectl.sh get pods
cluster/kubectl.sh get services
cluster/kubectl.sh get deployments
cluster/kubectl.sh run my-nginx --image=nginx --replicas=2 --port=80

## 在等待命令完成前，你可以打开一个新终端查看docker拉取镜像
  sudo docker images
  ## 你会看到docker正在拉去nginx镜像
  sudo docker ps
  ## 你会看到你的容器正在运行
  exit
## end wait

## 查看 kubernetes 相关信息
cluster/kubectl.sh get pods
cluster/kubectl.sh get services
cluster/kubectl.sh get deployments
```

### 运行用户定义的pod

要注意[容器](/docs/user-guide/containers)和 [pod](/docs/user-guide/pods) 之前的不同。
如果你只向 Kubernetes 请求前者（容器），Kubernetes 会创建一个新的封装好的 pod 给你。
但是（通过这个pod）你不能在本地主机查看到 nginx 的开始页面。为了验证 nginx 正确在容器中运行，你需要在容器中运行`curl`（通过`docker exec`执行）。

你可以通过用户定义的 manifest 来控制 pod 的信息。指定端口就可以在浏览器中访问 nginx 了：

```shell
cluster/kubectl.sh create -f docs/user-guide/pod.yaml
```

祝你好运!

### 问题

#### 我不能通过IP访问服务

一些使用 iptables 工具的防火墙软件不能很好地与 Kubernetes 配合。
如果你在网络上遇到麻烦，首先尝试关闭系统的防火墙或者其它使用 iptables 的系统。
此外，通过 `journalctl --since yesterday | grep avc` 指令检查 SELinux 是否屏蔽了什么。

集群IPs默认范围是 10.0.*.*，
这有可能引起 docker 容器IP和集群IP冲突。
如果你发现容器IP也在集群的IP范围内，
编辑 `hack/local-cluster-up.sh` 脚本的修改`service-cluster-ip-range`标识来更改集群的IP范围。

#### 我修改Kubernetes代码后该如何运行它？

```shell
cd kubernetes
hack/build-go.sh
hack/local-up-cluster.sh
```

#### kubectl显示容器已启动，但是get pod和docker ps命令没有显示。

一个或者多个 Kubernetes 守护进程可能已经崩溃，查看每一个`/tmp`文件夹下的[logs](/docs/admin/cluster-troubleshooting/#looking-at-logs) 。

```shell
$ ls /tmp/kube*.log
$ tail -f /tmp/kube-apiserver.log
```

#### 通过主机名 pod 与 service 链接失败

`local-up-cluster.sh`脚本没有启动 DNS 服务。类似的情况[在这](http://issue.k8s.io/6667). 你可以手动启动，相关的文档在 [这里](https://releases.k8s.io/{{page.githubbranch}}/cluster/addons/dns#how-do-i-configure-it)
