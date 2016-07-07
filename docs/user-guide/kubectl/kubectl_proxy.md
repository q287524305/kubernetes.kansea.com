---
---

## kubectl proxy

Run a proxy to the Kubernetes API server

### 摘要


To proxy all of the kubernetes api and nothing else, use:

kubectl proxy --api-prefix=/

To proxy only part of the kubernetes api and also some static files:

kubectl proxy --www=/my/files --www-prefix=/static/ --api-prefix=/api/

The above lets you 'curl localhost:8001/api/v1/pods'.

To proxy the entire kubernetes api at a different root, use:

kubectl proxy --api-prefix=/custom/

The above lets you 'curl localhost:8001/custom/api/v1/pods'


```
kubectl proxy [--port=PORT] [--www=static-dir] [--www-prefix=prefix] [--api-prefix=prefix]
```

### 示例

```
# Run a proxy to kubernetes apiserver on port 8011, serving static content from ./local/www/
kubectl proxy --port=8011 --www=./local/www/

# Run a proxy to kubernetes apiserver on an arbitrary local port.
# The chosen port for the server will be output to stdout.
kubectl proxy --port=0

# Run a proxy to kubernetes apiserver, changing the api prefix to k8s-api
# This makes e.g. the pods api available at localhost:8011/k8s-api/v1/pods/
kubectl proxy --api-prefix=/k8s-api
```

### 选项

```
      --accept-hosts="^localhost$,^127\\.0\\.0\\.1$,^\\[::1\\]$": Regular expression for hosts that the proxy should accept.
      --accept-paths="^/.*": Regular expression for paths that the proxy should accept.
      --address="127.0.0.1": The IP address on which to serve on.
      --api-prefix="/": Prefix to serve the proxied API under.
      --disable-filter[=false]: If true, disable request filtering in the proxy. This is dangerous, and can leave you vulnerable to XSRF attacks, when used with an accessible port.
  -p, --port=8001: The port on which to run the proxy. Set to 0 to pick a random port.
      --reject-methods="POST,PUT,PATCH": Regular expression for HTTP methods that the proxy should reject.
      --reject-paths="^/api/.*/exec,^/api/.*/run,^/api/.*/attach": Regular expression for paths that the proxy should reject.
  -u, --unix-socket="": Unix socket on which to run the proxy.
  -w, --www="": Also serve static files from the given directory under the specified prefix.
  -P, --www-prefix="/static/": Prefix to serve static files under, if static file directory is specified.
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。


