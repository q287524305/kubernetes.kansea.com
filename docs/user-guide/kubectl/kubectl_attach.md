---
assignees:
- bgrant0607
- eparis

---

## kubectl attach

连接到一个正在运行的容器。

### 摘要


连接到现有容器中一个正在运行的进程。

```
kubectl attach POD -c CONTAINER
```

### 示例

```
# 获取正在运行中的pod 123456-7890的输出，默认连接到第一个容器
kubectl attach 123456-7890

# 获取pod 123456-7890中ruby-container的输出
kubectl attach 123456-7890 -c ruby-container

# 切换到终端模式，将控制台输入发送到pod 123456-7890的ruby-container的“bash”命令，并将其输出到控制台/
# 错误控制台的信息发送回客户端。
kubectl attach 123456-7890 -c ruby-container -i -t
```

### 选项

```
  -c, --container="": 容器名。如果省略，则默认选择第一个 pod
  -i, --stdin[=false]: 将控制台输入发送到容器。
  -t, --tty[=false]: 将标准输入控制台作为容器的控制台输入。
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。
