---
---

Kubernetes可以在多种平台运行，从笔记本电脑，到云服务商的虚拟机，再到机架上的物理机服务器。
要创建一个Kubernetes集群，根据不同场景需要做的也不尽相同，可能是运行一条命令，也可能是配置自己的定制集群。
这里我们将引导你根据自己的需要选择合适的解决方案。

如果你只是想试一试 Kubernetes，我们推荐[基于 Docker 的本地](/docs/getting-started-guides/docker) 方案。

基于Docker的本地方案是众多能够完成快速搭建的[本地集群](#local-machine-solutions)方案中的一种，但是局限于单台机器。

当你准备好扩展到多台机器和更高可用性时，[托管](#hosted-solutions)解决方案是最容易搭建和维护的。

[全套云端方案](#turn-key-cloud-solutions) 只需要少数几个命令就可以在更多的云服务提供商搭建 Kubernetes。

[定制方案](#custom-solutions)  需要花费更多的精力，如果从头开始搭建 Kubernetes 集群，建议按照指南一步一步去做。

* TOC
{:toc}

### 本地服务器方案

本地服务器方案在一台物理机上创建拥有一个或者多个 Kubernetes 节点的单机集群。
创建过程是全自动的，且不需要任何云服务商的账户。
但是这种单机集群的规模和可用性都受限于单台机器。

本地服务器方案有：

- [Minikube](/docs/getting-started-guides/minikube/) (推荐)
- [基于本地 Docker](/docs/getting-started-guides/docker)
- [无虚拟机本地集群](/docs/getting-started-guides/locally) (Linux)
- [Vagrant](/docs/getting-started-guides/vagrant)


### 托管方案

[Google Container Engine](https://cloud.google.com/container-engine) 提供创建好的 Kubernetes 集群。

[Stackpoint.io](https://stackpoint.io) 提供自动构建和管理 Kubernetes 的云平台。

### 全套云端方案

以下方案让你可以通过几个命令就在很多IaaS云服务中创建Kubernetes集群，并且有很活跃的社区支持。

- [GCE](/docs/getting-started-guides/gce)
- [AWS](/docs/getting-started-guides/aws)
- [Azure](/docs/getting-started-guides/coreos/azure/) (基于 Weave, 由 WeaveWorks 提供)
- [Azure](/docs/getting-started-guides/azure/) (基于 Flannel, 由 Microsoft 提供)
- [CenturyLink Cloud](/docs/getting-started-guides/clc)

### 定制方案

Kubernetes可以在云服务提供商和物理机环境运行，并支持很多基本操作系统。

如果你可直接使用下列的指南中列举的方案。
某些指南可能有些过时，但是比起从零开始还是有不少参考价值。
如果你确实因为特殊原因或因为想了解底层原理，想要从零开始搭建，可以试试参考[从零开始指南](/docs/getting-started-guides/scratch)。

如果你对在新的平台支持 Kubernetes 感兴趣，
可以看看我们的[写新方案的建议](https://github.com/kubernetes/kubernetes/tree/{{page.githubbranch}}/docs/devel/writing-a-getting-started-guide.md)。

#### 云

以下是上文没有列出的云服务商或云操作系统支持的方案。

- [AWS + coreos](/docs/getting-started-guides/coreos)
- [GCE + CoreOS](/docs/getting-started-guides/coreos)
- [AWS + Ubuntu](/docs/getting-started-guides/juju)
- [Joyent + Ubuntu](/docs/getting-started-guides/juju)
- [Rackspace + CoreOS](/docs/getting-started-guides/rackspace)

#### 私有虚拟机

<<<<<<< HEAD
- [Vagrant](/docs/getting-started-guides/coreos) (采用CoreOS和flannel)
- [CloudStack](/docs/getting-started-guides/cloudstack) 采用Ansible，CoreOS和flannel）
- [Vmware](/docs/getting-started-guides/vsphere)  （采用Debian）
- [juju.md](/docs/getting-started-guides/juju) （采用Juju，Ubuntu和flannel）
- [Vmware](/docs/getting-started-guides/coreos) （采用CoreOS和flannel）
- [libvirt-coreos.md](/docs/getting-started-guides/libvirt-coreos)  （采用CoreOS）
=======
- [Vagrant](/docs/getting-started-guides/coreos) (uses CoreOS and flannel)
- [CloudStack](/docs/getting-started-guides/cloudstack) (uses Ansible, CoreOS and flannel)
- [Vmware vSphere](/docs/getting-started-guides/vsphere)  (uses Debian)
- [Vmware Photon Controller](/docs/getting-started-guides/photon-controller)  (uses Debian)
- [juju.md](/docs/getting-started-guides/juju) (uses Juju, Ubuntu and flannel)
- [Vmware](/docs/getting-started-guides/coreos)  (uses CoreOS and flannel)
- [libvirt-coreos.md](/docs/getting-started-guides/libvirt-coreos)  (uses CoreOS)
>>>>>>> 2ac973c0dd5984820428b35dca5f48c17e7f7287
- [oVirt](/docs/getting-started-guides/ovirt)
- [OpenStack Heat](/docs/getting-started-guides/openstack-heat) (采用 CentOS 和 flannel)
- [libvirt](/docs/getting-started-guides/fedora/flannel_multi_node_cluster) (采用 Fedora 和 flannel)
- [KVM](/docs/getting-started-guides/fedora/flannel_multi_node_cluster)  (采用 Fedora 和 flannel)

#### 物理机

- [Offline](/docs/getting-started-guides/coreos/bare_metal_offline) （无需互联网，采用CoreOS和flannel）
- [fedora/fedora_ansible_config.md](/docs/getting-started-guides/fedora/fedora_ansible_config)
- [Fedora single node](/docs/getting-started-guides/fedora/fedora_manual_config)
- [Fedora multi node](/docs/getting-started-guides/fedora/flannel_multi_node_cluster)
- [Centos](/docs/getting-started-guides/centos/centos_manual_config)
- [Ubuntu](/docs/getting-started-guides/ubuntu)
- [Docker Multi Node](/docs/getting-started-guides/docker-multinode)

#### 集成

这些方案提供了第三方的调度，资源管理，和更低级的平台。

- [Kubernetes on Mesos](/docs/getting-started-guides/mesos)
  - 使用 GCE 指令，但通用的，足以适应大多数现有的 Mesos 集群。
- [Kubernetes on DCOS](/docs/getting-started-guides/dcos)
  - 采用 AWS 的社区版 DCOS。
  - 支持云主机，私有虚拟机，记忆物理机的 企业版 DOCS。

## 解决方案表格

以下用表格形式列出上面的所有方案

IaaS Provider        | Config. Mgmt | OS     | Networking  | Docs                                              | Conforms | Support Level
-------------------- | ------------ | ------ | ----------  | ---------------------------------------------     | ---------| ----------------------------
GKE                  |              |        | GCE         | [docs](https://cloud.google.com/container-engine) | ['œ“][3]   | Commercial
Stackpoint.io        |              | multi-support       | multi-support   | [docs](http://www.stackpointcloud.com) |    | Commercial
GCE                  | Saltstack    | Debian | GCE         | [docs](/docs/getting-started-guides/gce)                                    | ['œ“][1]   | Project
Azure                | CoreOS       | CoreOS | Weave       | [docs](/docs/getting-started-guides/coreos/azure/)                    |          | Community ([@errordeveloper](https://github.com/errordeveloper), [@squillace](https://github.com/squillace), [@chanezon](https://github.com/chanezon), [@crossorigin](https://github.com/crossorigin))
Azure                | CoreOS       | CoreOS | flannel     | [docs](/docs/getting-started-guides/azure)                    |          | Community ([@colemickens](https://github.com/colemickens))
Docker Single Node   | custom       | N/A    | local       | [docs](/docs/getting-started-guides/docker)                                 |          | Project ([@brendandburns](https://github.com/brendandburns))
Docker Multi Node    | custom       | N/A    | flannel     | [docs](/docs/getting-started-guides/docker-multinode)                       |          | Project ([@brendandburns](https://github.com/brendandburns))
Bare-metal           | Ansible      | Fedora | flannel     | [docs](/docs/getting-started-guides/fedora/fedora_ansible_config)           |          | Project
Bare-metal           | custom       | Fedora | _none_      | [docs](/docs/getting-started-guides/fedora/fedora_manual_config)            |          | Project
Bare-metal           | custom       | Fedora | flannel     | [docs](/docs/getting-started-guides/fedora/flannel_multi_node_cluster)      |          | Community ([@aveshagarwal](https://github.com/aveshagarwal))
libvirt              | custom       | Fedora | flannel     | [docs](/docs/getting-started-guides/fedora/flannel_multi_node_cluster)      |          | Community ([@aveshagarwal](https://github.com/aveshagarwal))
KVM                  | custom       | Fedora | flannel     | [docs](/docs/getting-started-guides/fedora/flannel_multi_node_cluster)      |          | Community ([@aveshagarwal](https://github.com/aveshagarwal))
Mesos/Docker         | custom       | Ubuntu | Docker      | [docs](/docs/getting-started-guides/mesos-docker)                           |          | Community ([Kubernetes-Mesos Authors](https://github.com/mesosphere/kubernetes-mesos/blob/master/AUTHORS.md))
Mesos/GCE            |              |        |             | [docs](/docs/getting-started-guides/mesos)                                  |          | Community ([Kubernetes-Mesos Authors](https://github.com/mesosphere/kubernetes-mesos/blob/master/AUTHORS.md))
DCOS                 | Marathon   | CoreOS/Alpine | custom | [docs](/docs/getting-started-guides/dcos)                                   |          | Community ([Kubernetes-Mesos Authors](https://github.com/mesosphere/kubernetes-mesos/blob/master/AUTHORS.md))
AWS                  | CoreOS       | CoreOS | flannel     | [docs](/docs/getting-started-guides/aws)                                 |          | Community
GCE                  | CoreOS       | CoreOS | flannel     | [docs](/docs/getting-started-guides/coreos)                                 |          | Community ([@pires](https://github.com/pires))
Vagrant              | CoreOS       | CoreOS | flannel     | [docs](/docs/getting-started-guides/coreos)                                 |          | Community ([@pires](https://github.com/pires), [@AntonioMeireles](https://github.com/AntonioMeireles))
Bare-metal (Offline) | CoreOS       | CoreOS | flannel     | [docs](/docs/getting-started-guides/coreos/bare_metal_offline)              |          | Community ([@jeffbean](https://github.com/jeffbean))
Bare-metal           | CoreOS       | CoreOS | Calico      | [docs](/docs/getting-started-guides/coreos/bare_metal_calico)               |          | Community ([@caseydavenport](https://github.com/caseydavenport))
CloudStack           | Ansible      | CoreOS | flannel     | [docs](/docs/getting-started-guides/cloudstack)                             |          | Community ([@runseb](https://github.com/runseb))
Vmware vSphere       | Saltstack    | Debian | OVS         | [docs](/docs/getting-started-guides/vsphere)                                |          | Community ([@imkin](https://github.com/imkin))
Vmware Photon        | Saltstack    | Debian | OVS         | [docs](/docs/getting-started-guides/photon-controller)                      |          | Community ([@alainroy](https://github.com/alainroy))
Bare-metal           | custom       | CentOS | _none_      | [docs](/docs/getting-started-guides/centos/centos_manual_config)            |          | Community ([@coolsvap](https://github.com/coolsvap))
AWS                  | Juju         | Ubuntu | flannel     | [docs](/docs/getting-started-guides/juju)                                   |          | [Community](https://github.com/whitmo/bundle-kubernetes) ( [@whit](https://github.com/whitmo), [@matt](https://github.com/mbruzek), [@chuck](https://github.com/chuckbutler) )
OpenStack/HPCloud    | Juju         | Ubuntu | flannel     | [docs](/docs/getting-started-guides/juju)                                   |          | [Community](https://github.com/whitmo/bundle-kubernetes) ( [@whit](https://github.com/whitmo), [@matt](https://github.com/mbruzek), [@chuck](https://github.com/chuckbutler) )
Joyent               | Juju         | Ubuntu | flannel     | [docs](/docs/getting-started-guides/juju)                                   |          | [Community](https://github.com/whitmo/bundle-kubernetes) ( [@whit](https://github.com/whitmo), [@matt](https://github.com/mbruzek), [@chuck](https://github.com/chuckbutler) )
AWS                  | Saltstack    | Debian | AWS         | [docs](/docs/getting-started-guides/aws)                                    |          | Community ([@justinsb](https://github.com/justinsb))
AWS                  | kops         | Debian | AWS         | [docs](https://github.com/kubernetes/kops)                                  |          | Community ([@justinsb](https://github.com/justinsb))
Bare-metal           | custom       | Ubuntu | Calico      | [docs](/docs/getting-started-guides/ubuntu-calico)                          |          | Community ([@djosborne](https://github.com/djosborne))
Bare-metal           | custom       | Ubuntu | flannel     | [docs](/docs/getting-started-guides/ubuntu)                                 |          | Community ([@resouer](https://github.com/resouer), [@WIZARD-CXY](https://github.com/WIZARD-CXY))
libvirt/KVM          | CoreOS       | CoreOS | libvirt/KVM | [docs](/docs/getting-started-guides/libvirt-coreos)                         |          | Community ([@lhuard1A](https://github.com/lhuard1A))
oVirt                |              |        |             | [docs](/docs/getting-started-guides/ovirt)                                  |          | Community ([@simon3z](https://github.com/simon3z))
OpenStack Heat       | Saltstack    | CentOS | Neutron + flannel hostgw | [docs](/docs/getting-started-guides/openstack-heat)            |          | Community ([@FujitsuEnablingSoftwareTechnologyGmbH](https://github.com/FujitsuEnablingSoftwareTechnologyGmbH))
Rackspace            | CoreOS       | CoreOS | flannel     | [docs](/docs/getting-started-guides/rackspace)                              |          | Community ([@doublerr](https://github.com/doublerr))
any                  | any          | any    | any         | [docs](/docs/getting-started-guides/scratch)                                |          | Community ([@erictune](https://github.com/erictune))


*注意*：以上表格按照支持级别和测试及使用的版本进行排序。

表格中列说明：

- **IaaS Provider** 是指提供Kubernetes运行环境的虚拟机或物理机（节点）资源的提供商。
- **OS** 是指节点上运行的操作系统。
- **Config. Mgmt** 是指节点上安装和管理 Kubernetes 软件的的配置管理系统。
- **Networking** 是指实现[网络模型](/docs/admin/networking)的软件。
  _none_ 表示只支持一个节点，或支持单物理节点上的虚拟机节点。
- **Conformance** 表示使用该种配置创建的集群是否通过了项目一致性测试，支持 Kubernetes v1.0.0的 API 和基本特性。
- Support Levels (支持级别)
  - **Project**: Kubernetes 贡献者们经常使用该配置，所以通常最新的版本可使用。
  - **Commercial**: 某些厂商负责在自己的平台支持。
  - **Community**: 在社区中有活跃支持，但可能最新版本不适用。
  - **Inactive**: 对于初次使用Kubernetes的用户不推荐，并且有可能在将来被移除。
- **Notes** 比如适用的Kubernetes版本。


<!-- reference style links below here -->
<!-- GCE conformance test result -->
[1]: https://gist.github.com/erictune/4cabc010906afbcc5061
<!-- Vagrant conformance test result -->
[2]: https://gist.github.com/derekwaynecarr/505e56036cdf010bf6b6
<!-- GKE conformance test result -->
[3]: https://gist.github.com/erictune/2f39b22f72565365e59b
