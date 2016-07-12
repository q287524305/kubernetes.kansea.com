---
---

* TOC
{:toc}

用户指南是为所有想在已经运行着的Kubernetes集群上运行程序或者服务准备的。Kubernetes集群的设置和管理可以参见[Cluster Admin Guide](/docs/admin/)。而[Developer Guide](https://github.com/kubernetes/kubernetes/tree/{{page.githubbranch}}/docs/devel/) 是为所有想要用Kubernetes API来编写代码的工程师或者想直接为Kubernetes贡献代码的人准备的。

请确保你已经阅读完毕[运行用户指南中的示例的必读事项](/docs/user-guide/prereqs).

## 快速指南

1. [Kubernetes 101](/docs/user-guide/walkthrough/)
1. [Kubernetes 201](/docs/user-guide/walkthrough/k8s201/)

## 详细指南

如果你对Kubernetes的熟悉程度还不够，我们建议您按如下的顺序阅读指南：

1. [快速开始：启动并开放一个应用](/docs/user-guide/quick-start)
1. [容器的配置与启动：常用容器参数的配置](/docs/user-guide/configuring-containers)
1. [运行中应用的持续部署](/docs/user-guide/deploying-applications)
1. [应用的连接：将应用开放给客户端和用户](/docs/user-guide/connecting-applications)
1. [与生产环境中的容器打交道](/docs/user-guide/production-pods)
1. [部署的管理](/docs/user-guide/managing-deployments)
1. [应用的检视与调试](/docs/user-guide/introspection-and-debugging)
    1. [使用Kubernetes的Web用户界面](/docs/user-guide/ui)
    1. [日志](/docs/user-guide/logging)
    1. [监控](/docs/user-guide/monitoring)
    1. [通过`exec`切入容器](/docs/user-guide/getting-into-containers)
    1. [通过代理连接容器](/docs/user-guide/connecting-to-applications-proxy)
    1. [通过端口转发连接容器](/docs/user-guide/connecting-to-applications-port-forward)

## 概念指南

[**Cluster**](/docs/admin/)
: Cluster是一组物理的或者虚拟的机器，也包含其他运行Kubernetes所需的基础设施资源

[**Node**](/docs/admin/node/)
: 一个Node是一个物理的或者虚拟的运行Kubernetes的机器，Pod就是在Node上面进行调度的

[**Pod**](/docs/user-guide/pods/)
: 一个Pod是放置在一起的一组容器和卷

[**Label**](/docs/user-guide/labels/)
: Label指的是一个可以附加在资源上的键值对，例如可以附加在Pod上来传达一个用户定义的具有辨识性的属性。Label可以用来组织资源，或者选取资源的特定子集。

[**Selector**](/docs/user-guide/labels/#label-selectors)
: Selector是一个匹配Label的表达式，目的是用来辨识相关的资源，如一个负载均衡的服务的目标Pod是哪些。

[**Replication Controller**](/docs/user-guide/replication-controller/)
: Replication Controller是用来确保在同一时间有指定数量的Pod副本正在运行。它不但让伸缩变得简单，同时让在机器重启或者因为其他原因出故障的时候对一个Pod进行重建。

[**Service**](/docs/user-guide/services/)
: Service定义了一组Pod和一个访问这组Pod的方式，例如一个稳定的IP地址和相应的DNS名称。

[**Volume**](/docs/user-guide/volumes/)
: 卷是一个目录，里面很可能包含数据，可以被容器作为自己文件系统的一部分进行访问。Kubernetes的卷基于[Docker Volumes](https://docs.docker.com/userguide/dockervolumes/)构建而来，并且添加了volume directory和/或device的设置。

[**Secret**](/docs/user-guide/secrets/)
: Secret里面保存敏感的数据，例如用于认证的令牌，可以在请求的时候为容器所用。

[**Name**](/docs/user-guide/identifiers/)
: 一个用户或者客户端提供的资源名称。

[**Namespace**](/docs/user-guide/namespaces/)
: 命名空间就像一个资源名称的前缀。命名空间能通过如防止不相关的项目组发生命名冲突的措施,帮助不同的项目，项目组，或者客户来共享集群。

[**Annotation**](/docs/user-guide/annotations/)
: 可以用来保存更大（相比较于label）的键值对，并且可能包含可读性低的数据，目的是用来保存非辨认性目的的数据，特别是那些由工具和系统扩展操作的数据。annotation的值无法用来进行有效地过滤。

## 深入阅读

API 资源

  * [使用Resouce](/docs/user-guide/working-with-resources)

Pods 和容器

  * [Pod的生命周期和重启策略](/docs/user-guide/pod-states)
  * [生命周期中的钩子](/docs/user-guide/container-environment)
  * [计算资源，如CPU和内存](/docs/user-guide/compute-resources)
  * [指定命令和请求能力](/docs/user-guide/containers)
  * [Downward API: 从Pod中访问系统配置](/docs/user-guide/downward-api)
  * [镜像和注册表](/docs/user-guide/images)
  * [从docker-cli到kubectl的迁移](/docs/user-guide/docker-cli-to-kubectl)
  * [配置的相关提示和小技巧](/docs/user-guide/config-best-practices)
  * [向指定的Node分配Pod](/docs/user-guide/node-selection/)
  * [在一组正在运行的Pod执行滚动更新](/docs/user-guide/update-demo/)
