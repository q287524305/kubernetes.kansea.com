---
---

## kubectl apply

Apply a configuration to a resource by filename or stdin

### 摘要


Apply a configuration to a resource by filename or stdin.
The resource will be created if it doesn't exist yet.

JSON and YAML formats are accepted.

```
kubectl apply -f FILENAME
```

### 示例

```
# Apply the configuration in pod.json to a pod.
kubectl apply -f ./pod.json

# Apply the JSON passed into stdin to a pod.
cat pod.json | kubectl apply -f -
```

### 选项

```
  -f, --filename=[]: Filename, directory, or URL to file that contains the configuration to apply
  -o, --output="": Output mode. Use "-o name" for shorter output (resource/name).
      --record[=false]: Record current kubectl command in the resource annotation.
      --schema-cache-dir="~/.kube/schema": If non-empty, load/store cached API schemas in this directory, default is '$HOME/.kube/schema'
      --validate[=true]: If true, use a schema to validate the input before sending it
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。


