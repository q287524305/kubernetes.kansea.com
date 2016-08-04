---
assignees:
- bgrant0607
- dchen1107

---

## kubectl explain

Documentation of resources.

### 摘要


Documentation of resources.

Possible resource types include: pods (po), services (svc),
replicationcontrollers (rc), nodes (no), events (ev), componentstatuses (cs),
limitranges (limits), persistentvolumes (pv), persistentvolumeclaims (pvc),
resourcequotas (quota), namespaces (ns), horizontalpodautoscalers (hpa)
or endpoints (ep).

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
      --recursive[=false]: Print the fields of fields (Currently only 1 level deep)
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。


