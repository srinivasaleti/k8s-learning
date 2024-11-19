# Kubernetes Init Containers

## What are Init Containers?
Init containers are special containers in Kubernetes that run **before** the main application containers in a Pod. They are used for performing initialization tasks or setting up prerequisites needed by the main containers.

### Key Points
- **Init containers** run before any main (app) containers in a Pod.
- Kubernetes **delays** init containers until networking and storage are ready.
- Init containers run **sequentially** based on the order in the Pod's spec.
- Each init container must **complete successfully** before the next one starts.
- If an init container fails, it's **retried** based on the Pod's `restartPolicy`:
  - If `restartPolicy` is `Always`, init containers use `OnFailure`.
- A Pod is not marked as **Ready** until all init containers succeed.
- If the Pod restarts, **all init containers run again**.

## Example Use Cases
- **Setting Up Configuration Files**: Downloading or generating files that the main application container needs.
- **Checking Dependencies**: Waiting for a database or other service to become available before starting the main container.
- **Initial Data Population**: Seeding a database with initial data before the main container connects.
- Wait for some time before starting the app container with a command
- Clone a Git repository into a Volume

# Simple Example of Kubernetes Init Containers

## Scenario
Imagine you have a web server that depends on a configuration file being downloaded before it can start. We can use an init container to download this file before the main web server container starts.

## Example YAML Configuration

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webserver-pod
spec:
  # Define the init container
  initContainers:
  - name: download-config
    image: busybox
    command:
      - /bin/sh
      - "-c"
      - |
        echo "Downloading config file..."
        # Simulate downloading a config file
        echo "server_config=1234" > /config/server-config.txt
        echo "Config download complete."
        exit 0
    volumeMounts:
      - name: config-volume
        mountPath: /config

  # Define the main container
  containers:
  - name: web-server
    image: nginx
    volumeMounts:
      - name: config-volume
        mountPath: /usr/share/nginx/html/config
    ports:
      - containerPort: 80

  # Define a shared volume
  volumes:
  - name: config-volume
    emptyDir: {}
```

- Apply file: `kubectl apply -f <file_location>`
- Inspect container:  `kubectl exec -it simple-webserver-pod -- /bin/bash`
- Get logs of init container: `kubectl logs simple-webserver-pod -c download-config`