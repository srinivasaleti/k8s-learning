## Purpose
- Ensures a **stable set of replica Pods** are running at all times.
- Commonly managed automatically by a **Deployment**.

## Key Features
1. **Maintains Availability**:
   - Guarantees a specified number of **identical Pods** are always running.
   
2. **Pod Management**:
   - Creates or deletes Pods as needed to maintain the desired number of replicas.

3. **Pod Template**:
   - Defines the specifications for new Pods that need to be created.

## How ReplicaSet Works
- **Selector**: 
  - Identifies Pods it can manage using label matching.
  
- **Replica Count**:
  - Specifies the desired number of Pods to maintain.

- **Pod Creation**:
  - Uses the **Pod template** to create new Pods when required.

- **OwnerReferences**:
  - Links Pods to the ReplicaSet via the `metadata.ownerReferences` field.
  - Allows the ReplicaSet to track and manage its Pods.

- **Acquiring Pods**:
  - If a Pod:
    - Matches the ReplicaSet's selector.
    - Has no `OwnerReference` or a non-Controller `OwnerReference`.
  - The ReplicaSet automatically acquires and manages it.

## When to Use a ReplicaSet
- Use a **ReplicaSet** when:
  - Custom update orchestration is required.
  - Updates are not needed at all.

- **Recommendation**:
  - Prefer using **Deployments** over directly managing ReplicaSets, as Deployments provide:
    - Declarative updates to Pods.
    - Additional features for application management.
