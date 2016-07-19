---
---

## kubectl patch

Update field(s) of a resource using strategic merge patch.

### 摘要


Update field(s) of a resource using strategic merge patch

JSON and YAML formats are accepted.

Please refer to the models in https://htmlpreview.github.io/?https://github.com/kubernetes/kubernetes/blob/release-1.2/docs/api-reference/v1/definitions.html to find if a field is mutable.

```
kubectl patch (-f FILENAME | TYPE NAME) -p PATCH
```

### 示例

```

# Partially update a node using strategic merge patch
kubectl patch node k8s-node-1 -p '{"spec":{"unschedulable":true}}'

# Partially update a node identified by the type and name specified in "node.json" using strategic merge patch
kubectl patch -f node.json -p '{"spec":{"unschedulable":true}}'

# Update a container's image; spec.containers[*].name is required because it's a merge key
kubectl patch pod valid-pod -p '{"spec":{"containers":[{"name":"kubernetes-serve-hostname","image":"new image"}]}}'

# Update a container's image using a json patch with positional arrays
kubectl patch pod valid-pod --type='json' -p='[{"op": "replace", "path": "/spec/containers/0/image", "value":"new image"}]'
```

### 选项

```
  -f, --filename=[]: Filename, directory, or URL to a file identifying the resource to update
  -o, --output="": Output mode. Use "-o name" for shorter output (resource/name).
  -p, --patch="": The patch to be applied to the resource JSON file.
      --record[=false]: Record current kubectl command in the resource annotation.
      --type="strategic": The type of patch being provided; one of [json merge strategic]
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。


