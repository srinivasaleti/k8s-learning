# Ingress Rules in Kubernetes

Ingress rules define how external HTTP/HTTPS requests should be routed to services within a Kubernetes cluster. These rules are specified in the **Ingress resource** and managed by an Ingress controller.

---

## Components of Ingress Rules

1. **Host**

   - Specifies the domain name (optional).
   - Example: `api.example.com`.
   - If omitted, the rule applies to all incoming requests.

2. **Paths**

   - Defines the URL path patterns to match.
   - Each path is associated with a backend service.
   - Supports path types:
     - `Exact`: Matches the path exactly.
     - `Prefix`: Matches based on the path prefix.
     - `ImplementationSpecific`: Delegates path matching to the Ingress controller.

3. **Backend**
   - Specifies the service and port to route the traffic to.
   - Example:
     ```yaml
     backend:
       service:
         name: my-service
         port:
           number: 80
     ```

---

## Default Backend in Kubernetes Ingress

The **default backend** is a fallback service for requests that don't match any Ingress rules.

1. **Fallback Mechanism**: Handles unmatched requests (e.g., 404 errors).
2. **Ingress Controller Specific**: Configuration depends on the controller (e.g., NGINX, Traefik).

## Example Ingress Rule

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  defaultBackend:
    service:
      name: default-backend
      port:
        number: 80
  rules:
    - host: "example.com"
      http:
        paths:
          - path: "/"
            pathType: Prefix
            backend:
              service:
                name: frontend-service
                port:
                  number: 80
          - path: "/api"
            pathType: Prefix
            backend:
              service:
                name: backend-service
                port:
                  number: 8080
```

Explanation:

- Traffic to `example.com/` routes to frontend-service on port 80.
- Traffic to `example.com/api` routes to backend-service on port 8080.
