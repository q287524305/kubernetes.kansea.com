---
---

## kubectl rollout

rollout manages a deployment

### 摘要


Manages a deployment using subcommands like "kubectl rollout undo deployment/abc"

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

* [kubectl](kubectl.md)	 - 使用kubectl来管理Kubernetes集群。
* [kubectl rollout history](kubectl_rollout_history.md)	 - view rollout history
* [kubectl rollout pause](kubectl_rollout_pause.md)	 - Mark the provided resource as paused
* [kubectl rollout resume](kubectl_rollout_resume.md)	 - Resume a paused resource
* [kubectl rollout status](kubectl_rollout_status.md)	 - Watch rollout status until it's done
* [kubectl rollout undo](kubectl_rollout_undo.md)	 - undoes a previous rollout
