---
---

**停住你的脚步。现在开始已经推荐使用[利用 Minikube 运行 Kubernetes](../minikube/) 了。**

下面将介绍，如果使用 Docker 搭建一个单点 kubernetes 群集。

下面是最终示意图:

![基于 Docker 的单节点 Kubernetes](/images/docs/k8s-singlenode-docker.png)

* TOC
{:toc}

## 前提条件

**注意: 下面这些步骤没有经过[Docker For Mac和Docker For Windows](https://blog.docker.com/2016/03/docker-for-mac-windows-beta/)的验证。**

1. 你必须拥有一台安装有 Docker 的机器。
2. 选择使用什么版本的 Kubernetes，设置 ` ${K8S_VERSION}`  为高于或等于 "V1.2.0" 的版本号，如果你想使用稳定版的话，那么你可以运行一下命令:

```sh
export K8S_VERSION=$(curl -sS https://storage.googleapis.com/kubernetes-release/release/stable.txt)
```

或者获取最新版本(包括不稳定版本):

```sh
export K8S_VERSION=$(curl -sS https://storage.googleapis.com/kubernetes-release/release/latest.txt)
```

### 运行

```shell
export ARCH=amd64
docker run -d \
    --volume=/:/rootfs:ro \
    --volume=/sys:/sys:rw \
    --volume=/var/lib/docker/:/var/lib/docker:rw \
    --volume=/var/lib/kubelet/:/var/lib/kubelet:rw \
    --volume=/var/run:/var/run:rw \
    --net=host \
    --pid=host \
    --privileged \
    gcr.io/google_containers/hyperkube-${ARCH}:${K8S_VERSION} \
    /hyperkube kubelet \
        --containerized \
        --hostname-override=127.0.0.1 \
        --api-servers=http://localhost:8080 \
        --config=/etc/kubernetes/manifests \
        --cluster-dns=10.0.0.10 \
        --cluster-domain=cluster.local \
        --allow-privileged --v=2
```

> Note that `--cluster-dns` and `--cluster-domain` is used to deploy dns, feel free to discard them if dns is not needed.

> If you would like to mount an external device as a volume, add `--volume=/dev:/dev` to the command above. It may however, cause some problems described in [#18230](https://github.com/kubernetes/kubernetes/issues/18230)

> Architectures other than `amd64` are experimental and sometimes unstable, but feel free to try them out! Valid values: `arm`, `arm64` and `ppc64le`. ARM is available with Kubernetes version `v1.3.0-alpha.2` and higher. ARM 64-bit and PowerPC 64 little-endian are available with `v1.3.0-alpha.3` and higher. Track progress on multi-arch support [here](https://github.com/kubernetes/kubernetes/issues/17981)

This actually runs the kubelet, which in turn runs a [pod](/docs/user-guide/pods/) that contains the other master components.

** **SECURITY WARNING** ** services exposed via Kubernetes using Hyperkube are available on the host node's public network interface / IP address.  Because of this, this guide is not suitable for any host node/server that is directly internet accessible.  Refer to [#21735](https://github.com/kubernetes/kubernetes/issues/21735) for addtional info.

### 下载 `kubectl`

At this point you should have a running Kubernetes cluster. You can test it out
by downloading the kubectl binary for `${K8S_VERSION}` (in this example: `{{page.version}}.0`).


下载:

 - `linux/amd64`: http://storage.googleapis.com/kubernetes-release/release/{{page.version}}.0/bin/linux/amd64/kubectl
 - `linux/386`: http://storage.googleapis.com/kubernetes-release/release/{{page.version}}.0/bin/linux/386/kubectl
 - `linux/arm`: http://storage.googleapis.com/kubernetes-release/release/{{page.version}}.0/bin/linux/arm/kubectl
 - `linux/arm64`: http://storage.googleapis.com/kubernetes-release/release/{{page.version}}.0/bin/linux/arm64/kubectl
 - `linux/ppc64le`: http://storage.googleapis.com/kubernetes-release/release/{{page.version}}.0/bin/linux/ppc64le/kubectl
 - `OS X/amd64`: http://storage.googleapis.com/kubernetes-release/release/{{page.version}}.0/bin/darwin/amd64/kubectl
 - `OS X/386`: http://storage.googleapis.com/kubernetes-release/release/{{page.version}}.0/bin/darwin/386/kubectl
 - `windows/amd64`: http://storage.googleapis.com/kubernetes-release/release/{{page.version}}.0/bin/windows/amd64/kubectl.exe
 - `windows/386`: http://storage.googleapis.com/kubernetes-release/release/{{page.version}}.0/bin/windows/386/kubectl.exe

通用下载路径:
```
http://storage.googleapis.com/kubernetes-release/release/${K8S_VERSION}/bin/${GOOS}/${GOARCH}/${K8S_BINARY}
```

`linux/amd64`的一个安装例子:

```
curl -sSL "http://storage.googleapis.com/kubernetes-release/release/{{page.version}}.0/bin/linux/amd64/kubectl" > /usr/bin/kubectl
chmod +x /usr/bin/kubectl
```

On OS X, to make the API server accessible locally, setup a ssh tunnel.

```shell
docker-machine ssh `docker-machine active` -N -L 8080:localhost:8080
```

Setting up a ssh tunnel is applicable to remote docker hosts as well.

(Optional) Create kubernetes cluster configuration:

```shell
kubectl config set-cluster test-doc --server=http://localhost:8080
kubectl config set-context test-doc --cluster=test-doc
kubectl config use-context test-doc
```

### Test it out

List the nodes in your cluster by running:

```shell
kubectl get nodes
```

This should print:

```shell
NAME        STATUS    AGE
127.0.0.1   Ready     1h
```

### 运行一个程序

```shell
kubectl run nginx --image=nginx --port=80
```

Now run `docker ps` you should see nginx running. You may need to wait a few minutes for the image to get pulled.

### 用 service 来开放端口

```shell
kubectl expose deployment nginx --port=80
```

运行一下命令，来获取我们刚刚创建的群集的 service 的 IP 地址:

```shell{% raw %}
ip=$(kubectl get svc nginx --template={{.spec.clusterIP}})
echo $ip
{% endraw %}```

在浏览器中输入这个 IP:

```shell{% raw %}
kubectl get svc nginx --template={{.spec.clusterIP}}
{% endraw %}```

在 macOS 中，使用以下命令，访问虚拟机中的 Docker:

```shell
docker-machine ssh `docker-machine active` curl $ip
```

## 部署 DNS

说明看 [这](/docs/getting-started-guides/docker-multinode/deployDNS/) 。

### 关闭群集

1. 删除所有容器和 kubelet:

Many of these containers run under the management of the `kubelet` binary, which attempts to keep containers running, even if they fail.
So, in order to turn down the cluster, you need to first kill the kubelet container, and then any other containers.

You may use `docker rm -f $(docker ps -aq)`, note this removes _all_ containers running under Docker, so use with caution.

2. 清理文件系统:

在 macOS 中，需要先通过 ssh 连接到 docker 虚拟机:

```shell
docker-machine ssh `docker-machine active`
```

```shell
sudo umount `cat /proc/mounts | grep /var/lib/kubelet | awk '{print $2}'`
sudo rm -rf /var/lib/kubelet
```

### 排除故障

#### Node 是 `NotReady` 状态

如果你发现你的 node (节点)的状态是 `NotReady` ，那么他可能是因为你的系统不具备 memcg 而导致的。

1. 你的系统内核应该支持`memory accounting`，输入一下命令来开启它:

```shell
CONFIG_RESOURCE_COUNTERS=y
CONFIG_MEMCG=y
```

2. 在内核启动时启用`memory accounting`的命令:

```shell
GRUB_CMDLINE_LINUX="cgroup_enable=memory=1"
```

注意：以上只适用于GRUB2。
通过查看 `/proc/cmdline` 可以确认命令行参数是否已经成功传给内核：

```shell
$ cat /proc/cmdline
BOOT_IMAGE=/boot/vmlinuz-3.18.4-aufs root=/dev/sda5 ro cgroup_enable=memory=1
```
## 支持级别


IaaS Provider        | Config. Mgmt | OS     | Networking  | Docs                                              | Conforms | Support Level
-------------------- | ------------ | ------ | ----------  | ---------------------------------------------     | ---------| ----------------------------
Docker Single Node   | custom       | N/A    | local       | [docs](/docs/getting-started-guides/docker)                                 |          | Project ([@brendandburns](https://github.com/brendandburns))


所有解决方案及支持级别的信息，请参阅[解决方案表格](/docs/getting-started-guides/#table-of-solutions) .


## 深入阅读

更多详细的使用和管理 Kubernetes 群集相关的信息，请看 [Kubernetes 文档](/docs/)
