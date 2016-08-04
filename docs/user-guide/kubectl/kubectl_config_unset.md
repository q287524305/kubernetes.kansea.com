---
assignees:
- bgrant0607
- eparis

---

## kubectl config unset

在kubeconfig配置文件中清除一个单独的值。

### 摘要


在kubeconfig配置文件中清除一个单独的值。
PROPERTY_NAME 使用“.”进行分隔，每段代表一个属性名或者map的键，map的键不能包含“.”。

```
kubectl config unset PROPERTY_NAME
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl config](/docs/user-guide/kubectl/kubectl_config/)	 - config modifies kubeconfig files
