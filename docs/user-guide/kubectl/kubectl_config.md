---
assignees:
- bgrant0607
- eparis

---

## kubectl config

config modifies kubeconfig files

### 摘要


config modifies kubeconfig files using subcommands like "kubectl config set current-context my-context"

The loading order follows these rules:
1. If the --kubeconfig flag is set, then only that file is loaded.  The flag may only be set once and no merging takes place.
2. If $KUBECONFIG environment variable is set, then it is used a list of paths (normal path delimitting rules for your system).  These paths are merged together.  When a value is modified, it is modified in the file that defines the stanza.  When a value is created, it is created in the first file that exists.  If no files in the chain exist, then it creates the last file in the list.
3. Otherwise, ${HOME}/.kube/config is used and no merging takes place.


```
kubectl config SUBCOMMAND
```

### 选项

```
      --kubeconfig="": use a particular kubeconfig file
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。
* [kubectl config current-context](/docs/user-guide/kubectl/kubectl_config_current-context/)	 - Displays the current-context
* [kubectl config set](/docs/user-guide/kubectl/kubectl_config_set/)	 - Sets an individual value in a kubeconfig file
* [kubectl config set-cluster](/docs/user-guide/kubectl/kubectl_config_set-cluster/)	 - Sets a cluster entry in kubeconfig
* [kubectl config set-context](/docs/user-guide/kubectl/kubectl_config_set-context/)	 - Sets a context entry in kubeconfig
* [kubectl config set-credentials](/docs/user-guide/kubectl/kubectl_config_set-credentials/)	 - Sets a user entry in kubeconfig
* [kubectl config unset](/docs/user-guide/kubectl/kubectl_config_unset/)	 - Unsets an individual value in a kubeconfig file
* [kubectl config use-context](/docs/user-guide/kubectl/kubectl_config_use-context/)	 - Sets the current-context in a kubeconfig file
* [kubectl config view](/docs/user-guide/kubectl/kubectl_config_view/)
	 - Displays merged kubeconfig settings or a specified kubeconfig file.


