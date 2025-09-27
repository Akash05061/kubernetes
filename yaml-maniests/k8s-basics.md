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
