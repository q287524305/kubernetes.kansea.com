---
---

可以复制`kubectl`客户端配置文件([kubeconfig](/docs/user-guide/kubeconfig-file))可以访问正在运行的 Kubernetes 群集。
通过 `cluster/kube-up.sh` 产生的，配置文件在 `$HOME/.kube/config` 中，生成并使用`kubeconfig`的详细步骤如下：

**1. 创建一个群集**

```shell
$ cluster/kube-up.sh
```

**2. 复制 `kubeconfig` 到新主机**

```shell
$ scp $HOME/.kube/config user@remotehost:/path/to/.kube/config
```

**3. 在新主机中，让`kubectl`可以使用`config`副本**

* 选项 A：复制到默认位置

```shell
$ mv /path/to/.kube/config $HOME/.kube/config
```

* 选项 B：复制到工作目录（kubectl使用的）

```shell
$ mv /path/to/.kube/config $PWD
```

* 选项 C：手动指定 `kubectl`到`kubeconfig` 所在位置

```shell
# 通过环境变量
$ export KUBECONFIG=/path/to/.kube/config

# 通过命令行参数
$ kubectl ... --kubeconfig=/path/to/.kube/config
```

## 手动生成 `kubeconfig`

`kubeconfig` 是由 `kube-up` 生成的，你也可以用下面的命令自己生成它（任何指令集）：

```shell
# 创建 kubeconfig 入口
$ kubectl config set-cluster $CLUSTER_NICK \
    --server=https://1.1.1.1 \
    --certificate-authority=/path/to/apiserver/ca_file \
    --embed-certs=true \
    # 如果不需要 TLS ，则用下面两行替换 --certificate-authority 和 --embed-certs
    --insecure-skip-tls-verify=true \
    --kubeconfig=/path/to/standalone/.kube/config

# 创建用户入口
$ kubectl config set-credentials $USER_NICK \
    # 加载在主机生成的 token。
    --token=$token \
    # 使用 username|password 或者 token，任意一种方式
    --username=$username \
    --password=$password \
    --client-certificate=/path/to/crt_file \
    --client-key=/path/to/key_file \
    --embed-certs=true \
    --kubeconfig=/path/to/standalone/.kube/config

# 创建 context 入口
$ kubectl config set-context $CONTEXT_NAME --cluster=$CLUSTER_NICKNAME --user=$USER_NICK
```

笔记：

* `--embed-certs` 是指需要产生一个独立的 `kubeconfig`，将在另一台主机上运行。
* `--kubeconfig` 既加载配置参数，也加载保存的配置文件。如果第一次运行，上面的`--kubeconfig`文件可能会被忽略

```shell
$ export KUBECONFIG=/path/to/standalone/.kube/config
```

* 这个 ca_file, key_file, 和上面提到 KUBE 群集主机产生的 cert_file。
他们可以在`/srv/kubernetes`文件夹下。
承载的 token/basic 身份认证 也由 kube 主机上产生。

查看[kubeconfig-file.md](/docs/user-guide/kubeconfig-file) 来了解更多 `kubeconfig` 细节 ,
或者运行 `kubectl config -h`.

## 合并 `kubeconfig` 的例子

`kubectl` 加载并合并配置文件（按顺序）

1. `--kubeconfig=/path/to/.kube/config` 命令行标识
2. `KUBECONFIG=/path/to/.kube/config` 环境变量
3. `$PWD/.kube/config`
4. `$HOME/.kube/config`

如果在 host1创建群集 A，B，
在 host2上创建群集 C，D，你可以在两台主机上运行所有四组群集。

```shell
# 在 host2，复制 host1 的默认 kubeconfig，并且配置到环境变量
$ scp host1:/path/to/home1/.kube/config /path/to/other/.kube/config

$ export $KUBECONFIG=/path/to/other/.kube/config

# 在 host1，复制 host2 的默认 kubeconfig，并且配置到环境变量
$ scp host2:/path/to/home2/.kube/config /path/to/other/.kube/config

$ export $KUBECONFIG=/path/to/other/.kube/config
```

加载/合并 `kubeconfig`的详细的例子和说明在[kubeconfig-file](/docs/user-guide/kubeconfig-file)。
