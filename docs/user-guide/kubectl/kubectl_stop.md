---
---

## kubectl stop

已弃用: 通过 `name` 或者文件名 来关闭一个资源。

### 摘要

已弃用: 通过 `name` 或者文件名 来关闭一个资源。

stop 命令已弃用，由 delete 命令替代其所有功能。
更多详细请查看 'kubectl delete --help'。

Attempts to shut down and delete a resource that supports graceful termination.
If the resource is scalable it will be scaled to 0 before deletion.
尝试关闭和删除，一个可以正常终止的资源。
如果资源是可伸缩的，将删除之前被缩放为0。

```
kubectl stop (-f FILENAME | TYPE (NAME | -l label | --all))
```

### 示例

```
# 关闭 foo.
$ kubectl stop replicationcontroller foo

# 通过标签`name=myLabel`来终止 pod 和 service。
$ kubectl stop pods,services -l name=myLabel

# 关闭由`service.json`定义的`service`。
$ kubectl stop -f service.json

# 关闭`path/to/resources`文件夹下所有资源。
$ kubectl stop -f path/to/resources
```

### 选项

```
      --all[=false]: [-all] 选择所有指定的资源。
  -f, --filename=[]: 终止指定资源的文件名，目录名或者URL。
      --grace-period=-1: 在几秒内正常终止资源，负数的话则忽略。
      --ignore-not-found[=false]: 认为"resource not found"是终止成功。
  -o, --output="": 输出模式。"-o name"为快捷输出(资源/name).
  -l, --selector="": 刷选标签。
      --timeout=0: 放弃删除之前等待的时间，0 表示根据对象的大小去设定一个超时时间。
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](kubectl.md)	 - 使用kubectl来管理Kubernetes集群。
