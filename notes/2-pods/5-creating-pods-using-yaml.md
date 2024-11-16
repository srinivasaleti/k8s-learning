## Kubernetes Pod Operations with YAML

### 1. Create a YAML File Using Dry Run

To generate the YAML configuration for a Pod without actually creating it in the cluster, use the following command:

```bash
kubectl run nginx-pod --image=nginx --dry-run=client -o yaml > nginx-pod-definition.yaml
```
### 2. Run the Pod

Once you have the YAML file, you can create the Pod in the Kubernetes cluster by applying the YAML file:

```bash
kubectl apply -f nginx-pod-definition.yaml
```

`kubectl apply -f nginx-pod-definition.yaml`: Applies the configuration defined in pod-definition.yaml to create the Pod in the cluster.

### 4. View all pods

To view all pods you can use following command

```bash
kubectl get pods
```

### 5. Describe the Pod
To see detailed information about the Pod you just created, use the following command:

```bash
kubectl describe pod nginx-pod
```

### 6. To delete the pod
Once you are done with the Pod, you can delete it from the cluster using the following command:

```
kubectl delete pod nginx-pod
```

or 

```
kubectl delete -f nginx-pod-definition.yaml
```

This is very useful command when we wanted to delete more than one resources.

To verify if the pod is deleted or not run `kubectl get pods`