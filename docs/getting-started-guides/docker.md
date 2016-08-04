---
assignees:
- asridharan
- brendandburns
- fgrzadkowski

---

**停住你的脚步。现在开始，在本地环境下推荐使用[利用 Minikube 运行 Kubernetes](../minikube/) 了。**

下面将介绍，如果使用 Docker 搭建一个单点 kubernetes 群集。

下面是最终示意图:

![基于 Docker 的单节点 Kubernetes](/images/docs/k8s-singlenode-docker.png)

* TOC
{:toc}

## 前提条件

**注意: 下面这些步骤没有经过[Docker For Mac 和 Docker For Windows](https://blog.docker.com/2016/03/docker-for-mac-windows-beta/)的验证。**

1. 你必须拥有一台安装有 Docker "1.10"以上的机器。
2. 启用挂载。 在需要挂卷到其他容器时，Hyperkube 才会在容器中运行，例如在持久存储的情况下。所需步骤均依赖于系统的初始化。


  在 **systemd** 的情况下，更改 Docker 挂载标识为共享
  ```shell
    DOCKER_CONF=$(systemctl cat docker | head -1 | awk '{print $2}')
    sed -i.bak 's/^\(MountFlags=\).*/\1shared/' $DOCKER_CONF
    systemctl daemon-reload
    systemctl restart docker
  ```

  **除此以外**，使用 Hyperkube 手动设置安装点为共享:
  ```shell
    mkdir -p /var/lib/kubelet
    mount --bind /var/lib/kubelet /var/lib/kubelet
    mount --make-shared /var/lib/kubelet
  ```


### 运行

1. Decide which Kubernetes version to use. Set the `${K8S_VERSION}` variable to a version of Kubernetes >= "v1.2.0".

  如果你要使用 **稳定版** 的 Kubernetes，则运行:

  ```sh
  export K8S_VERSION=$(curl -sS https://storage.googleapis.com/kubernetes-release/release/stable.txt)
  ```

  **最新版本** (包含不稳定版本):

  ```sh
  export K8S_VERSION=$(curl -sS https://storage.googleapis.com/kubernetes-release/release/latest.txt)
  ```

2. Start Hyperkube

  ```shell
  export ARCH=amd64
  docker run -d \
      --volume=/sys:/sys:rw \
      --volume=/var/lib/docker/:/var/lib/docker:rw \
      --volume=/var/lib/kubelet/:/var/lib/kubelet:rw,shared \
      --volume=/var/run:/var/run:rw \
      --net=host \
      --pid=host \
      --privileged \
	  --name=kubelet \
      gcr.io/google_containers/hyperkube-${ARCH}:${K8S_VERSION} \
      /hyperkube kubelet \
          --hostname-override=127.0.0.1 \
          --api-servers=http://localhost:8080 \
          --config=/etc/kubernetes/manifests \
          --cluster-dns=10.0.0.10 \
          --cluster-domain=cluster.local \
          --allow-privileged --v=2
  ```

> 注意 `--cluster-dns` 和 `--cluster-domain` 是用来部署 dns 的, 如果不需要 dns 的话可以不设置。

> 如果你想要装在一个外部设备作为volume（卷）来使用的话，需要添加 `--volume=/dev:/dev` 到上面。 然而它可能会导致出现一些问题[#18230](https://github.com/kubernetes/kubernetes/issues/18230)

> `amd64` 以外的架构，有可能不稳定, 但如果你想尝试一下的话，可以设置为 `arm`, `arm64` 或者 `ppc64le`。 Kubernetes `v1.3.0-alpha.2` 以上的版本才开始支持 ARM. ARM 64-bit 和 PowerPC 64 需要 `v1.3.0-alpha.3` 或更高版本。 [在这](https://github.com/kubernetes/kubernetes/issues/17981)查看·multi-arch·支持进度

> If you are behind a proxy, you need to pass the proxy setup to curl in the containers to pull the certificates. Create a .curlrc under /root folder (because the containers are running as root) with the following line:
```
proxy = <your_proxy_server>:<port>
```

This actually runs the kubelet, which in turn runs a [pod](/docs/user-guide/pods/) that contains the other master components.

** **安全警告** ** services exposed via Kubernetes using Hyperkube are available on the host node's public network interface / IP address.  Because of this, this guide is not suitable for any host node/server that is directly internet accessible.  Refer to [#21735](https://github.com/kubernetes/kubernetes/issues/21735) for addtional info.

### 下载 `kubectl`

这时候你应该运行了一个 Kubernetes 群集。你可以下载`${K8S_VERSION}`版本的kubectl来进行测试。(本示例使用: `{{page.version}}.0`).


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
curl -sSL "https://storage.googleapis.com/kubernetes-release/release/{{page.version}}.0/bin/linux/amd64/kubectl" > /usr/bin/kubectl
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

### 测试

列出群集中的 node(节点)

```shell
kubectl get nodes
```

应该会打印出:

```shell
NAME        STATUS    AGE
127.0.0.1   Ready     1h
```

### 运行一个程序

```shell
kubectl run nginx --image=nginx --port=80
```

由于下载镜像需要花上几分钟，过一会运行 `docker ps` 你会看到 nginx 正在运行。

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
curl $ip
{% endraw %}```

在 macOS 中，使用以下命令，访问虚拟机中的 Docker:

```shell
docker-machine ssh `docker-machine active` curl $ip
```

## 部署 DNS

说明看 [这](/docs/getting-started-guides/docker-multinode/deployDNS/) 。

### 关闭群集

1\. 删除 Nginx 服务和 deployment:

If you plan on re-creating your nginx deployment and service you will need to clean it up.

```shell
kubectl delete service,deployments nginx
```

2\. Delete all the containers including the kubelet:

```shell
docker rm -f kubelet
docker rm -f `docker ps | grep k8s | awk '{print $1}'`
```

3\. 清理文件系统:

在 macOS 中，需要先通过 ssh 连接到 docker 虚拟机:

```shell
docker-machine ssh `docker-machine active`
```

```shell
grep /var/lib/kubelet /proc/mounts | awk '{print $2}' | sudo xargs -n1 umount
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
