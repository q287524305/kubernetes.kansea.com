---
---

通过 Kubernetes 部署和管理应用，你需要使用 Kubernetes命令行工具， [kubectl](/docs/user-guide/kubectl/kubectl/). 它可以让您检查集群资源，创建、删除和更新组件以及更多功能。您会用它来查看新集群并生成示例应用程序。

## 安装 kubectl

如果你下载了一个预编译 [发布版](https://github.com/kubernetes/kubernetes/releases), kubectl 应该在tar压缩包的 `platforms/<os>/<arch>` 下面。

如果从源码编译，kubectl 也应该在 `_output/local/bin/<os>/<arch>` 或者 `_output/dockerized/bin/<os>/<arch>`之下。

不必让 kubectl 二进制文件可执行，但用户指南中会假设它在你的 PATH 中。

最简单的安装方法就是复制或者移动 kubectl 到 PATH (e.g. `/usr/local/bin`)其中的一个文件夹下。 例如:

```shell
# OS X
$ sudo cp kubernetes/platforms/darwin/amd64/kubectl /usr/local/bin/kubectl
# Linux
$ sudo cp kubernetes/platforms/linux/amd64/kubectl /usr/local/bin/kubectl
```

还需要将它改为可执行的:

```shell
$ sudo chmod +x /usr/local/bin/kubectl
```

如果你不愿意复制 kubectl，那么你需要确保 kubectl 工具是在您的 path 中:

```shell
# OS X
export PATH=<path/to/kubernetes-directory>/platforms/darwin/amd64:$PATH

# Linux
export PATH=<path/to/kubernetes-directory>/platforms/linux/amd64:$PATH
```

## 配置 kubectl

为了让 kubectl 能够找到并能够访问 Kubernetes 群集，它需要一个[kubeconfig 文件](/docs/user-guide/kubeconfig-file)，使用`kube-up.sh`创建群集的时候会自动创建它。 (有关创建集群请看 [用户指南](/docs/getting-started-guides/))。
如果你需要访问一个不是你创建的群集，请看 [访问共享集群文档](/docs/user-guide/sharing-clusters)。
默认情况下，kubectl 配置文件会在`~/.kube/config`。

#### 确保你已经准备好了

通过获取群集状态来检测 kubectl 是否正确配置:

```shell
$ kubectl cluster-info
```

如果看到了一个网址，那么 OK 了。

## 接下来是什么?

[学习如何启动和公开你的应用程序。](/docs/user-guide/quick-start)
