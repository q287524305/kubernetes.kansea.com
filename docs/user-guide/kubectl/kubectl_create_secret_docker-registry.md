---
---
## kubectl create secret docker-registry

Create a secret for use with a Docker registry.

### 摘要



Create a new secret for use with Docker registries.

Dockercfg secrets are used to authenticate against Docker registries.

When using the Docker command line to push images, you can authenticate to a given registry by running
  'docker login DOCKER_REGISTRY_SERVER --username=DOCKER_USER --password=DOCKER_PASSWORD --email=DOCKER_EMAIL'.
That produces a ~/.dockercfg file that is used by subsequent 'docker push' and 'docker pull' commands to
authenticate to the registry.

When creating applications, you may have a Docker registry that requires authentication.  In order for the
nodes to pull images on your behalf, they have to have the credentials.  You can provide this information
by creating a dockercfg secret and attaching it to your service account.

```
kubectl create secret docker-registry NAME --docker-username=user --docker-password=password --docker-email=email [--docker-server=string] [--from-literal=key1=value1] [--dry-run]
```

### 示例

```
  # If you don't already have a .dockercfg file, you can create a dockercfg secret directly by using:
  $ kubectl create secret docker-registry my-secret --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL
```

### 选项

```
      --docker-email="": Email for Docker registry
      --docker-password="": Password for Docker registry authentication
      --docker-server="https://index.docker.io/v1/": Server location for Docker registry
      --docker-username="": Username for Docker registry authentication
      --dry-run[=false]: If true, only print the object that would be sent, without sending it.
      --generator="secret-for-docker-registry/v1": The name of the API generator to use.
      --no-headers[=false]: 当使用默认输出格式时不打印标题栏。
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

* [kubectl create secret](/docs/user-guide/kubectl/kubectl_create_secret/)	 - Create a secret using specified subcommand.


