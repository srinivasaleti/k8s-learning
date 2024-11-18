# Kubernetes Pod Failure Handling

## Container Failures within a Pod
- If one container in a Pod fails, Kubernetes may restart **just that container** depend on `restartPolicy`.

## Pod Failures and Cluster Recovery
- Sometimes, a Pod can fail in a way that the cluster **cannot fix**.
  - In such cases, Kubernetes **deletes the Pod** instead of trying to heal it.
  - It relies on other components (like controllers) for **automatic recovery**.

## Node Failures
- If a Pod’s node fails, the Pod is marked as **unhealthy**.
  - Kubernetes will **delete the Pod** after identifying the failure.
- A Pod will also be deleted if it's **evicted** due to:
  - Lack of resources (like CPU or memory).
  - Node maintenance tasks.

## Pod Rescheduling and Replacement
- A specific Pod (with a unique identifier called **UID**) is **never rescheduled** to a different node.
  - Instead, a new Pod is created to replace the old one.
  - This replacement Pod can have the same **name** but will have a **different UID**.

## Node Assignment for Replacement Pods
- Kubernetes **does not guarantee** that a replacement Pod will be scheduled on the same node as the original.

## Associated Lifetimes
- Some resources have the **same lifetime** as a Pod, like **volumes**.
  - These resources exist **only as long as the specific Pod** (with that exact UID) exists.
  - If a Pod is deleted, those associated resources are also destroyed.
  - Even if an identical replacement Pod is created, the related resources (like volumes) are **created anew**.