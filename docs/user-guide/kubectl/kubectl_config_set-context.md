---
---

## kubectl config set-context

Sets a context entry in kubeconfig

### 摘要


Sets a context entry in kubeconfig
Specifying a name that already exists will merge new fields on top of existing values for those fields.

```
kubectl config set-context NAME [--cluster=cluster_nickname] [--user=user_nickname] [--namespace=namespace]
```

### 示例

```
# Set the user field on the gce context entry without touching other values
kubectl config set-context gce --user=cluster-admin
```

### 选项

```
      --cluster="": cluster for the context entry in kubeconfig
      --namespace="": namespace for the context entry in kubeconfig
      --user="": user for the context entry in kubeconfig
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl config](/docs/user-guide/kubectl/kubectl_config/)	 - config modifies kubeconfig files


