---
assignees:
- bgrant0607
- eparis

---

## kubectl delete

Delete resources by filenames, stdin, resources and names, or by resources and label selector.

### 摘要


Delete resources by filenames, stdin, resources and names, or by resources and label selector.

JSON and YAML formats are accepted.

Only one type of the arguments may be specified: filenames, resources and names, or resources and label selector

Note that the delete command does NOT do resource version checks, so if someone
submits an update to a resource right when you submit a delete, their update
will be lost along with the rest of the resource.

```
kubectl delete ([-f FILENAME] | TYPE [(NAME | -l label | --all)])
```

### 示例

```
# Delete a pod using the type and name specified in pod.json.
kubectl delete -f ./pod.json

# Delete a pod based on the type and name in the JSON passed into stdin.
cat pod.json | kubectl delete -f -

# Delete pods and services with same names "baz" and "foo"
kubectl delete pod,service baz foo

# Delete pods and services with label name=myLabel.
kubectl delete pods,services -l name=myLabel

# Delete a pod with UID 1234-56-7890-234234-456456.
kubectl delete pod 1234-56-7890-234234-456456

# Delete all pods
kubectl delete pods --all
```

### 选项

```
      --all[=false]: [-all] to select all the specified resources.
      --cascade[=true]: If true, cascade the deletion of the resources managed by this resource (e.g. Pods created by a ReplicationController).  Default true.
  -f, --filename=[]: Filename, directory, or URL to a file containing the resource to delete.
      --grace-period=-1: Period of time in seconds given to the resource to terminate gracefully. Ignored if negative.
      --ignore-not-found[=false]: Treat "resource not found" as a successful delete. Defaults to "true" when --all is specified.
  -o, --output="": Output mode. Use "-o name" for shorter output (resource/name).
  -l, --selector="": Selector (label query) to filter on.
      --timeout=0: The length of time to wait before giving up on a delete, zero means determine a timeout from the size of the object
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。


