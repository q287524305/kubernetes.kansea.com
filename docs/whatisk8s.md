---
assignees:
- bgrant0607
- mikedanese

---


Kubernetes [是一个用于容器集群的自动化部署、扩容以及运维的开源平台。](http://www.slideshare.net/BrianGrant11/wso2con-us-2015-kubernetes-a-platform-for-automating-deployment-scaling-and-operations)

使用Kubernetes，你可以快速高效地响应客户需求：

 - 快速并且无意外的部署你的应用。
 - 动态地对应用进行扩容。
 - 无缝地发布新特性。
 - 仅使用需要的资源以优化硬件使用。

我们希望培育出一个组件及工具的生态，帮助大家减轻在公有云及私有云上运行应用的负担。

#### Kubernetes 是:

* **可移植的**：公有，私有，混合，多重云（multi-cloud）
* **可扩展的**：模块化, 插件化, 可挂载, 可组合
* **可自愈的**：自动布置, 自动重启, 自动复制, 自动扩容

Kubernetes项目是Google在2014年启动的。Kubernetes构建在[Google公司十几年的大规模高负载运维经验](https://research.google.com/pubs/pub43438.html)之上，同时结合了社区中各项最佳设计和实践。

##### 准备好[开始](/docs/getting-started-guides/)了吗？

## 为何要使用容器技术？

想了解为何要使用[容器](http://aucouranton.com/2014/06/13/linux-containers-parallels-lxc-openvz-docker-and-more/)技术？

![为何要使用容器技术?](/images/docs/why_containers.svg)

*Old Way* 是在主机上，通过包管理器安装应用程序来部署。这样做的缺点是，必将涉及到系统的应用程序，配置，库，以及生命周期等问题。
为了构建一个能够更新和回滚的虚拟机镜像，虚拟机会变得非常笨重。

*New Way* 是基于操作系统级虚拟化，而不是硬件虚拟化的容器来部署。这些容器彼此隔离：它们有自己的文件系统，也不能访问对方的程序，他们的运算和资源都是隔离的。
它们比虚拟机更容易建立，并且因为它们与底层系统没有耦合关系，所以可以很方便的在所有云服务器上分发。


由于容器是小而快，每个应用程序可以装在一个容器镜像里。是1对1的应用程序到容器镜像的关系。基于容器技术，镜像可以在编译或者发行时创建，而不是部署的时候才去创建。 因为每个应用程序不依赖于其他的程序，及系统基础环境。
在构建/发行容器的时候就保证了，开发环境和生产环境的一致性。同样，容器是远远比虚拟机更加透明化，这更加方便监控和管理。
当容器里程序的生命周期是由底层系统管理的，而不是容器内部黑盒管理。最后，管理每个单个应用程序的容器，和管理应用程序是一样的。

下面是一些关键点:

* **灵活的创建和部署应用**:
    相对使用虚拟机镜像，容器镜像的创建更加轻巧高效。
* **持续开发，持续集成以及持续部署**:
    提供频繁可靠地构建和部署容器镜像的能力，同时可以快速简单地回滚(因为镜像是固化的)。
* **开发和运维的关注点分离**:
    提供构建和部署的分离；这样也就将应用从基础设施中解耦。
* **开发，测试，生产环境保持高度一致**:
    无论是在笔记本电脑还是服务器上，都采用相同方式运行。
* **兼容不同的云平台或操作系统上**:
    可运行于Ubuntu，RHEL，CoreOS，on-prem或者Google Container Engine，或者任何其他环境.
* **以应用程序为中心的管理**:
    将抽象级别从在虚拟硬件上运行操作系统上升到了在使用特定逻辑资源的操作系统上运行应用程序。
* **松耦合，分布式，弹性，自由的[微服务](http://martinfowler.com/articles/microservices.html)**:
    应用被分割为若干独立的小型程序，可以被动态地部署和管理 -- 而不是一个运行在单机上的超级臃肿的大程序。
* **资源分离**:
    带来可预测的程序性能。
* **资源利用**:
    高性能，大容量。

#### 为什么需要 Kubernetes，它能做什么?

Kubernetes不仅仅只能调度编排物理机或者虚拟机上的容器， 然而，Kubernetes 还可允许开发者剪断与物理机或者虚拟机的脐带，从 **以主机为中心** 的基础设施转移到一个以 **容器为中心** 的基础设施上，继承容器的所有优势和好处。
Kubernetes 提供了一个，能够建立真正 **以容器为中心的** 开发环境的基础设施。

Kubernetes 可以满足一般生产环境的需求, 例如:

* [协同定位辅助](/docs/user-guide/pods/), 复合应用，和单独应用容器模型,
* [装在储存视同](/docs/user-guide/volumes/),
* [安全分发](/docs/user-guide/secrets/),
* [健康检查](/docs/user-guide/production-pods/#liveness-and-readiness-probes-aka-health-checks),
* [replicating application](/docs/user-guide/replication-controller/),
* [横向扩容](/docs/user-guide/horizontal-pod-autoscaling/),
* [命名和查找](/docs/user-guide/connecting-applications/),
* [负载均衡](/docs/user-guide/services/),
* [滚动更新](/docs/user-guide/update-demo/),
* [资源监控](/docs/user-guide/monitoring/),
* [进出站日志](/docs/user-guide/logging/),
* [自检和调试](/docs/user-guide/introspection-and-debugging/), and
* [权限](/docs/admin/authorization/).

提供了具有灵活的 IaaS 服务的 PaaS 平台，让你的应用提高了可移植性，不再依赖基础设施。

更多详细内容，请看[用户指南](/docs/user-guide/).

#### Kubernetes 是一个什么样的平台，为什么这么设计呢?

尽管 Kubernetes 提供了很多种解决方案，但总会有一些意想不到的应用场景。
往往一开始需要更强大的自动化特定编排，可以让应用程序开发流程简单化，加速开发速度。这也是为什么 Kubernetes 设计成为用于组件构建和工具组成的生态系统，使之便于部署，扩展和管理应用程序的一个平台。

[Labels](/docs/user-guide/labels/) 可以让用户随心所欲的来组织他们的资源。
[Annotations](/docs/user-guide/annotations/) 能够让用户自自定义布置资源信息,以方便他们的工作流程，并提供管理工具，让检查变得更简单。

此外, [Kubernetes 控制面板](/docs/admin/cluster-components) 可以让开发人员用相同的 [APIs](/docs/api/) 来构建。
用户可以编写自己的控制器，调度等。
如果选择使用[他们自己的 APIs](https://github.com/kubernetes/kubernetes/blob/{{page.githubbranch}}/docs/design/extending-api.md),可以通过[命令行工具](/docs/user-guide/kubectl-overview/)去实现。

这样的[设计](https://github.com/kubernetes/kubernetes/blob/{{page.githubbranch}}/docs/design/principles.md)使其他系统可以打造在 Kubernetes 之上。

#### Kubernetes 不是:

Kubernetes 不是传统的 PaaS(平台及服务)系统. 我们觉得让用户选择是更重要的.

* Kubernetes并不限制任何应用程序。 也不指定应用框架(例如， [Wildfly](http://wildfly.org/))，限制语言类型(例如， Java, Python, Ruby)，也不仅仅迎合 [12-factor应用程序](http://12factor.net/)模式. Kubernetes旨在支持各种多种多样的负载类型：只要一个程序能够在容器中运行，它就可以在Kubernetes中运行。
* Kubernetes不提供中间件（例如，消息总线），数据处理框架（例如，Spark），数据库（例如，mysql），也不群集存储系统（例如，Ceph）。
* Kubernetes 没有一键部署服务
* Kubernetes is unopinionated in the source-to-image space. It does not deploy source code and does not build your application. Continuous Integration (CI) workflow is an area where different users and projects have their own requirements and preferences, so we support layering CI workflows on Kubernetes but don't dictate how it should work.
* Kubernetes allows users to choose the logging, monitoring, and alerting systems of their choice. (Though we do provide some integrations as proof of concept.)
* Kubernetes does not provide nor mandate a comprehensive application configuration language/system (e.g., [jsonnet](https://github.com/google/jsonnet)).
* Kubernetes does not provide nor adopt any comprehensive machine configuration, maintenance, management, or self-healing systems.

另一方面， 确实有不少PaaS系统运行在Kubernetes之上，比如[Openshift](https://github.com/openshift/origin)， [Deis](http://deis.io/)和[Gondor](https://gondor.io/)。同样你也可以将定制的PaaS系统，结合一个持续集成系统再Kubernetes上进行实施：只需生成容器镜像并通过Kubernetes部署。

由于Kubernetes运行再应用层而不是硬件层，所以它提供了一些一般PaaS提供的功能，比如部署，扩容，负载均衡，日志，监控，等等。无论如何，Kubernetes不是一个单一应用，所以这些解决方案都是可选可插拔的。

Kubernetes 并不是单单的"编排系统"；它排除了对编排的需要:
“编排”的技术定义为按照指定流程执行一系列动作：执行A，然后B，然后C。相反，Kubernetes有一系列控制进程组成，持续地控制从当前状态到指定状态的流转。无需关注你是如何从A到C：只需结果如此。这样将使得系统更加易用，强大，健壮和弹性。

#### *Kubernetes* 这个名字是什么意思？k8s又是什么？

**Kubernetes** 这个名字源自希腊语，意思是“舵手”，也是“管理者”，“治理者”和["cybernetic"](http://www.etymonline.com/index.php?term=cybernetics)的源头。**K8s** 是Kubernetes的简称（用数字『8』替代中间的8个字母『ubernete』）。
