# Kubernetes Basics - 

## Pod
- Smallest deployable unit in Kubernetes.
- Runs one or more containers.
- Has its own IP but is ephemeral (can be restarted or recreated).

## Deployment
- Manages Pods and ensures the desired number are running.
- Handles updates, rollbacks, and scaling automatically.

## Service
- Provides a stable IP or DNS name to access Pods.
- Allows access from inside or outside the cluster depending on type.
- Handles load balancing across Pods.


## Ingress
- Ingress defines **rules to route external HTTP/S traffic** to services inside the cluster.
- Requires an **Ingress Controller** (like nginx-ingress) to work.
- Can route traffic based on **hostnames or paths**.
- Note: On Windows with Docker driver, Ingress **cannot be accessed directly from the host**; use port-forward or tunnel.

---

## Volumes

### 1. emptyDir
- A temporary directory created **inside the Pod**.
- Data exists **only while the Pod is running**; deleted when Pod is removed.
- Useful for **caching, scratch data, or intermediate storage** between containers in the same Pod.

### 2. hostPath
- Mounts a **directory from the host node** into the Pod.
- Data persists even if the Pod is deleted.
- Useful for accessing **host files, logs, or custom directories**.
- Must be used carefully, as it **gives the Pod access to the host filesystem**.

---

## Quick Comparison

| Volume Type | Persistence | Scope            | Use Case                  |
|-------------|------------|-----------------|---------------------------|
| emptyDir    | Temporary  | Pod-only        | Cache, scratch space      |
| hostPath    | Persistent | Host & Pod      | Logs, host files, custom paths |

# Persistent Storage

## PersistentVolume (PV)
- A PV is a **cluster-wide storage resource**.
- Created by the admin or dynamically by StorageClass.
- Can be backed by **hostPath, NFS, cloud disks, or other storage types**.
- Provides **persistent storage** independent of Pod lifecycle.

## PersistentVolumeClaim (PVC)
- A PVC is a **request for storage by a Pod**.
- Specifies **size, access mode, and storage class**.
- Kubernetes matches the PVC with an available PV.
- Pod uses the PVC as a volume; data persists even if the Pod is deleted.

---

## Quick Flow
1. Admin creates PV (or StorageClass for dynamic provisioning).  
2. Developer creates PVC specifying required storage.  
3. Kubernetes binds PVC to a suitable PV.  
4. Pod mounts PVC to access persistent storage.

---

## Quick Comparison

| Resource | Purpose                        | Lifecycle           |
|----------|--------------------------------|------------------|
| PV       | Provides persistent storage     | Independent of Pod |
| PVC      | Requests storage for a Pod      | Tied to Pod usage  |

# StorageClass

## StorageClass
- Defines **how dynamic storage is provisioned** in a cluster.
- Specifies **provisioner**, **parameters**, and **reclaim policy**.
- Used when a PersistentVolumeClaim (PVC) requests storage dynamically.
- Examples of provisioners:  
  - AWS EBS → `kubernetes.io/aws-ebs`  
  - GCP PD → `kubernetes.io/gce-pd`  
  - NFS → `kubernetes.io/nfs`  
  - Local → `kubernetes.io/no-provisioner`

## Key Fields
- `provisioner` → the type of storage backend.  
- `parameters` → backend-specific settings (e.g., disk type, replication).  
- `reclaimPolicy` → what happens to the storage when PVC is deleted (`Delete` or `Retain`).  
- `volumeBindingMode` → when the PV is bound (`Immediate` or `WaitForFirstConsumer`).

## Flow
1. Developer creates a PVC **without specifying a PV**.  
2. Kubernetes uses StorageClass to **dynamically create a PV**.  
3. PVC is bound to the dynamically created PV.  
4. Pod uses the PVC for persistent storage.

---

## Quick Comparison

| Concept        | Purpose                                      |
|----------------|---------------------------------------------|
| PV             | Provides actual storage                      |
| PVC            | Requests storage for a Pod                   |
| StorageClass   | Defines **how storage should be provisioned** dynamically |


