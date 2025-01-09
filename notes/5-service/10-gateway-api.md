# Kubernetes Gateway API

## Why?

- The Kubernetes Gateway API improves upon the Ingress API by offering:
  - **Extensibility**: Easily extendable for custom protocols and features.
  - **Granularity**: Fine-grained control over traffic routing, load balancing, and policies.
  - **Protocol Support**: Not limited to HTTP/HTTPS; supports TCP, UDP, and TLS.
  - **Separation of Concerns**: Infrastructure (Gateways) and routing rules (Routes) are decoupled.
  - **Advanced Use Cases**: Enables sophisticated traffic management like header-based routing and canary deployments.

## What?

The Gateway API is a set of resources for managing external and internal network traffic in Kubernetes clusters.

### Key Components:

1. **GatewayClass**:
   - A cluster-wide resource defining a type of gateway (e.g., Istio, AWS ALB).
   - Example: AWS Load Balancer or NGINX ingress controller.
2. **Gateway**:

   - Represents a logical gateway, listening for incoming traffic on specified ports and protocols.
   - Example: Configuring a Gateway to listen on port 443 for HTTPS traffic.

3. **Routes**:

   - Define how traffic from a Gateway is routed to backend services.
     - **HTTPRoute**: Routes HTTP/HTTPS traffic based on headers, paths, or query parameters.
     - **TCPRoute**: Routes TCP traffic like database connections.
     - **UDPRoute**: Routes UDP traffic for gaming or DNS.
     - **TLSRoute**: Routes encrypted HTTPS traffic based on SNI.

4. **ReferenceGrant**:

   - Grants permissions for resources in one namespace to reference resources in another.

5. **BackendPolicy**:
   - Adds configurations like retries, timeouts, or health checks for backend services.

## How?

1. **Set Up a GatewayClass**:
   - Define an implementation-specific Gateway controller (e.g., Istio, AWS).
   ```yaml
   apiVersion: gateway.networking.k8s.io/v1beta1
   kind: GatewayClass
   metadata:
     name: istio
   spec:
     controllerName: istio.io/gateway-controller
   ```
2. **Create a Gateway**:

   - Specify where traffic enters the cluster (e.g., port, protocol)

   ```yaml
   apiVersion: gateway.networking.k8s.io/v1beta1
   kind: Gateway
   metadata:
   name: example-gateway
   spec:
   gatewayClassName: istio
   listeners:
   - name: https
       protocol: HTTPS
       port: 443
   ```

3. **Define an HTTPRoute:**

   - Route traffic based on path or header.

   ```yaml
   apiVersion: gateway.networking.k8s.io/v1beta1
   kind: HTTPRoute
   metadata:
   name: header-routing
   spec:
   parentRefs:
   - name: example-gateway
   rules:
   - matches:
       - headers:
       - name: X-Custom-Header
           value: "special-user"
       backendRefs:
       - name: special-service
       port: 80
   ```

## Use Cases

### 1. **Header-Based Routing**

Route traffic to different backend services based on specific HTTP headers, such as routing premium users or specific device types to dedicated services.

---

### 2. **Path-Based Routing**

Route traffic based on specific URL paths, enabling services to handle requests to distinct APIs or endpoints.

---

### 3. **Canary Releases**

Gradually shift traffic to a new version of a service, allowing testing and validation before a full release.

---

### 4. **Multi-Tenant Gateways**

Support multiple tenants sharing a single Gateway, allowing each tenant to define their own routes securely.

---

### 5. **Load Balancing with TCPRoute**

Distribute traffic for TCP-based applications (e.g., databases) across multiple backends.

---

### 6. **Custom TLS Routing**

Route encrypted traffic based on SNI (Server Name Indication), providing domain-specific routing for HTTPS.

---

### 7. **Weighted Routing for A/B Testing**

Distribute traffic between two or more services to conduct A/B testing or gradual feature rollouts.

---

### 8. **Traffic Mirroring**

Duplicate incoming traffic to another backend for testing or debugging without impacting the primary service.

---

### 9. **Protocol-Specific Routing**

Route traffic for specific protocols, such as WebSockets or gRPC, to the appropriate backends.

---

### 10. **Cross-Namespace Routing**

Allow resources in one namespace to securely reference and route to resources in another namespace.

---

### 11. **Blue/Green Deployments**

Enable full traffic routing to a new environment (green) after testing in the existing environment (blue).

---
