# K8s Container Fault Tolerance

## Handling Container Failures

In Kubernetes, when a container inside a pod fails (like if your application crashes), it doesn't just give up! Kubernetes has built-in mechanisms to handle these failures and tries to get your application running again based on a `restartPolicy` defined in the pod's configuration. Here’s how Kubernetes handles container failures:

1. **Initial Crash**: If a container crashes or fails, Kubernetes will immediately try to restart it based on the `restartPolicy` specified in the pod's configuration.
2. **Repeated Crashes**: If the container keeps failing after the first restart, Kubernetes won’t keep restarting it over and over immediately. Instead, it waits longer and longer between restart attempts using a technique called "exponential backoff." This means it waits 10 seconds after the first failure, then 20 seconds, then 40 seconds, and so on, to avoid overloading the system with rapid restarts.
3. **CrashLoopBackOff**: If the container keeps crashing repeatedly, Kubernetes puts it in a special state called `CrashLoopBackOff`. This status tells you that the container is stuck in a loop of crashes and the system is currently waiting before trying again.
4. **Backoff Reset**: If the container manages to run successfully for a while (let’s say 10 minutes without crashing), Kubernetes resets the backoff timer. If it crashes again after this, Kubernetes will treat it like the first failure and start the restart process fresh.


## Common Causes of `CrashLoopBackOff`

- Application errors or bugs.
- Misconfigurations (environment variables, files, etc.).
- Resource constraints (CPU, memory).
- Failed health checks or probes.

## `restartPolicy` Overview

- **Always** (default): Restarts on any failure.
- **OnFailure**: Restarts only if the container exits with an error. Restarts only if the exit code is non-zero.
- **Never**: Never restarts automatically.

## Key Points

- Kubernetes uses backoff delays for repeated failures, up to 5 minutes.
- Successful runs for 10 minutes reset the backoff.

Kubernetes aims to keep your app running, balancing quick recovery with system stability.


# Testing

To test the restart policy for the nginx Pod, you can modify the Pod configuration to simulate a container failure and observe how Kubernetes handles restarts based on the restartPolicy. Here's how you can do it step-by-step:

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx-pod
  name: nginx-pod
spec:
  containers:
  - image: nginx
    name: nginx-pod
    command: ["/bin/sh", "-c", "exit 1"]  # This command will force the container to fail
    # exit 0 -: The container will start, run successfully, and then exit.
    # exit 1 -: The container will start, encounter a simulated "failure," and exit with an error status code, triggering Kubernetes restart behavior if the
  restartPolicy: Always
```

- Observe: `kubectl get pods -w`

- Apply Changes: `kubectl apply -f nginx-pod.yaml`

- To test with other you can change the `restartPolicy` in the YAML to see how Kubernetes behaves.