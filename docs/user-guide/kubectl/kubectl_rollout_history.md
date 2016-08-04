---
assignees:
- bgrant0607
- janetkuo

---

## kubectl rollout history

view rollout history

### 摘要


view previous rollout revisions and configurations.

```
kubectl rollout history (TYPE NAME | TYPE/NAME) [flags]
```

### 示例

```
# View the rollout history of a deployment
kubectl rollout history deployment/abc
```

### 选项

```
  -f, --filename=[]: Filename, directory, or URL to a file identifying the resource to get from a server.
      --revision=0: See the details, including podTemplate of the revision specified
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl rollout](/docs/user-guide/kubectl/kubectl_rollout/)	 - rollout manages a deployment


