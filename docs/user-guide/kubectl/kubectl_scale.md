---
assignees:
- bgrant0607
- janetkuo

---

## kubectl scale

Set a new size for a Replication Controller, Job, or Deployment.

### 摘要


Set a new size for a Replication Controller, Job, or Deployment.

Scale also allows users to specify one or more preconditions for the scale action.
If --current-replicas or --resource-version is specified, it is validated before the
scale is attempted, and it is guaranteed that the precondition holds true when the
scale is sent to the server.

```
kubectl scale [--resource-version=version] [--current-replicas=count] --replicas=COUNT (-f FILENAME | TYPE NAME)
```

### 示例

```
# Scale replication controller named 'foo' to 3.
kubectl scale --replicas=3 rc/foo

# Scale a resource identified by type and name specified in "foo.yaml" to 3.
kubectl scale --replicas=3 -f foo.yaml

# If the deployment named mysql's current size is 2, scale mysql to 3.
kubectl scale --current-replicas=2 --replicas=3 deployment/mysql

# Scale multiple replication controllers.
kubectl scale --replicas=5 rc/foo rc/bar rc/baz

# Scale job named 'cron' to 3.
kubectl scale --replicas=3 job/cron
```

### 选项

```
      --current-replicas=-1: Precondition for current size. Requires that the current size of the resource match this value in order to scale.
  -f, --filename=[]: Filename, directory, or URL to a file identifying the resource to set a new size
  -o, --output="": Output mode. Use "-o name" for shorter output (resource/name).
      --record[=false]: Record current kubectl command in the resource annotation.
      --replicas=-1: The new desired number of replicas. Required.
      --resource-version="": Precondition for resource version. Requires that the current resource version match this value in order to scale.
      --timeout=0: The length of time to wait before giving up on a scale operation, zero means don't wait.
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。


