---
---

## kubectl annotate

更新某个资源的注解

### 摘要


更新一个或多个资源的注解。

注解是一个键值对，它可以包含比label更多的信息，并且可能是机读数据。
注解用来存储那些辅助的，非区分性的信息，特别是那些为外部工具或系统扩展插件使用的数据。
如果`--overwrite`设为true，将会覆盖现有的注解，否则试图修改一个注解的值将会抛出错误。
如果设置了`--resource-version`，那么将会使用指定的这个版本，否则将使用当前版本。


支持的资源包括但不限于（大小写不限）：pods (po)、services (svc)、
replicationcontrollers (rc)、nodes (no)、events (ev)、componentstatuses (cs)、
limitranges (limits)、persistentvolumes (pv)、persistentvolumeclaims (pvc)、
orizontalpodautoscalers (hpa), resourcequotas (quota) 和 secrets

```
kubectl annotate [--overwrite] (-f FILENAME | TYPE NAME) KEY_1=VAL_1 ... KEY_N=VAL_N [--resource-version=version]
```

### 示例

```
# 更新pod “foo”，设置其注解'description'的值为'my frontend'。
# 如果同一个注解被赋值了多次，只保存最后一次设置的值。
kubectl annotate pods foo description='my frontend'

# 更新“pod.json”文件中 type 和 name 字段指定的 pod 的注解。
kubectl annotate -f pod.json description='my frontend'

# 更新pod “foo”，设置其注解'description'的值为'my frontend running nginx'，已有的值将被覆盖。
kubectl annotate --overwrite pods foo description='my frontend running nginx'

# 更新同一'namespace'下所有的pod。
kubectl annotate pods --all description='my frontend running nginx'

# 仅当pod “foo”当前版本为1时，更新其注解
kubectl annotate pods foo description='my frontend running nginx' --resource-version=1

# Update pod 'foo' by removing an annotation named 'description' if it exists.
# 更新pod “foo”，如果存在的话将删除其注解'description'。
# 不需要`--override`选项。
kubectl annotate pods foo description-
```

### 选项

```
      --all[=false]: 选择namespace中所有指定类型的资源。
  -f, --filename=[]: 用来指定待升级资源的文件名，目录名或者URL。
      --no-headers[=false]: 当使用默认输出格式时不打印标题栏。
  -o, --output="": 输出格式。一下任意一种：json|yaml|wide|name|go-template=...|go-template-file=...|jsonpath=...|jsonpath-file=... 参见 golang template [http://golang.org/pkg/text/template/#pkg-overview] 和 jsonpath template [http://releases.k8s.io/release-1.2/docs/user-guide/jsonpath.md].
      --output-version="": 输出指定版本格式的对象 (例如: 'extensions/v1beta1').
      --overwrite[=false]: 如果设置为true，允许覆盖更新注解，否则拒绝更新已存在的注解。
      --record[=false]: 在资源注释中记录当前 kubectl 命令。
      --resource-version="": 如果不为空，仅当资源当前版本和指定版本相同时才能更新注解。仅当更新单个资源时有效。
  -l, --selector="": 刷选标签。
  -a, --show-all[=false]: 打印时，显示所有资源(默认隐藏已终止的 Pod)
      --show-labels[=false]: 打印时，在最后一列显示所有标签(默认隐藏标签栏)
      --sort-by="": 非空时，按照这个字段的规定排序。该字段需要设置为 JSONPath 表达式 (例如:'{.metadata.name}')，在 JSONPath 表达式中指定 API 资源领域时，必须使用整数或者字符串。
      --template="": 当 -o=go-template, -o=go-template-file 时需要设置为模板字符串或者模板文件路径， 模板格式是 golang templates [http://golang.org/pkg/text/template/#pkg-overview].
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。
