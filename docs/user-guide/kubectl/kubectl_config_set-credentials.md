---
---

## kubectl config set-credentials

Sets a user entry in kubeconfig

### 摘要


Sets a user entry in kubeconfig
Specifying a name that already exists will merge new fields on top of existing values.

  Client-certificate flags:
    --client-certificate=certfile --client-key=keyfile

  Bearer token flags:
    --token=bearer_token

  Basic auth flags:
    --username=basic_user --password=basic_password

  Bearer token and basic auth are mutually exclusive.


```
kubectl config set-credentials NAME [--client-certificate=path/to/certfile] [--client-key=path/to/keyfile] [--token=bearer_token] [--username=basic_user] [--password=basic_password]
```

### 示例

```
# Set only the "client-key" field on the "cluster-admin"
# entry, without touching other values:
kubectl config set-credentials cluster-admin --client-key=~/.kube/admin.key

# Set basic auth for the "cluster-admin" entry
kubectl config set-credentials cluster-admin --username=admin --password=uXFGweU9l35qcif

# Embed client certificate data in the "cluster-admin" entry
kubectl config set-credentials cluster-admin --client-certificate=~/.kube/admin.crt --embed-certs=true
```

### 选项

```
      --client-certificate="": path to client-certificate for the user entry in kubeconfig
      --client-key="": path to client-key for the user entry in kubeconfig
      --embed-certs[=false]: embed client cert/key for the user entry in kubeconfig
      --password="": password for the user entry in kubeconfig
      --token="": token for the user entry in kubeconfig
      --username="": username for the user entry in kubeconfig
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl config](/docs/user-guide/kubectl/kubectl_config/)	 - config modifies kubeconfig files


