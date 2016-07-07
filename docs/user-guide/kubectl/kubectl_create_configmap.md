---
---

## kubectl create configmap

Create a configMap from a local file, directory or literal value.

### 摘要


Create a configmap based on a file, directory, or specified literal value.

A single configmap may package one or more key/value pairs.

When creating a configmap based on a file, the key will default to the basename of the file, and the value will
default to the file content.  If the basename is an invalid key, you may specify an alternate key.

When creating a configmap based on a directory, each file whose basename is a valid key in the directory will be
packaged into the configmap.  Any directory entries except regular files are ignored (e.g. subdirectories,
symlinks, devices, pipes, etc).


```
kubectl create configmap NAME [--type=string] [--from-file=[key=]source] [--from-literal=key1=value1] [--dry-run]
```

### 示例

```
  # Create a new configmap named my-config with keys for each file in folder bar
  kubectl create configmap my-config --from-file=path/to/bar

  # Create a new configmap named my-config with specified keys instead of names on disk
  kubectl create configmap my-config --from-file=ssh-privatekey=~/.ssh/id_rsa --from-file=ssh-publickey=~/.ssh/id_rsa.pub

  # Create a new configMap named my-config with key1=config1 and key2=config2
  kubectl create configmap my-config --from-literal=key1=config1 --from-literal=key2=config2
```

### 选项

```
      --dry-run[=false]: If true, only print the object that would be sent, without sending it.
      --from-file=[]: Key files can be specified using their file path, in which case a default name will be given to them, or optionally with a name and file path, in which case the given name will be used.  Specifying a directory will iterate each named file in the directory that is a valid configmap key.
      --from-literal=[]: Specify a key and literal value to insert in configmap (i.e. mykey=somevalue)
      --generator="configmap/v1": The name of the API generator to use.
      --no-headers[=false]: When using the default output, don't print headers.
  -o, --output="": Output format. One of: json|yaml|wide|name|go-template=...|go-template-file=...|jsonpath=...|jsonpath-file=... See golang template [http://golang.org/pkg/text/template/#pkg-overview] and jsonpath template [http://releases.k8s.io/release-1.2/docs/user-guide/jsonpath.md].
      --output-version="": Output the formatted object with the given group version (for ex: 'extensions/v1beta1').
      --save-config[=false]: If true, the configuration of current object will be saved in its annotation. This is useful when you want to perform kubectl apply on this object in the future.
      --schema-cache-dir="~/.kube/schema": If non-empty, load/store cached API schemas in this directory, default is '$HOME/.kube/schema'
  -a, --show-all[=false]: When printing, show all resources (default hide terminated pods.)
      --show-labels[=false]: When printing, show all labels as the last column (default hide labels column)
      --sort-by="": If non-empty, sort list types using this field specification.  The field specification is expressed as a JSONPath expression (e.g. '{.metadata.name}'). The field in the API resource specified by this JSONPath expression must be an integer or a string.
      --template="": Template string or path to template file to use when -o=go-template, -o=go-template-file. The template format is golang templates [http://golang.org/pkg/text/template/#pkg-overview].
      --validate[=true]: If true, use a schema to validate the input before sending it
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl create](/docs/user-guide/kubectl/kubectl_create/)	 - Create a resource by filename or stdin


