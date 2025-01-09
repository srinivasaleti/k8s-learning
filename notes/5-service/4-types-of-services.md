# Types of Services in Kubernetes

Kubernetes provides different types of Services to control how applications are exposed and accessed. Each type has specific use cases and features.

---

## 1. **ClusterIP** (Default)
The **ClusterIP** type exposes the Service only within the cluster. It assigns a stable virtual IP address for internal communication.

- **Use Case**: Internal communication between applications within the cluster.
- **Access**: Not accessible from outside the cluster.
- **Example**:
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: clusterip-service
  spec:
    type: ClusterIP
    selector:
      app: my-app
    ports:
      - protocol: TCP
        port: 80
        targetPort: 8080
    ```

## NodePort

The **NodePort** type exposes the Service on a static port (range: 30000–32767) on each Node’s IP. External clients can access the Service using <NodeIP>:<NodePort>.

- **Use Case**: Quick external access for testing or debugging.
- **Access**: Externally accessible via the Node's IP and port.
- **Example**
    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
    name: nodeport-service
    spec:
    type: NodePort
    selector:
        app: my-app
    ports:
        - protocol: TCP
        port: 80
        targetPort: 8080
        nodePort: 31000
    ```

## LoadBalancer
The **LoadBalancer** type integrates with a cloud provider’s load balancer to expose the Service to the internet. It assigns an external IP that routes traffic to the Service.

- **Use Case**: Public-facing applications that require high availability and scalability.
- **Access**: Accessible via the cloud provider’s load balancer.
- **Example**
    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
    name: loadbalancer-service
    spec:
    type: LoadBalancer
    selector:
        app: my-app
    ports:
        - protocol: TCP
        port: 80
        targetPort: 8080
    ```

## ExternalName
The **ExternalName** type maps a Kubernetes Service to an external DNS name. It does not define any selector or endpoints and is used to redirect traffic to external resources.

- **Use Case**: Connecting to external resources like databases or APIs.
- **Access**: Provides DNS-based redirection to external services.
- **Example**: 
    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
        name: externalname-service
    spec:
        type: ExternalName
        externalName: example.com
    ```

## Summary of Kubernetes Service Types

| **Service Type** | **Access**                        | **Use Case**                                      |
|-------------------|-----------------------------------|--------------------------------------------------|
| **ClusterIP**     | Internal (within the cluster)    | Communication between cluster applications.      |
| **NodePort**      | External (`<NodeIP>:<NodePort>`) | Quick external access for testing or debugging.  |
| **LoadBalancer**  | External (cloud load balancer)   | Public-facing applications requiring scalability.|
| **ExternalName**  | External (DNS redirection)       | Connecting to external resources like databases. |
