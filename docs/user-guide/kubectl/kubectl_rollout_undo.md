---
---

## kubectl rollout undo

undoes a previous rollout

### 摘要


undo rolls back to a previous rollout.

```
kubectl rollout undo (TYPE NAME | TYPE/NAME) [flags]
```

### 示例

```
# Rollback to the previous deployment
kubectl rollout undo deployment/abc
```

### 选项

```
  -f, --filename=[]: Filename, directory, or URL to a file identifying the resource to get from a server.
      --to-revision=0: The revision to rollback to. Default to 0 (last revision).
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl rollout](/docs/user-guide/kubectl/kubectl_rollout/)	 - rollout manages a deployment


