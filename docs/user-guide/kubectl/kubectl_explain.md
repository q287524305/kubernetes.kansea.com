---
---

## kubectl explain

Documentation of resources.

### 摘要


Documentation of resources.

Possible resource types include (case insensitive): pods (po), services (svc), deployments,
replicasets (rs), replicationcontrollers (rc), nodes (no), events (ev), limitranges (limits),
persistentvolumes (pv), persistentvolumeclaims (pvc), resourcequotas (quota), namespaces (ns),
serviceaccounts (sa), ingresses (ing), horizontalpodautoscalers (hpa), daemonsets (ds), configmaps,
componentstatuses (cs), endpoints (ep), and secrets.

```
kubectl explain RESOURCE
```

### 示例

```
# Get the documentation of the resource and its fields
kubectl explain pods

# Get the documentation of a specific field of a resource
kubectl explain pods.spec.containers
```

### 选项

```
      --include-extended-apis[=true]: If true, include definitions of new APIs via calls to the API server. [default true]
      --recursive[=false]: Print the fields of fields (Currently only 1 level deep)
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](kubectl.md)	 - 使用kubectl来管理Kubernetes集群。
