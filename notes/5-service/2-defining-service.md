# Defining a Service in Kubernetes

A **Service** is a Kubernetes object, similar to a Pod or ConfigMap. You can create, view, or modify Services using the Kubernetes API. Tools like `kubectl` simplify interactions with this API.

## Example: Creating a Service

Consider a set of Pods listening on TCP port 9376 and labeled with `name=MyApp`. Below is an example of a Service definition to expose that port:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    name: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```

## Explanation:
- **Service Name:** The Service is named my-service
- **Selector:** The Service targets Pods with the label app.kubernetes.io/name=MyApp.
- **Ports:**
    - port: The port on which the Service is accessible.
    - targetPort: The port on the Pods to which traffic is forwarded. By default, targetPort equals port for convenience.

## What Happens When You Apply This Manifest?
- A Service named my-service is created with the default ClusterIP type.
- Kubernetes assigns a cluster IP address to the Service, enabling internal communication via a virtual IP mechanism.
- The Service forwards traffic from port: `80` to targetPort: `9376` on matching Pods.
- The Service controller continuously monitors for Pods matching the selector and updates the EndpointSlices to maintain correct routing.