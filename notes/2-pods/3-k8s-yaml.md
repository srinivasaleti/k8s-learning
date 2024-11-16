# Notes: Creating a Kubernetes Pod Using YAML

## Overview
In this lecture, we are learning how to create a Kubernetes Pod using a YAML-based configuration file. Kubernetes utilizes YAML files to create various objects like pods, replicas, deployments, and services. All Kubernetes definition files have a similar structure and must contain four key top-level fields:

1. **API Version**
2. **Kind**
3. **Metadata**
4. **Spec**

These fields are mandatory for creating Kubernetes objects.

## Key Fields in a Kubernetes YAML File

### Basic structure

```yaml
apiVersion:
kind: 
metadata:
  ...
spec:
  ...
```

### 1. API Version
- This field specifies the version of the Kubernetes API that will be used to create the object.
- Different API versions are available depending on the type of object being created.
- Example values for API version:
  - `v1` (used for basic resources like Pods)
  - `apps/v1beta`
  - `extensions/v1beta`

For this example, since we're working with Pods, we will use `v1`.

### 2. Kind
- This field defines the type of object to be created.
- In this example, we are creating a Pod, so the kind is set to `Pod`.
- Other possible values:
  - `ReplicaSet`
  - `Deployment`
  - `Service`

### 3. Metadata
- Metadata contains data about the object such as its name and labels.
- It is structured as a dictionary, so all properties under `metadata` should be indented to the right.
  - `name`: Specifies the name of the object, e.g., `my-app-pod`.
  - `labels`: A set of key-value pairs to categorize the object (e.g., `app: my-app`).

Labels are useful for identifying and filtering objects. For example, you can label Pods as `frontend`, `backend`, or `database` for easier management.

### 4. Spec
- The `spec` field contains the configuration details of the object being created.
- This field is unique to the type of object and may vary depending on its properties.
- In this example, `spec` is used to define the container within the Pod.

## Creating a Pod Configuration

Below is an example of a YAML configuration file to create a Pod with an Nginx container:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx-container
      image: nginx
```