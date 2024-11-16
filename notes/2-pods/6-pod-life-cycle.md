# Kubernetes Pod Lifecycle

Pods follow a defined lifecycle, starting in the `Pending` phase, moving through `Running` if at least one of its primary containers starts `OK`, and then through either the `Succeeded` or `Failed` phases depending on whether any container in the Pod terminated in failure.

# Pod Characteristics:

- Pods are ephemeral, meaning they are not designed to be durable or long-lasting.
- Each Pod is assigned a unique UID when created.
- Pods are scheduled to run on specific nodes until they are terminated or deleted.
- Termination is determined by the Pod's restart policy.

# Container States in Kubernetes

### Overview
- In addition to a Pod's overall phase, Kubernetes tracks the state of each **container** within a Pod.
- Once a Pod is assigned to a Node, the **kubelet** creates containers using a container runtime.

### Container States
There are three possible states for containers within a Pod:

| **State**       | **Description**                                                                                                                                   |
|-----------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| **Waiting**     | The container is not yet running or terminated. It is in the process of startup (e.g., pulling images, applying secrets). A **Reason** field indicates why it is waiting. |
| **Running**     | The container is actively executing. Any configured **postStart hook** has already completed. Shows the time when it entered the Running state.      |
| **Terminated**  | The container has finished execution, either successfully or due to failure. Displays the **reason**, **exit code**, and start/finish times.        |

### How to Check Container States
- Use the command:
```bash
  kubectl describe pod <name-of-pod>
```

# Pod Status and Phases

### Pod Status Field
- The status of a Pod is represented by a `PodStatus` object, which includes a **phase** field.
- The **phase** provides a high-level summary of a Pod's lifecycle stage.
- Note: The phase is not a detailed state machine or a comprehensive summary of container status.

### Pod Phases
The possible values for a Pod's phase are:

| **Value**  | **Description**                                                                                                                                             |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Pending**    | The Pod has been accepted by the Kubernetes cluster, but one or more containers are not yet ready. This includes waiting for scheduling or image downloads. |
| **Running**    | The Pod is bound to a node, and all containers have been created. At least one container is running or starting/restarting.                                |
| **Succeeded**  | All containers have terminated successfully and will not be restarted.                                                                                    |
| **Failed**     | All containers have terminated, and at least one failed (non-zero exit status or system termination). Restart is not configured.                          |
| **Unknown**    | The Pod's state is unknown, typically due to communication errors with the node.                                                                         |

### Additional Status Indicators
- **CrashLoopBackOff**: Appears in the Status field when a Pod repeatedly fails to start.
- **Terminating**: Shown when a Pod is in the process of being deleted.

**Note**: Do not confuse `Status` (used in `kubectl` for user intuition) with the Pod's **phase**. The phase is a core part of Kubernetes' data model and API.

