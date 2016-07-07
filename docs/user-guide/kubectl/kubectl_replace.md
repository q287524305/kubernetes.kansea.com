---
---

## kubectl replace

Replace a resource by filename or stdin.

### 摘要


Replace a resource by filename or stdin.

JSON and YAML formats are accepted. If replacing an existing resource, the
complete resource spec must be provided. This can be obtained by
$ kubectl get TYPE NAME -o yaml

Please refer to the models in https://htmlpreview.github.io/?https://github.com/kubernetes/kubernetes/blob/release-1.2/docs/api-reference/v1/definitions.html to find if a field is mutable.

```
kubectl replace -f FILENAME
```

### 示例

```
# Replace a pod using the data in pod.json.
kubectl replace -f ./pod.json

# Replace a pod based on the JSON passed into stdin.
cat pod.json | kubectl replace -f -

# Update a single-container pod's image version (tag) to v4
kubectl get pod mypod -o yaml | sed 's/\(image: myimage\):.*$/\1:v4/' | kubectl replace -f -

# Force replace, delete and then re-create the resource
kubectl replace --force -f ./pod.json
```

### 选项

```
      --cascade[=false]: Only relevant during a force replace. If true, cascade the deletion of the resources managed by this resource (e.g. Pods created by a ReplicationController).
  -f, --filename=[]: Filename, directory, or URL to file to use to replace the resource.
      --force[=false]: Delete and re-create the specified resource
      --grace-period=-1: Only relevant during a force replace. Period of time in seconds given to the old resource to terminate gracefully. Ignored if negative.
  -o, --output="": Output mode. Use "-o name" for shorter output (resource/name).
      --record[=false]: Record current kubectl command in the resource annotation.
      --save-config[=false]: If true, the configuration of current object will be saved in its annotation. This is useful when you want to perform kubectl apply on this object in the future.
      --schema-cache-dir="~/.kube/schema": If non-empty, load/store cached API schemas in this directory, default is '$HOME/.kube/schema'
      --timeout=0: Only relevant during a force replace. The length of time to wait before giving up on a delete of the old resource, zero means determine a timeout from the size of the object
      --validate[=true]: If true, use a schema to validate the input before sending it
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。


