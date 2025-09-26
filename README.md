## What is Kubernetes?
Kubernetes is an open source platform that deploys and manages many containers in a virtual environment.
<img width="1511" height="850" alt="image" src="https://github.com/user-attachments/assets/4758b00a-3594-46e6-9434-62accdce4e2b" />


## The Control Plane (Master Node Components)

The Control Plane's components make global decisions about the cluster (like scheduling) and detect and respond to cluster events.

### Kube API Server
* **Role:** The central management point and frontend of the control plane.
* **Functionality:**
    * Exposes the Kubernetes API, allowing users and other components to interact with the cluster.
    * Acts as the gateway for all communications, validating and processing API requests.
    * All interaction with the cluster state (`etcd`) happens through the API Server.

### etcd
* **Role:** The cluster's distributed, consistent key-value database.
* **Functionality:**
    * Stores all cluster data, including its configuration, state, and specifications of all resources.
    * Serves as the single source of truth for the entire cluster.
    * Features a "watch API" that allows other components (like the API server) to be notified of changes in real-time. For security, only the API Server can communicate directly with `etcd`.

### Kube Scheduler
* **Role:** Assigns pods to worker nodes.
* **Functionality:**
    * Watches the API Server for newly created pods that don't have a node assigned.
    * Selects the most suitable worker node for a pod to run on based on resource availability, hardware/software constraints, and user-defined policies.

### Kube Controller Manager
* **Role:** A daemon that runs the core control loops (controllers).
* **Functionality:**
    * Watches the state of the cluster through the API Server and works to move the current state towards the desired state.
    * Includes several controllers, such as:
        * **Replication Controller:** Ensures the correct number of pods are running for a service.
        * **Node Controller:** Monitors the health of worker nodes and reacts if a node goes down.
        * **Endpoint Controller:** Populates the Endpoint object that joins Services and Pods.

## The Data Plane (Worker Node Components)

Every worker node in the cluster runs the following essential components to manage pods and communicate with the control plane.

### Container Runtime
* **Role:** The software responsible for running containers.
* **Functionality:**
    * Pulls container images from a registry, and starts and stops the containers.
    * Kubernetes supports several runtimes, including Docker, containerd, and CRI-O.

### Kubelet
* **Role:** The primary agent that runs on each worker node.
* **Functionality:**
    * Communicates with the API Server to check for new or modified pod specifications.
    * Ensures that the containers described in the PodSpecs are running and healthy on its node.
    * Registers the node with the cluster and reports its status (health, resource utilization) back to the master.

### Kube Proxy
* **Role:** The network proxy and load balancer on each node.
* **Functionality:**
    * Maintains network rules on the host operating system (e.g., using iptables).
    * Enables network communication to pods from sessions inside or outside the cluster.
    * Implements the Kubernetes Service concept, forwarding requests for a service to the correct backend pods.

## Example Workflow: How It All Works Together

This step-by-step process illustrates how the components interact to deploy an application.

1.  **Define and Send Spec:** A user defines the desired state of an application in a specification document (e.g., a YAML file) and sends it to the **Kube API Server** using a tool like `kubectl`.
2.  **Store Configuration:** The **API Server** validates the request and stores the configuration in the **etcd** database. This becomes the "desired state."
3.  **Schedule the Pods:** The **Kube Scheduler** notices the newly created pods without an assigned node. It evaluates the worker nodes and selects the best fit for each pod, then updates the pod information in **etcd** via the API Server.
4.  **Create the Objects:** The **Kubelet** on the assigned worker node, which is constantly watching the API Server, detects that a pod has been assigned to it.
5.  **Run the Containers:** The **Kubelet** instructs the **Container Runtime** (e.g., Docker) to pull the necessary container image and run the containers as defined in the pod specification.
6.  **Update Status:** As the pod starts, the **Kubelet** continuously reports the status of the pod and its containers back to the **API Server**, which updates the "actual state" in **etcd**.
7.  **Enable Networking:** The **Kube Proxy** on the worker node notices the new pod and creates the necessary network rules to allow traffic to reach it through its service.
8.  **Ensure Desired State:** The **Controller Manager** continuously watches the cluster state. If a pod fails or a node goes down, it detects the mismatch between the desired state (in `etcd`) and the actual state. It will then work with the **Scheduler** to launch a new pod to bring the cluster back to the desired state, demonstrating Kubernetes' self-healing capabilities.
