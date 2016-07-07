---
---

## kubectl config set-cluster

在kubeconfig配置文件中设置一个集群项。

### 摘要


在kubeconfig配置文件中设置一个集群项。
如果指定了一个已存在的名字，将合并新字段并覆盖旧字段。

```
kubectl config set-cluster NAME [--server=server] [--certificate-authority=path/to/certficate/authority] [--insecure-skip-tls-verify=true]
```

### 示例

```
# 仅设置e2e集群项中的server字段，不影响其他字段
kubectl config set-cluster e2e --server=https://1.2.3.4

# 向e2e集群项中添加认证鉴权数据
kubectl config set-cluster e2e --certificate-authority=~/.kube/e2e/kubernetes.ca.crt

# 取消dev集群项中的证书检查
kubectl config set-cluster e2e --insecure-skip-tls-verify=true
```

### 选项

```
      --api-version="": 设置kuebconfig配置文件中集群选项中的api-version。
      --certificate-authority="": 设置kuebconfig配置文件中集群选项中的certificate-authority路径。
      --embed-certs[=false]: 在是否则kubeconfig配置文件中嵌入客户端证书/key。
      --insecure-skip-tls-verify[=false]: 设置kuebconfig配置文件中集群选项中的insecure-skip-tls-verify开关。
      --server="": 设置kuebconfig配置文件中集群选项中的server。
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl config](/docs/user-guide/kubectl/kubectl_config/)	 - config modifies kubeconfig files
