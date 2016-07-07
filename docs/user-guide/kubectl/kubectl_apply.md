---
---

## kubectl apply

通过文件名或控制台输入，对资源进行配置。

### 摘要


通过文件名或控制台输入，对资源进行配置。
如果资源不存在，将会新建一个。

可以使用 JSON 或者 YAML 格式。

```
kubectl apply -f FILENAME
```

### 示例

```
# 将pod.json中的配置应用到pod
kubectl apply -f ./pod.json

# 将控制台输入的JSON配置应用到Pod
cat pod.json | kubectl apply -f -
```

### 选项

```
  -f, --filename=[]: 包含配置信息的文件名，目录名或者URL。
  -o, --output="": 输出模式。"-o name"为快捷输出(资源/name).
      --record[=false]: 在资源注释中记录当前 kubectl 命令。
      --schema-cache-dir="~/.kube/schema": 非空则将API schema缓存为指定文件，默认缓存到'$HOME/.kube/schema'
      --validate[=true]: 如果为true，在发送到服务端前先使用schema来验证输入。
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。
