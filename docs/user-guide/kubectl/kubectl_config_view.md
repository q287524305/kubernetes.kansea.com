---
assignees:
- bgrant0607
- brendandburns
- eparis
- hurf

---
## kubectl config view

显示合并后的kubeconfig设置，或者一个指定的kubeconfig配置文件。

### 摘要


显示合并后的kubeconfig设置，或者一个指定的kubeconfig配置文件。

你可以设置jsonpath表达式 `--output jsonpath={...}`来选择输出指定的值。

```
kubectl config view
```

### 示例

```
# 显示合并后的kubeconfig设置
kubectl config view

# 获取e2e用户的密码
kubectl config view -o jsonpath='{.users[?(@.name == "e2e")].user.password}'
```

### 选项

```
      --flatten[=false]: 将读取的kubeconfig配置文件扁平输出为自包含的结构（对创建可迁移的kubeconfig配置文件有帮助）
      --merge[=true]: 按照继承关系合并所有的kubeconfig配置文件。
      --minify[=false]: 如果为true，不显示目前环境未使用到的任何信息。
      --no-headers[=false]: 当使用默认输出格式时不打印标题栏。
  -o, --output="": Output format. One of: json|yaml|wide|name|go-template=...|go-template-file=...|jsonpath=...|jsonpath-file=... See golang template [http://golang.org/pkg/text/template/#pkg-overview] and jsonpath template [http://releases.k8s.io/release-1.2/docs/user-guide/jsonpath.md].
      --output-version="": Output the formatted object with the given group version (for ex: 'extensions/v1beta1').
      --raw[=false]: display raw byte data
  -a, --show-all[=false]: When printing, show all resources (default hide terminated pods.)
      --show-labels[=false]: When printing, show all labels as the last column (default hide labels column)
      --sort-by="": If non-empty, sort list types using this field specification.  The field specification is expressed as a JSONPath expression (e.g. '{.metadata.name}'). The field in the API resource specified by this JSONPath expression must be an integer or a string.
      --template="": Template string or path to template file to use when -o=go-template, -o=go-template-file. The template format is golang templates [http://golang.org/pkg/text/template/#pkg-overview].
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl config](/docs/user-guide/kubectl/kubectl_config/)	 - config modifies kubeconfig files
