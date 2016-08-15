---
---

## kubectl rollout history

view rollout history

### 摘要


View previous rollout revisions and configurations.

```
kubectl rollout history (TYPE NAME | TYPE/NAME) [flags]
```

### 示例

```
# View the rollout history of a deployment
kubectl rollout history deployment/abc

# View the details of deployment revision 3
kubectl rollout history deployment/abc --revision=3
```

### 选项

```
  -f, --filename=[]: Filename, directory, or URL to a file identifying the resource to get from a server.
  -R, --recursive[=false]: Process the directory used in -f, --filename recursively. Useful when you want to manage related manifests organized within the same directory.
      --revision=0: See the details, including podTemplate of the revision specified
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl rollout](kubectl_rollout.md)	 - rollout manages a deployment
