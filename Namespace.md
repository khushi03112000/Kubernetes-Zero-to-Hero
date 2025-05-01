## kubectl get all

### Command:
```bash
kubectl get all
```

This will show:
- All resources (Pods, Services, Deployments, ReplicaSets, StatefulSets, etc.)
- But only from the *current* namespace (by default — `default` namespace).

---

## kubectl get all -A

### Command:
```bash
kubectl get all -A
```

`-A` means "All Namespaces"

This shows resources from *every* namespace in your cluster.

---

## What is a Namespace in Kubernetes?

> Namespace = Virtual Environment / Logical Grouping within your Kubernetes Cluster.

Think of it like this:
- In one Kubernetes Cluster, you might want to separate:
  - Dev Environment
  - QA Environment
  - Prod Environment
- Or separate teams / projects.

Each of these can be a separate `namespace`.

---

## Real-life Analogy:
> One House = Kubernetes Cluster  
> Different Rooms = Namespaces  
> Resources in each room = Pods, Services, etc.  

You can have:
- Same resource names in different namespaces.
- But they won't clash because they are in separate rooms (namespaces).

---

## Built-in Namespaces in Kubernetes:

| Namespace | Purpose |
|-----------|---------|
| default   | Used when you don't specify any namespace. |
| kube-system | Used by Kubernetes internal components (CoreDNS, etc.). |
| kube-public | Generally open for all (not often used). |
| kube-node-lease | For node heartbeats / internal stuff. |

---

## Commands Cheat Sheet:

| Command | Purpose |
|---------|---------|
| kubectl get all | All resources in *default* namespace. |
| kubectl get all -A | All resources in *all* namespaces. |
| kubectl get pods -n kube-system | Pods in `kube-system` namespace. |
| kubectl get namespaces | List all namespaces. |
| kubectl create ns dev | Create a namespace `dev`. |
| kubectl config set-context --current --namespace=dev | Set default namespace to `dev` for current context. |

---

## Diagram:

```
+------------------------------------+
| Kubernetes Cluster                 |
|                                    |
| +-----------+   +-------------+    |
| | Namespace |   | Namespace   |    |
| | default   |   | dev         |    |
| |           |   |             |    |
| | Pods, SVC |   | Pods, SVC   |    |
| +-----------+   +-------------+    |
|                                    |
+------------------------------------+
```

---

## Final Tip:
Always check which namespace you are working in!

```bash
kubectl config view --minify | grep namespace
```


---

