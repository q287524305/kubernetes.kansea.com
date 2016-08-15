---
---

## kubectl exec

Execute a command in a container.

### 摘要


Execute a command in a container.

```
kubectl exec POD [-c CONTAINER] -- COMMAND [args...]
```

### 示例

```
# Get output from running 'date' from pod 123456-7890, using the first container by default
kubectl exec 123456-7890 date

# Get output from running 'date' in ruby-container from pod 123456-7890
kubectl exec 123456-7890 -c ruby-container date

# Switch to raw terminal mode, sends stdin to 'bash' in ruby-container from pod 123456-7890
# and sends stdout/stderr from 'bash' back to the client
kubectl exec 123456-7890 -c ruby-container -i -t -- bash -il
```

### 选项

```
  -c, --container="": Container name. If omitted, the first container in the pod will be chosen
  -p, --pod="": Pod name
  -i, --stdin[=false]: Pass stdin to the container
  -t, --tty[=false]: Stdin is a TTY
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](kubectl.md)	 - 使用kubectl来管理Kubernetes集群。
