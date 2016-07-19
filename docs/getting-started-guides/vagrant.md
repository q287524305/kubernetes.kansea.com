---
---

**停住你的脚步。现在开始，在本地环境下推荐使用[利用 Minikube 运行 Kubernetes](../minikube/) 了。**

使用 Vagrant（和 VirtualBox）运行 Kubernetes 是在本地机器（Linux，Mac OS X）进行运行/测试/开发的简单方法。



* TOC
{:toc}

### 前提条件

1. 安装版本高于 1.7.4 的 [Vagrant](http://www.vagrantup.com/downloads.html)
2. 下载一下中的一项:
   1. 最新版的 [Virtual Box](https://www.virtualbox.org/wiki/Downloads)
   2. 版本号>=5的 [VMWare Fusion](https://www.vmware.com/products/fusion/) 和合适的 [Vagrant VMWare Fusion provider](https://www.vagrantup.com/vmware)
   3. 版本号>=9的[VMWare Workstation](https://www.vmware.com/products/workstation/) 以及 [Vagrant VMWare Workstation provider](https://www.vagrantup.com/vmware)
   4. 版本号>=9的[Parallels Desktop](https://www.parallels.com/products/desktop/) 以及 [Vagrant Parallels provider](https://parallels.github.io/vagrant-parallels/)
   5. 支持硬件虚拟化并且带有libvirt的KVM. [Vagrant-libvirt](https://github.com/pradels/vagrant-libvirt). 为fedora提供官方的rpm，可以使用 `yum install vagrant-libvirt`命令来安装。

### 设置

简单运行下列代码就能够设置一个集群:

```sh
export KUBERNETES_PROVIDER=vagrant
curl -sS https://get.k8s.io | bash
```

你也可以下载 [Kubernetes 发行版](https://github.com/kubernetes/kubernetes/releases) 或者解压归档文件. 来打开终端运行以下命令启动本地集群:

```sh
cd kubernetes

export KUBERNETES_PROVIDER=vagrant
./cluster/kube-up.sh
```

环境变量 `KUBERNETES_PROVIDER` 用来告诉所有不同的集群管理脚本该使用哪一个脚本管理器（比如Vagrant）。如果你忘记设置这个变量，默认假设你运行Google Compute Engine上运行k8s。
默认情况下，Vagrant会创建一个单独的master VM（被称为kubernetes-master），以及一个节点VM（被称为kubernetes-node-1）。每个VM会占用1G内存，所以确保你有至少2G-4G的空余内存（以及合适的空闲硬盘空间）。

Vagrant 会提供集群中每台机器运行 Kuberbetes 所有必须的组件。每台机器会花费几分钟完成初始化设置。

如果你下载了多个Vagrant provider，Kubernetes通常会选择最恰当的那个。但是，你可以通过设置环境变量[`VAGRANT_DEFAULT_PROVIDER`](https://docs.vagrantup.com/v2/providers/default.html) 的值来让Kubernetes使用哪个Vagrant provider:

```sh
export VAGRANT_DEFAULT_PROVIDER=parallels
export KUBERNETES_PROVIDER=vagrant
./cluster/kube-up.sh
```

默认情况下每个集群中的VM运行 Fedora 系统。

通过下面的命令连接 master 或任意节点:

```sh
vagrant ssh master
vagrant ssh node-1
```

如果你运行超过一个节点，可以通过下面命令连接其它节点：

```sh
vagrant ssh node-2
vagrant ssh node-3
```

每一个安装了 Docker 和 kubelet 的 node (节点).

master节点会实例化Kubernetes master 组件作为pods运行在机器上。

查看kubernetes-master上的服务状态或者日志：

```console
[vagrant@kubernetes-master ~] $ vagrant ssh master
[vagrant@kubernetes-master ~] $ sudo su

[root@kubernetes-master ~] $ systemctl status kubelet
[root@kubernetes-master ~] $ journalctl -ru kubelet

[root@kubernetes-master ~] $ systemctl status docker
[root@kubernetes-master ~] $ journalctl -ru docker

[root@kubernetes-master ~] $ tail -f /var/log/kube-apiserver.log
[root@kubernetes-master ~] $ tail -f /var/log/kube-controller-manager.log
[root@kubernetes-master ~] $ tail -f /var/log/kube-scheduler.log
```

查看任意节点的服务状态：

```console
[vagrant@kubernetes-master ~] $ vagrant ssh node-1
[vagrant@kubernetes-master ~] $ sudo su

[root@kubernetes-master ~] $ systemctl status kubelet
[root@kubernetes-master ~] $ journalctl -ru kubelet

[root@kubernetes-master ~] $ systemctl status docker
[root@kubernetes-master ~] $ journalctl -ru docker
```

### 使用 Vagrant 与 Kubernetes 集群交互

启动好你的 Kubernetes 集群后，你可以使用常用的 Vagrant 命令管理集群中的节点。

源代码改变后推送更新到 Kubernetes 代码：

```sh
./cluster/kube-push.sh
```

暂停与重启集群：

```sh
vagrant halt
./cluster/kube-up.sh
```

删除集群：

```sh
vagrant destroy
```

一旦你的Vgrant机器运行并且提供相应资源，第一件要做的事情就是检查能否使用`kubectl.sh`脚本。

你可能需要先编译二进制文件，可以使用`make`命令：

```console
$ ./cluster/kubectl.sh get nodes

NAME                LABELS
10.245.1.4          <none>
10.245.1.5          <none>
10.245.1.3          <none>
```

### 验证你的 master

当我们使用vagrant provider管理Kubernetes时，`cluster/kubectl.sh`脚本会创建证书保存在home或～（ps：Linux中使用～表示当前用户的家目录）目录下的`~/.kubernetes_vagrant_auth`文件中，这样以后就不会出现验证的提示了。

```sh
cat ~/.kubernetes_vagrant_auth
```

```json
{ "User": "vagrant",
  "Password": "vagrant",
  "CAFile": "/home/k8s_user/.kubernetes.vagrant.ca.crt",
  "CertFile": "/home/k8s_user/.kubecfg.vagrant.crt",
  "KeyFile": "/home/k8s_user/.kubecfg.vagrant.key"
}
```

现在你可以使用`cluster/kubectl.sh`脚本了。例如使用下面的命令列出已经启动的节点：

```sh
./cluster/kubectl.sh get nodes
```

### 运行容器

你的集群已经运行起来了，你可以列出集群中的节点：

```sh
$ ./cluster/kubectl.sh get nodes

NAME                 LABELS
10.245.2.4           <none>
10.245.2.3           <none>
10.245.2.2           <none>
```

现在开始运行一些容器吧！

现在你可以用`cluster/kube-*.sh`的任何命令来与你的VMs交互。
启动容器之前并没有`pods`，`services`，`replication controllers`

```sh
$ ./cluster/kubectl.sh get pods
NAME        READY     STATUS    RESTARTS   AGE

$ ./cluster/kubectl.sh get services
NAME              CLUSTER_IP       EXTERNAL_IP       PORT(S)       SELECTOR               AGE

$ ./cluster/kubectl.sh get replicationcontrollers
CONTROLLER   CONTAINER(S)   IMAGE(S)   SELECTOR   REPLICAS
```

启动一个运行 nginx、使用`replication controllers`并且副本数为3的容器：

```sh
$ ./cluster/kubectl.sh run my-nginx --image=nginx --replicas=3 --port=80
```

（此时）列出pods，会看到3个容器已经被启动了，并且处于等待状态：

```sh
$ ./cluster/kubectl.sh get pods
NAME             READY     STATUS    RESTARTS   AGE
my-nginx-5kq0g   0/1       Pending   0          10s
my-nginx-gr3hh   0/1       Pending   0          10s
my-nginx-xql4j   0/1       Pending   0          10s
```

你需要等待（Vagrant）分配好资源，这时可以通过命令来监测节点：

```sh
$ vagrant ssh node-1 -c 'sudo docker images'
kubernetes-node-1:
    REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
    <none>              <none>              96864a7d2df3        26 hours ago        204.4 MB
    google/cadvisor     latest              e0575e677c50        13 days ago         12.64 MB
    kubernetes/pause    latest              6c4579af347b        8 weeks ago         239.8 kB
```

下载好 docker nginx 镜像后容器就会启动，通过下面的命令查看：

```sh
$ vagrant ssh node-1 -c 'sudo docker ps'
kubernetes-node-1:
    CONTAINER ID        IMAGE                     COMMAND                CREATED             STATUS              PORTS                    NAMES
    dbe79bf6e25b        nginx:latest              "nginx"                21 seconds ago      Up 19 seconds                                k8s--mynginx.8c5b8a3a--7813c8bd_-_3ffe_-_11e4_-_9036_-_0800279696e1.etcd--7813c8bd_-_3ffe_-_11e4_-_9036_-_0800279696e1--fcfa837f
    fa0e29c94501        kubernetes/pause:latest   "/pause"               8 minutes ago       Up 8 minutes        0.0.0.0:8080->80/tcp     k8s--net.a90e7ce4--7813c8bd_-_3ffe_-_11e4_-_9036_-_0800279696e1.etcd--7813c8bd_-_3ffe_-_11e4_-_9036_-_0800279696e1--baf5b21b
    aa2ee3ed844a        google/cadvisor:latest    "/usr/bin/cadvisor"    38 minutes ago      Up 38 minutes                                k8s--cadvisor.9e90d182--cadvisor_-_agent.file--4626b3a2
    65a3a926f357        kubernetes/pause:latest   "/pause"               39 minutes ago      Up 39 minutes       0.0.0.0:4194->8080/tcp   k8s--net.c5ba7f0e--cadvisor_-_agent.file--342fd561
```

这时再次列举`pods`，`services`和`replication controllers`，会看到：

```sh
$ ./cluster/kubectl.sh get pods
NAME             READY     STATUS    RESTARTS   AGE
my-nginx-5kq0g   1/1       Running   0          1m
my-nginx-gr3hh   1/1       Running   0          1m
my-nginx-xql4j   1/1       Running   0          1m

$ ./cluster/kubectl.sh get services
NAME              CLUSTER_IP       EXTERNAL_IP       PORT(S)       SELECTOR               AGE

$ ./cluster/kubectl.sh get replicationcontrollers
CONTROLLER   CONTAINER(S)   IMAGE(S)   SELECTOR       REPLICAS   AGE
my-nginx     my-nginx       nginx      run=my-nginx   3          1m
```

我们没有启动任何服务，因此列举服务是什么也没有。但是我们看到3个副本正常显示。查看 [运行第一个容器](../user-guide/simple-nginx/) 了解如何创建服务。现在你已经可以尝试增加或减少副本了：

```sh
$ ./cluster/kubectl.sh scale rc my-nginx --replicas=2
$ ./cluster/kubectl.sh get pods
NAME             READY     STATUS    RESTARTS   AGE
my-nginx-5kq0g   1/1       Running   0          2m
my-nginx-gr3hh   1/1       Running   0          2m
```

祝贺你!

## 常见问题

#### I keep downloading the same (large) box all the time!

Vagrantfile 默认从 S3 下载 box。当调用`kube-up.sh`脚本是你可以提供name和可选的连接作为参数来修改下载站点：

```sh
export KUBERNETES_BOX_NAME=choose_your_own_name_for_your_kuber_box
export KUBERNETES_BOX_URL=path_of_your_kuber_box
export KUBERNETES_PROVIDER=vagrant
./cluster/kube-up.sh
```

#### I am getting timeouts when trying to curl the master from my host!

During provision of the cluster, you may see the following message:

```sh
Validating node-1
.............
Waiting for each node to be registered with cloud provider
error: couldn't read version from server: Get https://10.245.1.2/api: dial tcp 10.245.1.2:443: i/o timeout
```

Some users have reported VPNs may prevent traffic from being routed to the host machine into the virtual machine network.

To debug, first verify that the master is binding to the proper IP address:

```sh
$ vagrant ssh master
$ ifconfig | grep eth1 -C 2
eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST> mtu 1500 inet 10.245.1.2 netmask
   255.255.255.0 broadcast 10.245.1.255
```

Then verify that your host machine has a network connection to a bridge that can serve that address:

```sh
$ ifconfig | grep 10.245.1 -C 2

vboxnet5: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.245.1.1  netmask 255.255.255.0  broadcast 10.245.1.255
        inet6 fe80::800:27ff:fe00:5  prefixlen 64  scopeid 0x20<link>
        ether 0a:00:27:00:00:05  txqueuelen 1000  (Ethernet)
```

If you do not see a response on your host machine, you will most likely need to connect your host to the virtual network created by the virtualization provider.

If you do see a network, but are still unable to ping the machine, check if your VPN is blocking the request.

#### I just created the cluster, but I am getting authorization errors!

You probably have an incorrect ~/.kubernetes_vagrant_auth file for the cluster you are attempting to contact.

```sh
rm ~/.kubernetes_vagrant_auth
```

After using kubectl.sh make sure that the correct credentials are set:

```sh
cat ~/.kubernetes_vagrant_auth
```

```json
{
  "User": "vagrant",
  "Password": "vagrant"
}
```

#### I just created the cluster, but I do not see my container running!

If this is your first time creating the cluster, the kubelet on each node schedules a number of docker pull requests to fetch prerequisite images.  This can take some time and as a result may delay your initial pod getting provisioned.

#### I have brought Vagrant up but the nodes cannot validate!

Log on to one of the nodes (`vagrant ssh node-1`) and inspect the salt minion log (`sudo cat /var/log/salt/minion`).

#### I want to change the number of nodes!

You can control the number of nodes that are instantiated via the environment variable `NUM_NODES` on your host machine.  If you plan to work with replicas, we strongly encourage you to work with enough nodes to satisfy your largest intended replica size.  If you do not plan to work with replicas, you can save some system resources by running with a single node. You do this, by setting `NUM_NODES` to 1 like so:

```sh
export NUM_NODES=1
```

#### I want my VMs to have more memory!

You can control the memory allotted to virtual machines with the `KUBERNETES_MEMORY` environment variable.
Just set it to the number of megabytes you would like the machines to have. For example:

```sh
export KUBERNETES_MEMORY=2048
```

If you need more granular control, you can set the amount of memory for the master and nodes independently. For example:

```sh
export KUBERNETES_MASTER_MEMORY=1536
export KUBERNETES_NODE_MEMORY=2048
```

#### I want to set proxy settings for my Kubernetes cluster boot strapping!

If you are behind a proxy, you need to install vagrant proxy plugin and set the proxy settings by

```sh
vagrant plugin install vagrant-proxyconf
export VAGRANT_HTTP_PROXY=http://username:password@proxyaddr:proxyport
export VAGRANT_HTTPS_PROXY=https://username:password@proxyaddr:proxyport
```

Optionally you can specify addresses to not proxy, for example

```sh
export VAGRANT_NO_PROXY=127.0.0.1
```

If you are using sudo to make kubernetes build for example make quick-release, you need run `sudo -E make quick-release` to pass the environment variables.

#### I ran vagrant suspend and nothing works!

`vagrant suspend` seems to mess up the network.  This is not supported at this time.

#### I want vagrant to sync folders via nfs!

You can ensure that vagrant uses nfs to sync folders with virtual machines by setting the KUBERNETES_VAGRANT_USE_NFS environment variable to 'true'. nfs is faster than virtualbox or vmware's 'shared folders' and does not require guest additions. See the [vagrant docs](http://docs.vagrantup.com/v2/synced-folders/nfs.html) for details on configuring nfs on the host. This setting will have no effect on the libvirt provider, which uses nfs by default. For example:

```sh
export KUBERNETES_VAGRANT_USE_NFS=true
```
