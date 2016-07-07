---
---

## kubectl config set-cluster

Sets a cluster entry in kubeconfig

### 摘要


Sets a cluster entry in kubeconfig.
Specifying a name that already exists will merge new fields on top of existing values for those fields.

```
kubectl config set-cluster NAME [--server=server] [--certificate-authority=path/to/certficate/authority] [--insecure-skip-tls-verify=true]
```

### 示例

```
# Set only the server field on the e2e cluster entry without touching other values.
kubectl config set-cluster e2e --server=https://1.2.3.4

# Embed certificate authority data for the e2e cluster entry
kubectl config set-cluster e2e --certificate-authority=~/.kube/e2e/kubernetes.ca.crt

# Disable cert checking for the dev cluster entry
kubectl config set-cluster e2e --insecure-skip-tls-verify=true
```

### 选项

```
      --api-version="": api-version for the cluster entry in kubeconfig
      --certificate-authority="": path to certificate-authority for the cluster entry in kubeconfig
      --embed-certs[=false]: embed-certs for the cluster entry in kubeconfig
      --insecure-skip-tls-verify[=false]: insecure-skip-tls-verify for the cluster entry in kubeconfig
      --server="": server for the cluster entry in kubeconfig
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl config](/docs/user-guide/kubectl/kubectl_config/)	 - config modifies kubeconfig files


