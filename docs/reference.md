---

---
在参考文档中，你可以找到 Kubernetes API CLIs 和工具的文档，以及资料库和设计文档。

## API 参考文档

* [Kubernetes API](/docs/api/) - Kubernetes 核心 API。
* [Extensions API](/docs/api-reference/extensions/v1beta1/operations/) - 管理类似于 Jobs，Ingress 和 HorizontalPodAutoscalers 的扩展资源，。

## CLI 参考文档

* [kubectl](/docs/user-guide/kubectl-overview/) - 在 Kubernetes 集群运行命令.
	* [JSONPath](/docs/user-guide/jsonpath/) - 格式 	通过 kubectl 使用 [JSONPath expressions](http://goessner.net/articles/JsonPath/) 的指南。
* [kube-apiserver](/docs/admin/kube-apiserver/) -  验证和配置API 对象的的 REST API。例如pods, services, replication controllers 。
* [kube-proxy](/docs/admin/kube-proxy/) - 可以做简单的 TCP/UDP 流的代理，或者在一组后端 TCP/UDP 轮询调度的代理。
* [kube-scheduler](/docs/admin/kube-scheduler/) - 一个丰富的策略，拓扑感知，特定工作负载的方法，具有杰出的可用性，性能和潜力。
* [kubelet](/docs/admin/kubelet/) - 在每个 node 运行的主 "node agent"。kubelet 由一组 PodSpecs构成，并确保其容器运行和健康。

## 术语

探索 Kubernetes 基本概念的词汇。一些好的起点 [Pods](/docs/user-guide/pods/), [Nodes](/docs/admin/nodes/), [Services](/docs/user-guide/services/), 和 [Replication Controllers](/docs/user-guide/replication-controller/)。

## 设计文档

Kubernetes 功能的设计文档。不错的起点 [Kubernetes Architecture](https://github.com/kubernetes/kubernetes/blob/release-1.1/docs/design/architecture.md) 和 [Kubernetes Design Overview](https://github.com/kubernetes/kubernetes/tree/release-1.1/docs/design)。
