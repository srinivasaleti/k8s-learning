# What is Ingress in Kubernetes?

An **Ingress** is a Kubernetes API object that manages external access to services within a Kubernetes cluster. It provides rules for how external HTTP and HTTPS traffic is routed to the services in the cluster.

---

# Why Do We Need Ingress if We Have a LoadBalancer Service?

## 1. **Cost Efficiency**

- **LoadBalancer Service**: Requires a public IP for each service, especially in cloud environments. As the number of services grows, the cost increases significantly because each LoadBalancer service creates its own external load balancer.
- **Ingress**: Introduced to reduce these costs by using a single public IP to route traffic to multiple services within the cluster.

## 2. **Advanced Features**

- **LoadBalancer Service**: Offers basic traffic management, but lacks enterprise-grade functionalities.
- **Ingress**: Provides advanced features like:
  - URL/path-based routing (e.g., `/api` or `/app`).
  - Domain-based routing (e.g., `api.example.com`).
  - SSL/TLS termination.
  - Support for enterprise-grade load balancer implementations like NGINX, F5, or Traefik, which offer better performance, scalability, and additional features such as rate limiting and advanced traffic policies.

---

These two reasons highlight why Ingress is a more scalable, cost-effective, and feature-rich solution compared to using multiple LoadBalancer services.

## Key Features of Ingress

1. **HTTP and HTTPS Routing**

   - Ingress defines rules to direct traffic to services based on domain names, paths, or other criteria.

2. **TLS/SSL Support**

   - Enables secure communication by supporting SSL/TLS termination.

3. **Centralized Access Management**

   - Acts as a single entry point for multiple services, simplifying external access configuration.

4. **Load Balancing**

   - Distributes traffic across pods for high availability.

5. **Advanced Routing**
   - Supports advanced features like URL path rewrites, redirects, and rate limiting.

---

## Traffic Flow with Ingress

1. **Client Request**: An external client sends an HTTP or HTTPS request.
2. **Ingress Controller**: The Ingress Controller processes the request based on the rules defined in the Ingress resource.
3. **Service**: The request is forwarded to the appropriate service inside the cluster.
4. **Pod**: The service routes the request to the appropriate pod.

```
Client (External User)
        |
        v
 +-------------------+
 | Load Balancer     |  (Single Public IP)
 +-------------------+
        |
        v
 +-------------------+
 | Ingress Controller|
 | (e.g., NGINX, F5) |
 +-------------------+
        |     |     |
        v     v     v
+-------+  +-------+  +-------+
|Service1|  |Service2|  |Service3|
+-------+  +-------+  +-------+
        |     |     |
        v     v     v
     +Pod+  +Pod+  +Pod+
```

---

## Example Use Case

Suppose you have two services in your cluster:

1. A **frontend** application.
2. An **API backend**.

You can configure Ingress rules as follows:

- `http://example.com` routes traffic to the frontend service.
- `http://example.com/api` routes traffic to the backend service.

---

## Benefits of Using Ingress

1. **Simplifies External Access**
   - Provides a unified way to manage access to multiple services.
2. **Reduces Costs**

   - Eliminates the need for individual external IPs for each service.

3. **Enhances Security**

   - Centralized TLS/SSL termination ensures secure communication.

4. **Improves Performance**
   - Load balancing and caching capabilities optimize resource usage.
