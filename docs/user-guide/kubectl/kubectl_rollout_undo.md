---
---

## kubectl rollout undo

undoes a previous rollout

### 摘要


Rollback to a previous rollout.

```
kubectl rollout undo (TYPE NAME | TYPE/NAME) [flags]
```

### 示例

```
# Rollback to the previous deployment
kubectl rollout undo deployment/abc

# Rollback to deployment revision 3
kubectl rollout undo deployment/abc --to-revision=3
```

### 选项

```
  -f, --filename=[]: Filename, directory, or URL to a file identifying the resource to get from a server.
  -R, --recursive[=false]: Process the directory used in -f, --filename recursively. Useful when you want to manage related manifests organized within the same directory.
      --to-revision=0: The revision to rollback to. Default to 0 (last revision).
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl rollout](kubectl_rollout.md)	 - rollout manages a deployment


