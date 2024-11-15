# Kubernetes Pods - Notes

## Introduction to Pods
- **Pods** are the smallest deployable units in Kubernetes.
- They encapsulate one or more containers.
- A Pod is a single instance of an application.
- Kubernetes does not deploy containers directly; instead, containers are encapsulated in Pods.

## Pod and Application Scaling
- **Scaling Up**: To handle increased load, create additional Pods, not additional containers within the same Pod.
  - Each Pod typically has a **one-to-one relationship** with the application container it runs.
  - To scale up, create more Pods.
- **Scaling Down**: Delete the existing Pods.

## Multi-Container Pods
- **Single-Container Pods**: The most common use case where a single application container is sufficient.
- **Multi-Container Pods**: Less common and used when additional containers are needed to perform supporting tasks.
  - These additional containers are called **helper containers**.
  - Examples include processing data or files that support the primary application container.
  - Containers within the same Pod share the same **network space** and **storage space**.
  - Containers in the same Pod communicate with each other using `localhost` and are created or destroyed together.

## Docker Containers vs. Kubernetes Pods
- In Docker, you handle the deployment, networking, volume sharing, and lifecycle management manually.
- With Kubernetes:
  - Pods handle these concerns automatically.
  - Containers in a Pod share resources like storage and network.
  - Pods offer a more organized and scalable approach to application management.

## Pod Deployment and Management
- **kubectl run**: Command to deploy a Docker container by creating a Pod.
  - Example: `kubectl run nginx --image=nginx`
  - Kubernetes pulls the Docker image from a repository like Docker Hub.
- **kubectl get pods**: Command to list all Pods in the cluster.
  - The Pod's status can be `ContainerCreating`, `Running`, etc.
  - Initially, a Pod might not be accessible externally without additional configuration.

## Key Takeaways
1. **Scaling Applications**: Use additional Pods for scaling, not more containers within a Pod.
2. **Networking**: Multi-container Pods share the same network space, making internal communication easier.
3. **Storage**: Containers in a Pod can share storage resources.
4. **Lifecycle**: All containers in a Pod are created and destroyed together.