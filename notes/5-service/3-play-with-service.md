# Managing NGINX with Kubernetes: Pod, Deployment, and Service

In this guide, we will go through the steps to:
- Create a **Pod** using a **Deployment**
- Attach a **Service** to the Deployment
- Start and list the **Service**
- Access **NGINX** via the Service
- Delete the **Service**

---

## Step 1: Create a Pod using Deployment

To create a Pod with NGINX using a **Deployment**, we need to define the Deployment YAML file. A Deployment will automatically manage the Pods based on the defined specifications.

### Deployment YAML Definition:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: service-test1-nginx-deployment ## Just for testing giving this name
spec:
  replicas: 3 
  selector:
    matchLabels:
      app: service-test1-nginx-app 
  template:
    metadata:
      labels:
        app: service-test1-nginx-app
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80  # Expose port 80 on the container
```

Keep it under `practice/service/test1/nginx-deployment.yaml`

To create the Deployment and Pods:

```bash
kubectl apply -f practice/service/test1/nginx-deployment.yaml
```

## Step 2: Attach a Service to the Deployment

A Service is used to expose the NGINX Pods so they can be accessed within the Kubernetes cluster or externally (depending on the type of Service).


```yaml
apiVersion: v1
kind: Service
metadata:
  name: service-test1-nginx-service
spec:
  selector:
    app: service-test1-nginx-app  # Select Pods with the matching label
  ports:
    - protocol: TCP
      port: 80  # The port exposed by the Service
      targetPort: 80  # The port on the Pods to forward traffic to
  type: ClusterIP  # Default service type (internal only)
```

Keep it under `practice/service/test1/nginx-service.yaml`

- To create the Service:

```bash
kubectl apply -f practice/service/test1/nginx-service.yaml
```

- Once the Service is defined and applied, Kubernetes automatically starts the Service. You don't need to manually start it.

## Step3: Check service status
- To list all the services, use the following command:

```bash
kubectl get service
```

- To check the status of the Service:

```bash
kubectl get service service-test1-nginx-service
```

## Step 4: Access NGINX via the Service
By default K8s creates a ClusterIP service, which can be accessed within the cluster. If you need to access the service externally (from outside the Kubernetes cluster), and you're using a type LoadBalancer or NodePort service, you would use the external IP or port, respectively.

If you're using ClusterIP (internal-only), you can port-forward to access the service from outside the cluster:

```bash
kubectl port-forward svc/service-test1-nginx-service 8080:80
```

Now you can access NGINX at `http://localhost:8080` in your browser.

## Step5: Delete service
If you no longer need the Service, you can delete it using the following command:

```bash
kubectl delete svc service-test1-nginx-service
```

This will remove the Service, but the Pods created by the Deployment will continue running unless you also delete the Deployment.

```bash
kubectl delete deployment service-test1-nginx-deployment
```

**or**

If you want to delete all the resources at once just run

```bash
kubectl delete -f practice/service/test1
```