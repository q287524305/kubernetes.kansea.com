---
---

## kubectl attach

连接到一个正在运行的容器。

### 摘要


Attach to a process that is already running inside an existing container.

```
kubectl attach POD -c CONTAINER
```

### 示例

```
# Get output from running pod 123456-7890, using the first container by default
kubectl attach 123456-7890

# Get output from ruby-container from pod 123456-7890
kubectl attach 123456-7890 -c ruby-container

# Switch to raw terminal mode, sends stdin to 'bash' in ruby-container from pod 123456-7890
# and sends stdout/stderr from 'bash' back to the client
kubectl attach 123456-7890 -c ruby-container -i -t
```

### 选项

```
  -c, --container="": Container name. If omitted, the first container in the pod will be chosen
  -i, --stdin[=false]: Pass stdin to the container
  -t, --tty[=false]: Stdin is a TTY
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](/docs/user-guide/kubectl/kubectl/)	 - 使用kubectl来管理Kubernetes集群。


