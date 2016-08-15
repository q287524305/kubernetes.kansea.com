---
---

## kubectl create

Create a resource by filename or stdin

### 摘要


Create a resource by filename or stdin.

JSON and YAML formats are accepted.

```
kubectl create -f FILENAME
```

### 示例

```
# Create a pod using the data in pod.json.
kubectl create -f ./pod.json

# Create a pod based on the JSON passed into stdin.
cat pod.json | kubectl create -f -
```

### 选项

```
  -f, --filename=[]: Filename, directory, or URL to file to use to create the resource
      --include-extended-apis[=true]: If true, include definitions of new APIs via calls to the API server. [default true]
  -o, --output="": Output mode. Use "-o name" for shorter output (resource/name).
      --record[=false]: Record current kubectl command in the resource annotation.
  -R, --recursive[=false]: Process the directory used in -f, --filename recursively. Useful when you want to manage related manifests organized within the same directory.
      --save-config[=false]: If true, the configuration of current object will be saved in its annotation. This is useful when you want to perform kubectl apply on this object in the future.
      --schema-cache-dir="~/.kube/schema": If non-empty, load/store cached API schemas in this directory, default is '$HOME/.kube/schema'
      --validate[=true]: If true, use a schema to validate the input before sending it
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](kubectl.md)	 - 使用kubectl来管理Kubernetes集群。
* [kubectl create configmap](kubectl_create_configmap.md)	 - Create a configMap from a local file, directory or literal value.
* [kubectl create namespace](kubectl_create_namespace.md)	 - Create a namespace with the specified name.
* [kubectl create secret](kubectl_create_secret.md)	 - Create a secret using specified subcommand.
* [kubectl create serviceaccount](kubectl_create_serviceaccount.md)	 - Create a service account with the specified name.
