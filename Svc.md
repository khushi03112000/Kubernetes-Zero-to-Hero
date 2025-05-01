## 🧠 **Understanding Kubernetes IPs and Architecture**

### 🔹 **1. Hierarchy of Cluster Components**
```
Kubernetes Cluster
│
├── Node (Master & Worker)
│   └── Pod(s)
│       └── Container(s)
```

- **Cluster**: A set of nodes (VMs or machines).
- **Node**: A worker machine (VM/physical). Each node runs Pods.
- **Pod**: The smallest deployable unit in Kubernetes. Contains 1 or more containers.

---

### 📍**2. IP Address Types in Kubernetes**

| IP Type     | Belongs To | Who Uses It                     | Visibility                     |
|-------------|------------|----------------------------------|--------------------------------|
| **ClusterIP** | Service    | Used inside the cluster          | Accessible only within cluster |
| **Node IP**  | Node       | Used to access services via NodePort | External or internal access    |
| **Pod IP**   | Pod        | Assigned by CNI (network plugin) | Internal use, dynamic, short-lived |

#### ➤ **Pod IP**
- Assigned when Pod is created.
- Changes if Pod restarts or moves.
- Not suitable for stable communication.

#### ➤ **Node IP**
- The IP address of the physical/virtual machine.
- Used in NodePort services to route external traffic.

#### ➤ **ClusterIP**
- Virtual IP created by Kubernetes.
- Used by other services or pods inside the cluster to talk to this service.

---

## 🧪 **3. Types of Kubernetes Services**

### 🔸 A. **ClusterIP (Default)**
> Exposes the service **internally** in the cluster.

```yaml
spec:
  type: ClusterIP
  ports:
    - port: 80         # Service port
      targetPort: 5000 # Pod port
```

- Accessed only by **other pods/services**.
- Not visible to external users.
- Ideal for internal microservice communication.

📌 **Example**: A backend service consumed by a frontend inside the same cluster.

---

### 🔸 B. **NodePort**
> Exposes the service on **a static port on each Node IP**.

```yaml
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 5000
      nodePort: 30036
```

- Can be accessed externally: `http://<NodeIP>:30036`
- Node forwards the request to the Pod.

📌 **Example**: For development or demo, when LoadBalancer isn’t available.

---

### 🔸 C. **LoadBalancer**
> Works on cloud providers (AWS, GCP, Azure). Creates a public **external IP**.

```yaml
spec:
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: 5000
```

- Cloud provider assigns an **external IP**.
- Best for production; supports load balancing and auto-scaling.

📌 **Example**: Exposing your Flask or Node.js app to the internet.

---

## 📊 Diagram: Cluster → Nodes → Pods + Service IPs

```plaintext
                     +--------------------------+
                     |     Kubernetes Cluster   |
                     +--------------------------+
                                |
            +------------------+------------------+
            |                                     |
       +---------+                         +-------------+
       | Node 1  |                         | Node 2      |
       | (Worker)|                         | (Worker)    |
       +---------+                         +-------------+
            |                                     |
   +-------------------+                +-------------------+
   | Pod A             |                | Pod C             |
   | IP: 10.244.0.5     |                | IP: 10.244.1.6     |
   | Container (5000)  |                | Container (5000)  |
   +-------------------+                +-------------------+

   ClusterIP SVC: 10.96.0.100
   NodePort SVC: NodeIP:30036
   LoadBalancer: ExternalIP: (Cloud Assigned)
```

---

## Practical Commands to Try:

```bash
kubectl get nodes -o wide          # Check Node IP
kubectl get svc                    # Check Cluster IP
minikube ssh                      # SSH into Minikube
curl <Cluster-IP>:<port>          # Will work inside minikube
curl <Node-IP>:<NodePort>         # Will work from your local machine
```

---

+----------------------------------------------------+
| Node (Minikube VM)                                 |
| Internal IP: 192.168.49.2 (Node IP)               |
|                                                    |
|  +--------------------+                           |
|  | Pod A              |                           |
|  | Pod IP: 10.244.0.5 |                           |
|  +--------------------+                           |
|                                                    |
|  +--------------------+                           |
|  | Pod B              |                           |
|  | Pod IP: 10.244.0.6 |                           |
|  +--------------------+                           |
|                                                    |
+----------------------------------------------------+
