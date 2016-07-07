---
---

## kubectl config set

在kubeconfig配置文件中设置一个单独的值。

### 摘要


在kubeconfig配置文件中设置一个单独的值
PROPERTY_NAME 使用“.”进行分隔，每段代表一个属性名或者map的键，map的键不能包含“.”。
PROPERTY_VALUE 需要设置的新值。

```
kubectl config set PROPERTY_NAME PROPERTY_VALUE
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl config](/docs/user-guide/kubectl/kubectl_config/)	 - config modifies kubeconfig files
