---
---

## kubectl rollout

rollout manages a deployment

### 摘要


rollout manages a deployment using subcommands like "kubectl rollout undo deployment/abc"

```
kubectl rollout SUBCOMMAND
```

### 示例

```
# Rollback to the previous deployment
kubectl rollout undo deployment/abc
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。
* [kubectl rollout history](/docs/user-guide/kubectl/kubectl_rollout_history/)	 - view rollout history
* [kubectl rollout pause](/docs/user-guide/kubectl/kubectl_rollout_pause/)	 - Mark the provided resource as paused
* [kubectl rollout resume](/docs/user-guide/kubectl/kubectl_rollout_resume/)	 - Resume a paused resource
* [kubectl rollout undo](/docs/user-guide/kubectl/kubectl_rollout_undo/)	 - undoes a previous rollout


