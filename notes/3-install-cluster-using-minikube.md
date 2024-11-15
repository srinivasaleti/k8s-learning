# Setting Up a Kubernetes Cluster on macOS Using Minikube

## Prerequisites
1. **macOS**: Ensure your macOS is up-to-date.
2. **Command Line Tools**: Make sure you have the Xcode Command Line Tools installed:
   ```bash
   xcode-select --install
   ```
3. **Virtualization**: Install a virtualization solution if needed (like Docker, VirtualBox, or HyperKit).

## Installation Steps

### 1. Install Minikube
Link: https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Fx86-64%2Fstable%2Fbinary+download

Download and install Minikube directly using `brew`:
```bash
brew install minikube
```

### 2. Start Minikube
Once the virtualization driver is set up, start Minikube:
```bash
minikube start
```
This command initializes a Kubernetes cluster using the specified driver. If you are using Docker as a driver, it will default to that.

To specify a driver, you can use:
```bash
minikube start --driver=docker
```

### 2. Install kubectl
- Kubectl is a command line tool to interact with K8s cluster & nodes
- Install kubectl either from official website or minikube can download the appropriate version of kubectl and you should be able to use it like this:
```
minikube kubectl -- get po -A
```

- Set `kubectl` alias

```
alias kubectl="minikube kubectl --"
```

- Verify the installation:
```bash
kubectl version --client
```

### 4. Verify Minikube Installation
Check the status of your Minikube cluster:
```bash
minikube status
```

Verify that `kubectl` can connect to the Minikube cluster:
```bash
kubectl get nodes
```

### 6. Basic Minikube Commands
- **Open Dashboard**: Launch a Kubernetes dashboard in the browser:
  ```bash
  minikube dashboard
  ```
- **Stop Minikube**: To stop the Minikube cluster:
  ```bash
  minikube stop
  ```
- **Delete Minikube Cluster**: If you want to delete the Minikube cluster:
  ```bash
  minikube delete
  ```

### 7. Deploying a Sample Application
To deploy a simple app in your Minikube cluster, follow these steps:

#### Step 1: Create a Deployment
Deploy an example `nginx` web server:
```bash
kubectl create deployment nginx --image=nginx
```

#### Step 2: Expose the Deployment
Expose the `nginx` Deployment as a service:
```bash
kubectl expose deployment nginx --type=NodePort --port=80
```

#### Step 3: Access the Application
Find the URL to access your application:
```bash
minikube service nginx --url
```
Open the URL provided by the command in your web browser.

## Troubleshooting
- **Issue with Virtualization**: Ensure your virtualization driver is running (Docker, VirtualBox, HyperKit, etc.).
- **Permissions**: If you encounter permission issues, try running commands with `sudo`.
- **Cluster Status**: Use `minikube logs` to check for any errors if the cluster doesn’t start correctly.

## Useful Minikube Commands
- **Check Cluster Information**:
  ```bash
  kubectl cluster-info
  ```
- **View Pods**:
  ```bash
  kubectl get pods
  ```
- **View Services**:
  ```bash
  kubectl get services
  ```
- **SSH into Minikube VM**:
  ```bash
  minikube ssh
  ```
- **Enable Add-ons** (like Ingress):
  ```bash
  minikube addons enable ingress
  ```

## Additional Resources
- **Minikube Documentation**: [Minikube Official Docs](https://minikube.sigs.k8s.io/docs/)
- **Kubernetes Documentation**: [Kubernetes Official Docs](https://kubernetes.io/docs/)
