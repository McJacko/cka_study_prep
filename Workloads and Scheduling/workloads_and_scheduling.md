# Understand deployments and how to perform rolling update and rollbacks

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

# Use ConfigMaps and Secrets to configure applications

# CKA Exam Study Guide: Use ConfigMaps and Secrets to Configure Applications

## Overview

ConfigMaps and Secrets are essential components in Kubernetes used to decouple configuration artifacts from image content to keep containerized applications portable. This guide covers how to create, use, and manage ConfigMaps and Secrets to configure applications, with relevant `kubectl` commands.

---

## ConfigMaps

### Definition
A ConfigMap is an API object used to store non-confidential data in key-value pairs. ConfigMaps allow you to separate your configurations from your Pods and components, thus enabling better portability and configuration management.

### Creating ConfigMaps

1. **From a literal value:**
   ```sh
   kubectl create configmap <configmap-name> --from-literal=<key>=<value>
   ```

2. **From a file:**
   ```sh
   kubectl create configmap <configmap-name> --from-file=<path-to-file>
   ```

3. **From an environment file:**
   ```sh
   kubectl create configmap <configmap-name> --from-env-file=<path-to-env-file>
   ```

4. **From a directory:**
   ```sh
   kubectl create configmap <configmap-name> --from-file=<directory-path>
   ```

### Using ConfigMaps

1. **As environment variables in a Pod:**
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: <pod-name>
   spec:
     containers:
     - name: <container-name>
       image: <image-name>
       env:
       - name: <ENV_VAR_NAME>
         valueFrom:
           configMapKeyRef:
             name: <configmap-name>
             key: <key>
   ```

2. **As configuration files mounted in volumes:**
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: <pod-name>
   spec:
     containers:
     - name: <container-name>
       image: <image-name>
       volumeMounts:
       - name: <volume-name>
         mountPath: <path-in-container>
     volumes:
     - name: <volume-name>
       configMap:
         name: <configmap-name>
   ```

### Updating ConfigMaps

1. **Editing a ConfigMap:**
   ```sh
   kubectl edit configmap <configmap-name>
   ```

2. **Replace an existing ConfigMap:**
   ```sh
   kubectl replace -f <configmap-definition-file.yaml>
   ```

---

## Secrets

### Definition
A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a Pod specification or in an image, which is less secure.

### Creating Secrets

1. **From literal values:**
   ```sh
   kubectl create secret generic <secret-name> --from-literal=<key>=<value>
   ```

2. **From a file:**
   ```sh
   kubectl create secret generic <secret-name> --from-file=<path-to-file>
   ```

3. **From an environment file:**
   ```sh
   kubectl create secret generic <secret-name> --from-env-file=<path-to-env-file>
   ```

### Using Secrets

1. **As environment variables in a Pod:**
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: <pod-name>
   spec:
     containers:
     - name: <container-name>
       image: <image-name>
       env:
       - name: <ENV_VAR_NAME>
         valueFrom:
           secretKeyRef:
             name: <secret-name>
             key: <key>
   ```

2. **As files mounted in volumes:**
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: <pod-name>
   spec:
     containers:
     - name: <container-name>
       image: <image-name>
       volumeMounts:
       - name: <volume-name>
         mountPath: <path-in-container>
     volumes:
     - name: <volume-name>
       secret:
         secretName: <secret-name>
   ```

### Updating Secrets

1. **Editing a Secret:**
   ```sh
   kubectl edit secret <secret-name>
   ```

2. **Replace an existing Secret:**
   ```sh
   kubectl replace -f <secret-definition-file.yaml>
   ```

---

## Practical Examples

### Example 1: Configuring an Application with ConfigMaps

1. **Create a ConfigMap from a literal value:**
   ```sh
   kubectl create configmap my-config --from-literal=APP_COLOR=blue
   ```

2. **Use the ConfigMap in a Pod as an environment variable:**
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
     - name: mycontainer
       image: nginx
       env:
       - name: APP_COLOR
         valueFrom:
           configMapKeyRef:
             name: my-config
             key: APP_COLOR
   ```

3. **Deploy the Pod:**
   ```sh
   kubectl apply -f pod-configmap.yaml
   ```

### Example 2: Configuring an Application with Secrets

1. **Create a Secret from a literal value:**
   ```sh
   kubectl create secret generic my-secret --from-literal=username=admin --from-literal=password=s3cr3t
   ```

2. **Use the Secret in a Pod as an environment variable:**
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
     - name: mycontainer
       image: nginx
       env:
       - name: USERNAME
         valueFrom:
           secretKeyRef:
             name: my-secret
             key: username
       - name: PASSWORD
         valueFrom:
           secretKeyRef:
             name: my-secret
             key: password
   ```

3. **Deploy the Pod:**
   ```sh
   kubectl apply -f pod-secret.yaml
   ```

---

## Best Practices

1. **Use ConfigMaps for non-sensitive data:** ConfigMaps are suitable for storing configuration data such as environment settings, application parameters, and other non-sensitive data.

2. **Use Secrets for sensitive data:** Secrets should be used for storing sensitive information such as passwords, tokens, and keys.

3. **Limit access to Secrets:** Ensure that only the necessary Pods and components have access to the Secrets.

4. **Regularly rotate Secrets:** Regularly update and rotate your Secrets to minimize the risk of exposure.

5. **Use Kubernetes RBAC:** Implement Role-Based Access Control (RBAC) to manage access to ConfigMaps and Secrets.

6. **Avoid hardcoding sensitive data:** Avoid including sensitive information directly in Pod specifications or application code.

---

By understanding and implementing ConfigMaps and Secrets effectively, you can enhance the security, manageability, and scalability of your Kubernetes applications.


# Know how to scale applications
# Understand the primitives used to create robust, self-healing, application deployments
# Understand how resource limits can affect Pod scheduling
# Awareness of manifest management and common templating tools
