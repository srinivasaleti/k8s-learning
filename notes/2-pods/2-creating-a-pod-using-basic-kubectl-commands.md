# Running a Simple Nginx Pod Using Minikube

## Prerequisites
- **Minikube**: Make sure you have Minikube installed. You can install it from [here](https://minikube.sigs.k8s.io/docs/start/).
- **kubectl**: The Kubernetes command-line tool. You can install it from [here](https://kubernetes.io/docs/tasks/tools/).

## Step-by-Step Guide

### 1. Start Minikube
Start your Minikube cluster with the following command:
```bash
minikube start
```

### 2. Create an Nginx Pod

To create a pod running the Nginx image, use the following command:

```
kubectl run nginx-pod --image=nginx --port=80
```

* `nginx-pod`: This is the name of the pod.
* `--image=nginx`: Specifies the image to use (latest Nginx image from Docker Hub).
* `--port=80`: Sets the container port to 80.


### 3. Verify the Pod

To check if the pod is running, use:

```
kubectl get pods
```

### 4. Access the Nginx Pod
To access the running Nginx container, use kubectl port-forward:

```
kubectl port-forward pod/nginx-pod 8080:80
```

Now, you can open your browser and go to http://localhost:8080 to see the default Nginx page.

### 5. Check Logs

To view logs from the Nginx container, use:

```
kubectl logs nginx-pod
```

### 6. Access the Shell Inside the Pod

To access the shell inside the Nginx container, use:

```
kubectl exec -it nginx-pod -- /bin/bash
```

### Delete the Pod

If you're done and want to remove the pod, use:

```
kubectl delete pod nginx-pod
```