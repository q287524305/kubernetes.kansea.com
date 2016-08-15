---
---

## kubectl cluster-info

显示集群信息。

### 摘要


显示 master 节点，和带有`kubernetes.io/cluster-service=true`标签的 services 的地址。
进一步调试和诊断集群，请使用'kubectl cluster-info dump'。

```
kubectl cluster-info
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](kubectl.md)	 - 使用kubectl来管理Kubernetes集群。
