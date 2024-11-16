# Kubernetes Architecture

Kubernetes is a powerful container orchestration platform designed to automate the deployment, scaling, and management of containerized applications. Its architecture is based on a **client-server model**, where the Kubernetes cluster consists of **control plane components** and **worker nodes**.

---

## 1. Core Components of Kubernetes

### Control Plane Components

The **Control Plane** is responsible for managing the Kubernetes cluster. It makes global decisions about the cluster, such as scheduling, and detects/responds to cluster events. Key components include:

- **API Server**:
  - The main entry point for all Kubernetes RESTful API requests.
  - Validates and processes API requests from users, kubectl, and other components.
  - **Role**: Acts as the front-end for the Kubernetes control plane and communicates with other control plane components.
  
- **etcd**:
  - A distributed key-value store that stores all cluster data.
  - Acts as the “source of truth” for the cluster’s current and desired state.
  - **Role**: Provides reliable storage for configuration data, allowing control plane components to retrieve and maintain the cluster’s state.

- **Controller Manager**:
  - Runs various controllers (such as the Node, Deployment, and Job controllers) that monitor the cluster and manage the lifecycle of different resources.
  - Each controller watches for changes in the cluster and makes adjustments to align the actual state with the desired state.
  - **Role**: Ensures resources are running and maintained as expected.

- **Scheduler**:
  - Assigns newly created Pods to Nodes based on resource availability, affinity rules, and other constraints.
  - Considers resource requests, Node conditions, and other policies to optimize Pod placement.
  - **Role**: Manages workload distribution to balance resources across the cluster.

### Worker Node Components

Worker nodes, also called **compute nodes**, run applications and manage Pods. Key components on each node include:

- **Kubelet**:
  - A Kubernetes agent running on each worker node.
  - Ensures containers are running within Pods by monitoring and reporting status to the control plane.
  - **Role**: Communicates with the control plane to receive instructions and ensures the desired state for each Pod.

- **Kube-Proxy**:
  - A network proxy that maintains network rules and allows for service discovery and connectivity.
  - Routes traffic destined for Services to appropriate Pods across nodes.
  - **Role**: Manages network communication and load balancing within the cluster.

- **Container Runtime**:
  - Software responsible for running containers (e.g., Docker, containerd).
  - Manages container life cycles and enables communication between containers and the host OS.
  - **Role**: Executes and isolates containerized applications.

---

## 2. Kubernetes Objects

Kubernetes has core objects used to define the desired state of applications:

- **Pod**: The smallest unit in Kubernetes, representing one or more containers that share resources.
- **Service**: An abstraction that defines a logical set of Pods and a policy for accessing them.
- **Deployment**: Manages the deployment and scaling of a set of identical Pods.
- **Namespace**: A way to divide cluster resources for different teams or projects.

---

## 3. Kubernetes Architecture Flow

1. **User Interaction**:
   - Users interact with the cluster through the **kubectl** command-line tool or other API clients.
   - Commands are sent to the **API Server** for processing.

2. **API Server**:
   - The API Server authenticates, validates, and processes requests, then stores the configuration in **etcd**.

3. **Scheduler**:
   - Watches the **API Server** for unassigned Pods and assigns them to appropriate Nodes.

4. **Controller Manager**:
   - Watches the cluster's state through **etcd** and uses controllers to ensure resources meet the desired state.

5. **Worker Node Execution**:
   - The **Kubelet** on each Node pulls the assigned Pod configurations, ensures the container runtime starts the containers, and monitors their health.
   - **Kube-Proxy** configures networking to enable Pod-to-Pod communication within the cluster.

---

## 4. Diagram of Kubernetes Architecture

```plaintext
          +--------------------------------------+
          |              Control Plane           |
          +--------------------------------------+
          |                                      |
          |   +------------------------------+   |
          |   |         API Server           |   |
          |   +------------------------------+   |
          |          |          |                |
          |          |          |                |
          |   +-------------+   +----------+     |
          |   |   etcd      |   | Scheduler|     |
          |   +-------------+   +----------+     |
          |                 |          |         |
          |   +--------------------------------+ |
          |   |       Controller Manager       | |
          |   +--------------------------------+ |
          +--------------------------------------+
                       |          |
                       |          |
       +---------------+----------+-------------+
       |              Worker Nodes              |
       +----------------------------------------+
       |                                        |
       |   +-----------+   +----------------+   |
       |   |  Kubelet  |   |    Kube-Proxy  |   |
       |   +-----------+   +----------------+   |
       |       |                    |           |
       |       |      +-------------+---+       |
       |       |      | Container Runtime |     |
       |       |      +-------------------+     |
       +----------------------------------------+

