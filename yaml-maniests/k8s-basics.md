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
