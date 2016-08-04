---
assignees:
- bgrant0607
- eparis

---

## kubectl config set-context

在kubeconfig配置文件中设置一个环境项。

### 摘要


在kubeconfig配置文件中设置一个环境项。
如果指定了一个已存在的名字，将合并新字段并覆盖旧字段。

```
kubectl config set-context NAME [--cluster=cluster_nickname] [--user=user_nickname] [--namespace=namespace]
```

### 示例

```
# 设置gce环境项中的user字段，不影响其他字段。
kubectl config set-context gce --user=cluster-admin
```

### 选项

```
      --cluster="": 设置kuebconfig配置文件中环境选项中的集群。
      --namespace="": 设置kuebconfig配置文件中环境选项中的命名空间。
      --user="": 设置kuebconfig配置文件中环境选项中的用户。
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl config](/docs/user-guide/kubectl/kubectl_config/)	 - config modifies kubeconfig files
