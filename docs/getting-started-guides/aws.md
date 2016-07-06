---
---

* TOC
{:toc}

## 前提条件

1.您需要一个 AWS 账户，访问[http://aws.amazon.com](http://aws.amazon.com)获得。
2.安装并配置 [AWS 命令行界面](http://aws.amazon.com/cli)。
3.你需要一个拥有 EC2 全部权限的 AWS [实例配置文件和角色](http://docs.aws.amazon.com/IAM/latest/UserGuide/instance-profiles.html)。

注：这个脚本默认使用“默认”的 AWS 实例配置文件。
您可以使用`AWS_DEFAULT_PROFILE`环境变量来明确地配置 AWS 实例配置文件：

```shell
export AWS_DEFAULT_PROFILE=myawsprofile
```

## 启动集群

### 支持程序: `get-kube`

```shell
#使用 wget
export KUBERNETES_PROVIDER=aws; wget -q -O - https://get.k8s.io | bash
#使用 cURL
export KUBERNETES_PROVIDER=aws; curl -sS https://get.k8s.io | bash
```

注：这个脚本调用[cluster/kube-up.sh](http://releases.k8s.io/{{page.githubbranch}}/cluster/kube-up.sh), 而[cluster/kube-up.sh](http://releases.k8s.io/{{page.githubbranch}}/cluster/kube-up.sh)反过来使用[cluster/aws/config-default.sh](http://releases.k8s.io/{{page.githubbranch}}/cluster/aws/config-default.sh)调用[cluster/aws/util.sh](http://releases.k8s.io/{{page.githubbranch}}/cluster/aws/util.sh)。

这个过程需要约5至10分钟。一旦集群启动，你的主虚拟机和节点虚拟机的IP地址将被打印，
同样地，有关运行在集群中的默认服务（监控，日志，DNS）的信息也会被打印。
用户凭据和安全令牌都写在`~/.kube/config`中，在使用CLI或HTTP基本认证时需要用到它。

默认情况下，该脚本将会使用在`us-west-2a`(俄勒冈州)创建一个新的VPC和一个基于 Ubuntu 四节点的k8s集群。
您可以根据下面的文本，重写定义在[config-default.sh](http://releases.k8s.io/{{page.githubbranch}}/cluster/aws/config-default.sh)中的变量来改变这种默认的配置：

```shell
export KUBE_AWS_ZONE=eu-west-1c
export NUM_NODES=2
export MASTER_SIZE=m3.medium
export NODE_SIZE=m3.medium
export AWS_S3_REGION=eu-west-1
export AWS_S3_BUCKET=mycompany-kubernetes-artifacts
export INSTANCE_PREFIX=k8s
...
```

If you don't specify master and minion sizes, the scripts will attempt to guess
the correct size of the master and worker nodes based on `${NUM_NODES}`. In
version 1.2 these default are:

* For the master, for clusters of less than 150 nodes it will use an
  `m3.medium`, for clusters of greater than 150 nodes it will use an
  `m3.large`.

* For worker nodes, for clusters less than 50 nodes it will use a `t2.micro`,
  for clusters between 50 and 150 nodes it will use a `t2.small` and for
  clusters with greater than 150 nodes it will use a `t2.medium`.

WARNING: beware that `t2` instances receive a limited number of CPU credits per hour and might not be suitable for clusters where the CPU is used
consistently. As a rough estimation, consider 15 pods/node the absolute limit a `t2.large` instance can handle before it starts exhausting its CPU credits
steadily, although this number depends heavily on the usage.

In prior versions of Kubernetes, we defaulted the master node to a t2-class
instance, but found that this sometimes gave hard-to-diagnose problems when the
master ran out of memory or CPU credits.  If you are running a test cluster
and want to save money, you can specify `export MASTER_SIZE=t2.micro` but if
your master pauses do check the CPU credits in the AWS console.

For production usage, we recommend at least `export MASTER_SIZE=m3.medium` and
`export NODE_SIZE=m3.medium`.  And once you get above a handful of nodes, be
aware that one m3.large instance has more storage than two m3.medium instances,
for the same price.

We generally recommend the m3 instances over the m4 instances, because the m3
instances include local instance storage.  Historically local instance storage
has been more reliable than AWS EBS, and performance should be more consistent.
The ephemeral nature of this storage is a match for ephemeral container
workloads also!

If you use an m4 instance, or another instance type which does not have local
instance storage, you may want to increase the `NODE_ROOT_DISK_SIZE` value,
although the default value of 32 is probably sufficient for the smaller
instance types in the m4 family.

该脚本也会尝试创建或者复用名为 “kubernetes” 的密钥对和名为 “kubernetes-master” 及 “kubernetes-minion” 的 IAM 文件。如果这些文件已经存在，请确保您想要在这里使用它们。

注：如果使用已存在的 “kubernetes” 密钥对，那么您必须设置 `AWS_SSH_KEY` 密钥为您的私有密钥。

### 后备方案

CoreOS 维护的 [一个 CLI 工具](https://coreos.com/kubernetes/docs/latest/kubernetes-on-aws.html)， `kube-aws` 可以管理一个基于 [CoreOS](http://www.coreos.com) 的 Kubernetes 集群， 用 AWS 工具: EC2, 自动创建云以及自动扩容。

## 开始使用您的集群

### 命令行管理工具： `kubectl`

集群启动脚本将会在您的工作站留下一个`kubernetes`目录。
可以与之替代的是，您还可以从[这个页面](https://github.com/kubernetes/kubernetes/releases)下载最新的 Kubernetes 发行版。

接下来，在`PATH`中添加适当的二进制文件夹，以便可以访问 kubectl：

```shell
# OS X
export PATH=<path/to/kubernetes-directory>/platforms/darwin/amd64:$PATH

# Linux
export PATH=<path/to/kubernetes-directory>/platforms/linux/amd64:$PATH
```

此工具的最新文档页面可以在这里找到：[kubectl manual](/docs/user-guide/kubectl/kubectl)。

默认情况下，`kubectl`将使用集群启动时生成的`kubeconfig`文件对 API 进行身份验证。更多相关信息,请阅读[kubeconfig 文件](/docs/user-guide/kubeconfig-file)。

By default, `kubectl` will use the `kubeconfig` file generated during the cluster startup for authenticating against the API.
For more information, please read [kubeconfig files](/docs/user-guide/kubeconfig-file)

### 示例

看一个[简单的 nginx 示例](/docs/user-guide/simple-nginx)，尝试使用一下你的新集群。

“Guestbook” 应用程序是另外一个流行的 Kubernetes 入门示例: [guestbook 例子](https://github.com/kubernetes/kubernetes/tree/{{page.githubbranch}}/examples/guestbook/)。

更多完整的应用程序,请查看[示例目录](https://github.com/kubernetes/kubernetes/tree/{{page.githubbranch}}/examples/)。

## 拆除集群

确保您用来提供给集群的环境变量仍在输出，然后调用下面`kubernetes`目录中的脚本：

```shell
cluster/kube-down.sh
```

## 支持级别


IaaS 提供商       | Config. Mgmt | OS     | 网络  | 文档                                              | Conforms | 支持级别
-------------------- | ------------ | ------ | ----------  | ---------------------------------------------     | ---------| ----------------------------
AWS                  | CoreOS       | CoreOS | flannel     | [docs](/docs/getting-started-guides/aws)                                 |          | Community
AWS                  | Saltstack    | Ubuntu | OVS         | [docs](/docs/getting-started-guides/aws)                                    |          | Community ([@justinsb](https://github.com/justinsb))

所有方案的支持级别请参阅[方案表格](/docs/getting-started-guides/#table-of-solutions) .

## 补充阅读

更多关于管理和使用 Kubernetes 集群的细节请参见[Kubernetes 文档](/docs/) 。
