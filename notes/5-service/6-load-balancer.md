## Key Features of LoadBalancer Service

1. **Automatic Load Balancer Provisioning:**
   - Kubernetes interacts with the cloud provider's API to automatically create and configure a load balancer.
   - The load balancer is associated with the service and distributes traffic to the appropriate Pods.

2. **External Accessibility:**
   - A public IP address or domain name is assigned to the service, making it accessible to external clients.
   - Clients can connect to the application using the provided IP or domain.

3. **Traffic Distribution:**
   - Incoming traffic is distributed evenly across all healthy Pods associated with the service.
   - Ensures efficient utilization of resources and load balancing.

4. **Cloud Provider Integration:**
   - Works seamlessly with major cloud providers like AWS, GCP, and Azure.
   - Utilizes cloud-specific features such as health checks, SSL termination, and firewall rules.

5. **Supports Multiple Protocols:**
   - Primarily supports TCP and UDP, depending on the cloud provider's load balancer capabilities.

---

## Differences Between `NodePort` and `LoadBalancer` Services

| Feature             | NodePort                          | LoadBalancer                   |
|---------------------|------------------------------------|---------------------------------|
| **Use Case**        | Expose service on each node's IP  | Expose service using a cloud-managed load balancer |
| **Accessibility**   | Access via `<NodeIP>:<NodePort>`  | Access via external load balancer's IP |
| **External Traffic**| Requires manual load balancing    | Automatic external traffic management |
| **Cloud Dependency**| No                               | Yes                            |

---

## Best Practices for LoadBalancer Service

1. **Use Cloud-Specific Annotations:**
   - Configure features like timeouts, health checks, and SSL termination using annotations.
   - Ensure annotations match the cloud provider's requirements.

2. **Limit Unnecessary Exposure:**
   - Use `LoadBalancer` type sparingly to control costs and minimize security risks.

3. **Combine with Ingress:**
   - For complex routing, deploy an Ingress resource alongside the LoadBalancer service.

4. **Health Checks:**
   - Ensure Pods respond correctly to readiness probes to avoid sending traffic to unhealthy Pods.

5. **Monitor and Secure Traffic:**
   - Utilize monitoring tools like Prometheus and Grafana.
   - Apply network policies and firewall rules to secure the service.

## When to Use LoadBalancer Service

- **External Exposure:** When your application needs to be accessible from outside the Kubernetes cluster.
- **High Availability:** For scenarios requiring automatic traffic distribution and failover across multiple Pods.
- **Cloud-Native Applications:** Works best in cloud environments that support managed load balancers.   

## Testing

- Deployment, node port service yaml files kept in `practice/service/loadbalancer`
- To apply run : `kubectl apply -f practice/service/loadbalancer`
- Verify services running: `kubectl get service`

## Accessing NodePort Services on `localhost` in Minikube

### Why Do We Need This?

Minikube runs Kubernetes inside a virtual machine or container, which has its own internal IP (e.g., `192.168.49.2`). By default, services exposed using **NodePort** are accessible through the **Node IP** and the **NodePort**, but not directly on `localhost`. To make it easier to test and debug services locally, we use Minikube's built-in commands to access them via `localhost`.

### Using Minikube's Built-In Service Command

Minikube provides a convenient command to access services exposed by Kubernetes. This method ensures you can access the service directly on your local machine's `localhost`, without worrying about the internal Minikube IP.

```
minikube service loadbalancer-service-nginx
```

This command:

- Automatically finds the service's NodePort.
- Maps it to localhost and opens it in your default browser

Verify Access Minikube will open a URL like: `http://localhost:<some-port>`

## Delete service created
- To delete services and pods created run: `kubectl delete -f practice/service/loadbalancer`