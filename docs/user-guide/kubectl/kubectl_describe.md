---
---

## kubectl describe

Show details of a specific resource or group of resources

### 摘要


Show details of a specific resource or group of resources.

This command joins many API calls together to form a detailed description of a
given resource or group of resources.

$ kubectl describe TYPE NAME_PREFIX

will first check for an exact match on TYPE and NAME_PREFIX. If no such resource
exists, it will output details for every resource that has a name prefixed with NAME_PREFIX

Possible resource types include (case insensitive): pods (po), services (svc), deployments,
replicasets (rs), replicationcontrollers (rc), nodes (no), events (ev), limitranges (limits),
persistentvolumes (pv), persistentvolumeclaims (pvc), resourcequotas (quota), namespaces (ns),
serviceaccounts (sa), ingresses (ing), horizontalpodautoscalers (hpa), daemonsets (ds), configmaps,
componentstatuses (cs), endpoints (ep), and secrets.

```
kubectl describe (-f FILENAME | TYPE [NAME_PREFIX | -l label] | TYPE/NAME)
```

### 示例

```
# Describe a node
kubectl describe nodes kubernetes-minion-emt8.c.myproject.internal

# Describe a pod
kubectl describe pods/nginx

# Describe a pod identified by type and name in "pod.json"
kubectl describe -f pod.json

# Describe all pods
kubectl describe pods

# Describe pods by label name=myLabel
kubectl describe po -l name=myLabel

# Describe all pods managed by the 'frontend' replication controller (rc-created pods
# get the name of the rc as a prefix in the pod the name).
kubectl describe pods frontend
```

### 选项

```
  -f, --filename=[]: Filename, directory, or URL to a file containing the resource to describe
      --include-extended-apis[=true]: If true, include definitions of new APIs via calls to the API server. [default true]
  -R, --recursive[=false]: Process the directory used in -f, --filename recursively. Useful when you want to manage related manifests organized within the same directory.
  -l, --selector="": Selector (label query) to filter on
      --show-events[=true]: If true, display events related to the described object.
```

{% include_relative parent_commands.md %}

### 参见

* [kubectl](kubectl.md)	 - 使用kubectl来管理Kubernetes集群。
