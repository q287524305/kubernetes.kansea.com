---
---

## kubectl edit

编辑服务端的资源。

### 摘要


使用系统默认编辑器编辑服务端的资源。

edit 命令允许你直接编辑使用命令行工具获取的任何资源。此命令将打开你通过 KUBE_EDITOR 或者 EDITOR 环境变量定义的编辑器，
或者直接在 Linux 上使用“vi” 或者windows 的”notepad”。你可以同时编辑多个资源，但所有的变更都只会一次性
提交。除了命令行参数外，此命令也接受文件名，但所直指定的文件必须使用早于当前的资源版本。

The files to edit will be output in the default API version, or a version specified
by --output-version. The default format is YAML - if you would like to edit in JSON
pass -o json. The flag --windows-line-endings can be used to force Windows line endings,
otherwise the default for your operating system will be used.
所编辑的文件将使用默认的API版本输出，或者通过`--output-version`选项显式指定。默认的输出格式是YAML，
如果你需要使用JSON格式编辑，使用`-o json`选项。`--windows-line-endings`则可以强制使用 windows 行尾符，不指定的话将会使用操作系统默认值。

如果当更新资源的时候出现错误，将会在磁盘上创建一个临时文件，记录未成功的更新。更新资源时最常见的错误
是其他人也在更新服务端的资源。当这种情况发生时，你需要将你所作的更改应用到最新版本的资源上，或者编辑
保存的临时文件，使用最新的资源版本。

```
kubectl edit (RESOURCE/NAME | -f FILENAME)
```

### 示例

```
  # 编辑名为“docker-registry”的service
  kubectl edit svc/docker-registry

  # 使用一个不同的编辑器
  KUBE_EDITOR="nano" kubectl edit svc/docker-registry

  # 编辑名为“docker-registry”的service，使用JSON格式、v1 API版本
  kubectl edit svc/docker-registry --output-version=v1 -o json
```

### 选项

```
  -f, --filename=[]: 用来指定待编辑资源的文件名，目录名或者URL。
      --include-extended-apis[=true]: If true, include definitions of new APIs via calls to the API server. [default true]
  -o, --output="yaml": 输出格式，可选yaml或者json中的一种。
      --output-version="": 输出资源使用的API版本（例如'extensions/v1beta1'）。
      --record[=false]: 在资源注释里记录当前 kubectl 命令。
  -R, --recursive[=false]: Process the directory used in -f, --filename recursively. Useful when you want to manage related manifests organized within the same directory.
      --save-config[=false]: 如果为true，当前对象的配置会保存在它的注释里。如果以后还要执行此 kubectl 命令的话，这会非常有用。
      --windows-line-endings[=false]: 使用 Windows 行尾符（默认为 Unix 行尾符）
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](kubectl.md)	 - 使用 kubectl 来管理 Kubernetes 集群。
