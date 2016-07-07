---
---

## kubectl config set-credentials

在kubeconfig配置文件中设置一个用户项。

### 摘要


在kubeconfig配置文件中设置一个用户项。
如果指定了一个已存在的名字，将合并新字段并覆盖旧字段。

  客户端证书设置：:
    --client-certificate=certfile --client-key=keyfile

  不记名令牌设置:
    --token=bearer_token

  Basic认证设置:
    --username=basic_user --password=basic_password

  不记名令牌和基础认证不能同时使用。


```
kubectl config set-credentials NAME [--client-certificate=path/to/certfile] [--client-key=path/to/keyfile] [--token=bearer_token] [--username=basic_user] [--password=basic_password]
```

### 示例

```
# 仅设置"cluster-admin"用户项下的"client-key"字段，不影响其他值
kubectl config set-credentials cluster-admin --client-key=~/.kube/admin.key

# 为"cluster-admin"用户项设置基础认证选项
kubectl config set-credentials cluster-admin --username=admin --password=uXFGweU9l35qcif

# 为cluster"cluster-admin"用户项开启证书验证并设置证书文件路径
kubectl config set-credentials cluster-admin --client-certificate=~/.kube/admin.crt --embed-certs=true
```

### 选项

```
      --client-certificate="": 设置kuebconfig配置文件中用户选项中的证书文件路径。
      --client-key="": 设置kuebconfig配置文件中用户选项中的证书密钥路径。
      --embed-certs[=false]: 在是否则kubeconfig配置文件中嵌入客户端证书/key
      --password="": 设置kuebconfig配置文件中用户选项中的密码。
      --token="": 设置kuebconfig配置文件中用户选项中的令牌。
      --username="": 设置kuebconfig配置文件中用户选项中的用户名。
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl config](/docs/user-guide/kubectl/kubectl_config/)	 - config modifies kubeconfig files
