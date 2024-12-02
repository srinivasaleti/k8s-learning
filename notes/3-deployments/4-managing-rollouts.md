# Managing Deployment Rollouts in Kubernetes

Kubernetes provides tools to manage and track the rollout of changes to Deployments. This includes viewing the rollout history, and reverting to specific versions if needed.

---

## 1. Create deployment

To Create deployment run:

```
kubectl apply -f practice/deployment/nginx-deployment.yaml
```

## 2. Viewing Rollout History

The `kubectl rollout history` command displays all revisions of a Deployment, including changes made to the Pod template (e.g., image updates or environment changes).

### Command
```bash
kubectl rollout history deployment/<deployment_name>
```

Ex: `kubectl rollout history deployment/nginx-deployment`

Output:
```
REVISION  CHANGE-CAUSE
1         Initial deployment
2         kubectl set image deployment/nginx-deployment nginx=nginx:1.16.0
3         kubectl set image deployment/nginx-deployment nginx=nginx:1.17.0
```

## 2. Viewing Detailed Information for a Specific Revision
You can view detailed information about a specific revision to understand what changes were made.

### Command
```
kubectl rollout history deployment/<deployment_name> --revision=<revision_number>
```

Ex: `kubectl rollout history deployment/nginx-deployment --revision=2`


## 3. Update deployment

To update image run below command:

```
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.0
```

To verify: 
- Get running pods: `kubectl get pods`
- Get details of a pod by running: `kubectl describe pod <pod_name>`

## 4. Rolling Back to a Specific Revision

If an update introduces issues, you can roll back the Deployment to a previous version using the kubectl rollout undo command.

### Command to Roll Back

```
kubectl rollout undo deployment/<deployment_name> --to-revision=<revision_number>
```

Ex: `kubectl rollout undo deployment/nginx-deployment --to-revision=2`


**What Happens:**
- Kubernetes reverts the Deployment to the specified revision.
- A new revision is created to represent the rollback.

## 5. Checking Rollout Status

To monitor the status of a rollout (e.g., during an update or rollback), use:

```
kubectl rollout status deployment/<deployment_name>
```

# Summary of Rollout Commands in Kubernetes

| **Action**                              | **Command**                                             |
|-----------------------------------------|---------------------------------------------------------|
| View rollout history                    | `kubectl rollout history deployment/<deployment_name>` |
| View details of a specific revision     | `kubectl rollout history deployment/<deployment_name> --revision=<revision_number>` |
| Roll back to a specific revision        | `kubectl rollout undo deployment/<deployment_name> --to-revision=<revision_number>` |
| Monitor rollout status                  | `kubectl rollout status deployment/<deployment_name>`  |
