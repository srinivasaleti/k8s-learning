
## What is a NodePort Service?

A `NodePort` service is a Kubernetes service type that allows external access to services running in the cluster by exposing a static port on each node. The service can be accessed through the `<NodeIP>:<NodePort>` combination. It is useful for:

- Debugging applications.
- Environments without a cloud-based LoadBalancer.
- Exposing services to external traffic.

## Key Features of `NodePort`
1. **Port Allocation**:
   - Kubernetes allocates a port from the default range of **30000-32767** (configurable via the `--service-node-port-range` flag).
   - The assigned port is reported in the `.spec.ports[*].nodePort` field of the service.

2. **Accessibility**:
   - A `NodePort` Service can be accessed externally by connecting to any node's IP and the allocated port.
   - Each node in the cluster listens on the assigned port and proxies the traffic to the appropriate backend Pods.

3. **Load Balancing**:
   - While Kubernetes handles traffic routing internally, `NodePort` services provide the flexibility to set up custom external load balancers.

4. **Protocol Support**:
   - Supports multiple protocols, such as TCP, UDP, and SCTP.

## Why Use `NodePort`?
- **Custom Load Balancers**: Ideal for implementing custom load balancing solutions outside Kubernetes.
- **Direct Node Access**: Enables direct exposure of node IP addresses and ports for specific use cases.
- **Flexibility**: Useful for integrating Kubernetes with environments or configurations not fully supported by Kubernetes.

## NodePort Service Configuration Example

Below is an example of a NodePort service configuration:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: node-port-service-nginx # Service name following the node-port-service convention
spec:
  selector:
    app: node-port-service-nginx-app # Selects Pods with the matching label
  ports:
    - protocol: TCP
      port: 80 # The port exposed by the Service
      targetPort: 80 # The port on the Pods to forward traffic to
      nodePort: 30080 # The static NodePort (optional; will auto-assign if not specified)
  type: NodePort # Service type for external access
```

## Testing

- Deployment, node port service yaml files kept in `practice/service/nodeport`
- To apply run : `kubectl apply -f practice/service/nodeport`
- Verify services running: `kubectl get service`

## Accessing NodePort Services on `localhost` in Minikube

### Why Do We Need This?

Minikube runs Kubernetes inside a virtual machine or container, which has its own internal IP (e.g., `192.168.49.2`). By default, services exposed using **NodePort** are accessible through the **Node IP** and the **NodePort**, but not directly on `localhost`. To make it easier to test and debug services locally, we use Minikube's built-in commands to access them via `localhost`.

### Using Minikube's Built-In Service Command

Minikube provides a convenient command to access services exposed by Kubernetes. This method ensures you can access the service directly on your local machine's `localhost`, without worrying about the internal Minikube IP.

```
minikube service node-port-service-nginx
```

This command:

- Automatically finds the service's NodePort.
- Maps it to localhost and opens it in your default browser

Verify Access Minikube will open a URL like: `http://localhost:<some-port>`

## Delete service created
- To delete services and pods created run: `kubectl delete -f practice/service/nodeport`