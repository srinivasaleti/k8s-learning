# Notes on ReplicaSet Controller (Using NGINX Image)

## Step 1: Create a ReplicaSet
### YAML Manifest Example
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
spec:
  replicas: 3  # Number of Pod replicas
  selector:
    matchLabels:
      app: nginx  # Selector for Pods
  template:
    metadata:
      labels:
        app: nginx  # Labels for the Pods
    spec:
      containers:
        - name: nginx
          image: nginx:latest  # Using the nginx image
          ports:
            - containerPort: 80
```

Note: save it in `practice/replica-set/nginx-replica-set.yaml`

* Apply the Manifest

```
kubectl apply -f practice/replica-set/nginx-replica-set.yaml
```

## Step 2: Verify the ReplicaSet

**Check ReplicaSet Status**

```
kubectl get replicaset
```

- This shows the number of Pods created and their status.


**Check Pod Status**

```
kubectl get pods
```

## Step 3: Scale the ReplicaSet

**Increase or Decrease Replicas**

```
kubectl scale replicaset nginx-replicaset --replicas=5
```

- Adjust the number of pods running to 5

**Verify the Scaling**

```
kubectl get replicaset
kubectl get pods
```

## Step 4: Update the ReplicaSet

```
kubectl set image replicaset nginx-replicaset nginx=nginx:1.21
```

## Step 5: Delete the ReplicaSet

```
kubectl delete replicaset nginx-replicaset
```