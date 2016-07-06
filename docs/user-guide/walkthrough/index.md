---
---

## Kubectl CLI 和 Pods

对于Kubernetes 101，我们将覆盖的内容包括kubectl, pods, volumes, 以及多容器。

为了 kubectl 可以正常运行示例，请确保你已经在本地有示例目录，可以从 [一个发布版](https://github.com/kubernetes/kubernetes/releases) 或者 [源代码](https://github.com/kubernetes/kubernetes)中得到的。

* TOC
{:toc}


## Kubectl CLI

同 Kubernetes 交互的最简单方法是通过[kubectl](/docs/user-guide/kubectl-overview/)命令行界面。
对于 kubectl 的更多信息，包括它的使用方法，命令，参数，请阅读 [kubectl CLI 参考手册](/docs/user-guide/kubectl-overview/).

如果你没有安装好或者配置好 kubectl，在继续阅读之前把 [前提条件](/docs/user-guide/prereqs/) 做完。

## Pods

在 Kubernetes, 一组单个或者多个容器叫做 _pod_。pod中的容器是部署在一起的，并且作为一组来启动，停止和复制。

更多细节请看 [pods](/docs/user-guide/pods/) .


#### Pod 定义

最简单的 pod定义 描述了单个容器的部署。  比如，一个 nginx web 服务器的 pod 可以定义如下：

{% include code.html language="yaml" file="pod-nginx.yaml" ghlink="/docs/user-guide/walkthrough/pod-nginx.yaml" %}

pod 的定义声明了某个_预期状态_。预期状态是 Kubernetes 模型中非常重要的概念。很多事情在系统中体现为预期状态，而 Kubernetes 会确保当前状态匹配预期状态。举个例子，当你创建一个 Pod，你指明其中的容器进入运行。如果容器没有运行（例如程序错误，...），为了驱使容器进入预期状态，Kubernetes 将持续为你（再）创建这些容器。这个过程将一直持续到该Pod被删除。

更多细节信息，请查阅[设计文档](https://github.com/kubernetes/kubernetes/blob/{{page.githubbranch}}/docs/design/README.md)。


#### Pod 管理

创建一个包含 nginx 服务的 pod ([pod-nginx.yaml](/docs/user-guide/walkthrough/pod-nginx.yaml)):

```shell
$ kubectl create -f docs/user-guide/walkthrough/pod-nginx.yaml
```

列出所有 pod:

```shell
$ kubectl get pods
```

大部分环境下，Pod 的 IP 都是外部不可见的。最便捷的测试 Pod 是否工作的方法是创建一个 busybox pod 并且在上面远程运行命令。详细请查看[命令文档](/docs/user-guide/getting-into-containers/)。

如果Pod IP可以访问，你应该可以访问80端口的 http。

```shell{% raw %}
$ kubectl run busybox --image=busybox --restart=Never --tty -i --generator=run-pod/v1 --env "POD_IP=$(kubectl get pod nginx -o go-template={{.status.podIP}})"
u@busybox$ wget -qO- http://$POD_IP # Run in the busybox container
u@busybox$ exit # Exit the busybox container
$ kubectl delete pod busybox # Clean up the pod we created with "kubectl run"
{% endraw %}```

通过名字删除 pod：

```shell
$ kubectl delete pod nginx
```


#### Volumes

这对于一个简单的静态网站服务器很不错，那么对于需要持续性存储情况如何？

容器文件系统仅仅存在于容器的生存周期。所以，如果你的应用状态需要忍受迁移、重启和崩溃，你需要配置一些持续性存储。

在下面的例子中，我们将创建一个 Redis Pod，这个 Pod 包括一个named Volume和包含 Volume 安装路径的 Volume 安装点。

1. 定义一个 volume:

```yaml
volumes:
    - name: redis-persistent-storage
      emptyDir: {}
```

2. 在容器定义内定义一个Volume装载位置:

```yaml
volumeMounts:
    # 名字必须与下面的Volume名字一致
    - name: redis-persistent-storage
      # 容器中的装载位置
      mountPath: /data/redis
```

带有持续存储Volume的Redis Pod定义举例 ([pod-redis.yaml](/docs/user-guide/walkthrough/pod-redis.yaml)):

{% include code.html language="yaml" file="pod-redis.yaml" ghlink="/docs/user-guide/walkthrough/pod-redis.yaml" %}

注意事项:

- `volumeMounts` `name` 是一个特定的  `volumes` `name`。
- `volumeMounts` `mountPath` 是容器中装载的路径。

##### Volume 类型

- **EmptyDir**: 在 node 上创建一个和 pod 同生命周期的文件夹，容器出错或重启都会使用这个文件夹。
- **HostPath**: 挂在文件到一个系统上已存目录上 (例如 `/var/logs`).

更多详细请查看 [volumes](/docs/user-guide/volumes/)。


#### Multiple Containers

_注意:
下面的例子在语义上正确，但是某些镜像（比如 kubernetes/git-monitor）目前还不存在。我们正在努力将这些内容加入到可以工作的例子中。_


然而，通常你会希望存在两种容器在一起工作。一个例子是网站服务器，和一个可以从git仓库中获取更新的帮助任务。

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: www
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - mountPath: /srv/www
      name: www-data
      readOnly: true
  - name: git-monitor
    image: kubernetes/git-monitor
    env:
    - name: GIT_REPO
      value: http://github.com/some/repo.git
    volumeMounts:
    - mountPath: /data
      name: www-data
  volumes:
  - name: www-data
    emptyDir: {}
```

注意我们也在这里增加了一个volume。在这个例子里，这个 volume 同时被安装在两个容器中。在网页服务器容器中，由于并不需要写这个目录，因此被标注为`readOnly`。

最后，我们也引入了一个给`git-monitor`容器使用的环境变量，这个变量允许我们将我们希望跟踪的特定git仓库作为参数使用。

## 接下来是什么?

继续学习 [Kubernetes 201](/docs/user-guide/walkthrough/k8s201) 或者查看一个完整的[guestbook 示例](https://github.com/kubernetes/kubernetes/tree/{{page.githubbranch}}/examples/guestbook/)。
