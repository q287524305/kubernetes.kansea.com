---
assignees:
- bprashanth
- davidopp
- derekwaynecarr
- erictune
- janetkuo
- thockin

---

* TOC
{:toc}

## 它们是什么?

Admission Controller插件是一段代码，其拦截Kubernetes API服务的请求早于对象的持久性，
但是在请求的认证和授权之后。插件代码位于API服务进程中，会编译成二进制以便此时使用。

集群在接受一个请求之前，每一个Admission Controller插件都会按序运行。
如果这个序列中的某个插件拒绝该请求，则整个的请求都会被立刻拒绝，返回一个错误给用户。

Admission Controller插件在某些情况下也许会改变传进来的对象，配置系统默认值。
此外，Admission Controller插件也许会改变请求处理中的部分相关资源去做些事情，比如增量配额的使用。

## 为什么使用它？

Kubernetes中许多高级功能需要激活Admission Controller插件，
以便更好的支持该功能。总之，没有正确配置Admission Controller插件的Kubernetes API服务是不完整的服务，很多用户期望的服务是不支持的。

## 如何接入admission control插件？

Kubernetes API 服务器提供了一个参数，`admission-control`，用逗号分隔，在集群中修改对象之前，调用许可控制选项的有序列表。

## 每个插件的功能是什么？

### AlwaysAdmit

使用插件本身处理所有请求。

### AlwaysPullImages

This plug-in modifies every new Pod to force the image pull policy to Always. This is useful in a
multitenant cluster so that users can be assured that their private images can only be used by those
who have the credentials to pull them. Without this plug-in, once an image has been pulled to a
node, any pod from any user can use it simply by knowing the image's name (assuming the Pod is
scheduled onto the right node), without any authorization check against the image. When this plug-in
is enabled, images are always pulled prior to starting containers, which means valid credentials are
required.

### AlwaysDeny

拒绝所有请求，主要用于测试。

### DenyExecOnPrivileged (废弃)

如果一个Pod有一个特权Container，该插件就会拦截所有的请求，在该Pod中执行一个命令。

如果你的集群支持特权Container，而且你想要限制终端用户在那些Container中执行命令的权限，我们强烈建议使用该插件。

该功能已经合并到[DenyEscalatingExec](#denyescalatingexec)插件

### DenyEscalatingExec

该插件拒绝执行和附加令到允许主机访问的且有升级特权的 Pod。包含含有运行特权的 Pod，有访问主机 PID Namespace 的权限。

如果你的集群支持含有升级特权的Container，而且你想要限制终端用户在这些Container中执行命令的能力，我们强烈建议使用该插件。

### ServiceAccount

这个插件实现了[serviceAccounts](/docs/user-guide/service-accounts)的自动化。如果你打算使用Kubernetes `ServiceAccount`对象，我们强烈建议使用该插件。

### SecurityContextDeny

[SecurityContext](/docs/user-guide/security-context) 定义了一些不适用于`Container`的选项，这个插件将会拒绝任何含有该SecurityContext的Pod。

### ResourceQuota

该插件会检查传入的请求，确保其不违反任何`Namespace`中`ResourceQuota`对象枚举的约束条件。
如果你在 Kubernetes 开发中正在使用`ResourceQuota`对象，你必须使用该插件实现配额约束条件。

查看[resourceQuota设计文档](https://github.com/kubernetes/kubernetes/blob/{{page.githubbranch}}/docs/design/admission_control_resource_quota.md) 和[Resource Quota示例](/docs/admin/resourcequota/) 来了解更多细节。

强烈建议配置该插件在Admission Controller插件的序列中。是这样的，不会过早的分配配额，只有在 admission control拒绝之后才会分配。

### LimitRanger

该插件会检查传入的请求，确保其不违反任何`Namespace`中`LimitRange`对象枚举的约束条件。如果你在 Kubernetes 开发中正在使用`LimitRange`对象，你必须使用该插件实现约束条件。`LimitRange`也经常用于 Pod 中默认资源请求，不会指定哪一个请求。目前，默认`LimitRange`，在默认的`Namespace`中对所有的 Pod，需要 0.1CPU。

查阅[LimitRange设计文档](https://github.com/kubernetes/kubernetes/blob/{{page.githubbranch}}/docs/design/admission_control_limit_range.md)和[用例](/docs/admin/limitrange/)，了解更多详细信息。

### InitialResources (experimental)

This plug-in observes pod creation requests. If a container omits compute resource requests and limits,
then the plug-in auto-populates a compute resource request based on historical usage of containers running the same image.
If there is not enough data to make a decision the Request is left unchanged.
When the plug-in sets a compute resource request, it annotates the pod with information on what compute resources it auto-populated.

See the [InitialResouces proposal](https://github.com/kubernetes/kubernetes/blob/{{page.githubbranch}}/docs/proposals/initial-resources.md) for more details.

### NamespaceExists (废弃)

该插件会检查所有传入的请求，尝试在Kubernetes `Namespace`中创建资源，如果该`Namespace`不是当前创建的，该插件会拒绝这个请求。我们强烈建议使用该插件，确保数据的完整性。

Admission Controller的该功能已经并入`NamespaceLifecycle`插件。

### NamespaceAutoProvision (废弃)

该插件会检查所有传入的请求，尝试在Kubernetes `Namespace`中创建资源，如果`Namespace`不存在，会创建一个新的`Namespace`。

我们强烈建议`NamespaceExists`插件优先级高于`NamespaceAutoProvision`插件。

### NamespaceLifecycle

如果`Namespace`已经终止，则不能在其中创建新的`Namespace`，该插件会强制该操作。

删除一个`Namespace`会终止一系列操作，移除该`Namespace`中所有对象（Pod，服务等等）。为了加强该过程的完整性，我们建议使用该插件。

## 是否有推荐的插件集合？

是的。

高于1.2.0版本的，我们强烈建议使用如下的许可控制插件集合（按顺序排列）：

```shell
--admission-control=NamespaceLifecycle,LimitRanger,ServiceAccount,ResourceQuota
```

Kubernetes1.0，我们强烈建议使用如下的许可控制插件集合（按顺序排列）：

```shell
--admission-control=NamespaceLifecycle,LimitRanger,SecurityContextDeny,ServiceAccount,PersistentVolumeLabel,ResourceQuota
```
