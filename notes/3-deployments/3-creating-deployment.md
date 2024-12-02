A **Deployment** in Kubernetes is used to manage application updates, scaling, and availability. In this guide, we will create a Deployment that runs three replicas of an `nginx` Pod.

``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

*Save it in `/practice/deployment/nginx-deployment.yaml`*


# Steps to Create and Manage the Deployment

## 1. Apply the Deployment
Use the kubectl apply command to create the Deployment:

```
kubectl apply -f /practice/deployment/nginx-deployment.yaml
```
**What Happens:** Kubernetes creates a Deployment, which in turn creates a ReplicaSet to manage three nginx Pods.

## 2. Monitor Deployment Status
You can watch the Deployment status in real-time using:

```
kubectl get deployments -w
```

## 3. Verify Pods
To check the Pods created by the Deployment, run:

```
kubectl get pods
```

Expected Output: Three nginx Pods should be listed with unique names (e.g., nginx-deployment-xxxxx).

## 4.Test Self-Healing
Kubernetes Deployments are self-healing. You can test this by deleting a pod and observing how Kubernetes recreates it:

```
kubectl delete pod <pod_name>
```

- Replace <pod_name> with the name of one of the Pods listed in the previous step.
- Verify that a new pod is created

```
kubectl get pods
```
**Why This Happens:** The ReplicaSet ensures the desired number of replicas (three in this case) are always running. When a pod is deleted, a new one is created to maintain the desired state.

## 5.Scale the Deployment
To change the number of replicas, update the Deployment using the kubectl scale command:

```
kubectl scale deployment nginx-deployment --replicas=5
```

**What Happens:** Kubernetes adjusts the number of running Pods to match the desired count (5 in this case).

Verify the updated Pods:

```
kubectl get pods
```

## 6. Update the Deployment

To update the container image in the Deployment, modify the YAML file or use the kubectl set command:

```
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.0
```

**What Happens:** Kubernetes performs a rolling update to replace the old Pods with new ones running the updated container image.

Monitor the update:

```
kubectl rollout status deployment/nginx-deployment
```

## 7. Rollback a Deployment

If something goes wrong during an update, you can roll back to the previous version:

```
kubectl rollout undo deployment/nginx-deployment
```

**What Happens:** Kubernetes reverts the Deployment to its previous state.

## 8. Delete deployment

To delete deployment run 

```
kubectl delete deployment nginx-deployment
```

or 

```
 kubectl delete -f practice/deployment/nginx-deployment.yaml
```

# Summary of Kubernetes Deployment Commands

| **Action**                      | **Command**                                           |
|---------------------------------|-------------------------------------------------------|
| Apply a Deployment              | `kubectl apply -f /practice/deployment/nginx-deployment.yaml` |
| Monitor Deployment              | `kubectl get deployments -w`                         |
| Scale Deployment                | `kubectl scale deployment nginx-deployment --replicas=<count>` |
| Update Container Image          | `kubectl set image deployment/nginx-deployment nginx=<image>` |
| Rollback to Previous Version    | `kubectl rollout undo deployment/nginx-deployment`   |
