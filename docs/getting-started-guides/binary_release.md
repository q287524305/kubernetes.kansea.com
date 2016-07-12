---
---

你可以从代码编译或者下载一个预发布版。如果你不打算自己开发 Kubernetes，那么我建议你下载一个预发布版。

如果你仅仅想要在本地运行 Kubernetes，那么我们推荐你使用 Minikube。你可以在[这](https://github.com/kubernetes/minikube/releases/latest)下载 Minikube。
Minikube 可以确保你很容易地在本地搭建起 Kubernetes 群集。

* TOC
{:toc}

### 预发布版

你可以从 [GitHub Kubernetes 版本库](https://github.com/kubernetes/kubernetes/releases)下载二进制版。

在 Linux 或 macOS 下载最新的版本并解压缩，切换到 `kubernetes/` 目录下然后你就可以按照用户指南来搭建你的私有云了。

macOS 上你也可以用 [homebrew](http://brew.sh/) : `brew install kubernetes-cli`

### 编译

获取 Kubernetes 源代码。如果你仅仅要用源代码编译一次，那么你没必要搭建一个完整的 Golang 环境，使用 Docker 容器就可以了。

编译非常简单。

```shell
git clone https://github.com/kubernetes/kubernetes.git
cd kubernetes
make release
```

更多详细内容请参阅[`build/` 文件夹](http://releases.k8s.io/{{page.githubbranch}}/build/)

### 下载 Kubernetes 并且自动设置一个群集

`https://get.k8s.io` 这个脚本可以使用 `wget` 或者 `curl` 来自动下载 Kubernetes，并在你的私有云中创建群集。

```shell
# wget version
export KUBERNETES_PROVIDER=YOUR_PROVIDER; wget -q -O - https://get.k8s.io | bash

# curl version
export KUBERNETES_PROVIDER=YOUR_PROVIDER; curl -sS https://get.k8s.io | bash
```

`YOUR_PROVIDER`支持一下服务:

* `gce` - Google Compute Engine [default]
* `gke` - Google Container Engine
* `aws` - Amazon EC2
* `azure` - Microsoft Azure
* `vagrant` - Vagrant (on local virtual machines)
* `vsphere` - VMWare VSphere
* `rackspace` - Rackspace

通过这个脚本来完善更多服务商，, 看 [Kubernetes 代码库的 `/cluster` 文件夹](https://github.com/kubernetes/kubernetes/tree/{{page.githubbranch}}/cluster)， 其中每一个文件夹代表一个`YOUR_PROVIDER`支持的服务商。
如果没找到你的服务商，你可以查看[入门指南](/docs/getting-started-guides); 那会有我们为他们准备的文档。
