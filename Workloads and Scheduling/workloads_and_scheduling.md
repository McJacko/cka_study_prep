# Understand deployments and how to perform rolling update and rollbacks

```markdown
# CKA Exam Study Guide: Understanding Deployments and How to Perform Rolling Updates and Rollbacks

## Introduction

Kubernetes Deployments are used to manage the deployment of applications in a Kubernetes cluster. They ensure that a specified number of instances of an application are running and provide mechanisms for updating and rolling back application versions.

### Key Concepts
- **Deployment**: A resource that provides declarative updates to applications.
- **ReplicaSet**: Maintains a stable set of replica Pods running at any given time.
- **Pod**: The smallest deployable unit that can be created and managed in Kubernetes.
- **Rolling Update**: Gradually replaces old versions of Pods with new versions.
- **Rollback**: Reverts the application to a previous version in case of issues with the current version.

## Creating a Deployment

To create a Deployment, you define it in a YAML file and apply it using `kubectl`. Here's an example YAML for a simple Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

To create this Deployment, use the following command:

```sh
kubectl apply -f deployment.yaml
```

## Checking the Status of a Deployment

After creating a Deployment, you can check its status to ensure it is running correctly.

```sh
kubectl get deployments
kubectl describe deployment nginx-deployment
kubectl get pods -l app=nginx
```

## Performing a Rolling Update

A rolling update allows you to update the Pods in a Deployment to a new version without downtime. To perform a rolling update, update the image version in the Deployment YAML file and apply the change:

```yaml
spec:
  containers:
  - name: nginx
    image: nginx:1.16.0  # Update the image version
```

Apply the changes:

```sh
kubectl apply -f deployment.yaml
```

Kubernetes will then perform a rolling update, gradually replacing old Pods with new ones.

To check the progress of the rolling update:

```sh
kubectl rollout status deployment/nginx-deployment
```

## Rolling Back a Deployment

If there is an issue with the new version of your application, you can roll back to the previous version using the following command:

```sh
kubectl rollout undo deployment/nginx-deployment
```

You can also roll back to a specific revision if needed:

```sh
kubectl rollout undo deployment/nginx-deployment --to-revision=2
```

## Viewing Deployment History

To see the history of revisions for a Deployment, use:

```sh
kubectl rollout history deployment/nginx-deployment
```

To get detailed information about a specific revision:

```sh
kubectl rollout history deployment/nginx-deployment --revision=2
```

## Scaling a Deployment

Scaling a Deployment changes the number of replica Pods. To scale a Deployment to a specified number of replicas:

```sh
kubectl scale deployment nginx-deployment --replicas=5
```

## Deleting a Deployment

To delete a Deployment and all associated resources:

```sh
kubectl delete deployment nginx-deployment
```

## Summary

Understanding and managing Kubernetes Deployments is crucial for maintaining application availability and facilitating smooth updates. Key commands and concepts to remember include creating and applying Deployment YAML files, checking Deployment status, performing rolling updates, rolling back changes, viewing Deployment history, scaling Deployments, and deleting Deployments.

### Essential `kubectl` Commands
- `kubectl apply -f deployment.yaml`
- `kubectl get deployments`
- `kubectl describe deployment <deployment-name>`
- `kubectl get pods -l <label>`
- `kubectl rollout status deployment/<deployment-name>`
- `kubectl rollout undo deployment/<deployment-name>`
- `kubectl rollout history deployment/<deployment-name>`
- `kubectl scale deployment <deployment-name> --replicas=<number>`
- `kubectl delete deployment <deployment-name>`

This guide covers the basics of managing Deployments in Kubernetes, which is an important part of the Certified Kubernetes Administrator (CKA) exam. Ensure you practice these commands and understand their implications to confidently manage application deployments in a Kubernetes environment.
```




# Use ConfigMaps and Secrets to configure applications
# Know how to scale applications
# Understand the primitives used to create robust, self-healing, application deployments
# Understand how resource limits can affect Pod scheduling
# Awareness of manifest management and common templating tools
