### Manage Role-Based Access Control (RBAC)

---

#### Introduction to Role-Based Access Control (RBAC)

RBAC is a method used by Kubernetes to regulate access to the cluster. It allows administrators to define roles and assign them to users, thereby controlling what actions those users can perform within the cluster. The primary objects involved in RBAC are Roles, ClusterRoles, RoleBindings, and ClusterRoleBindings.

---

#### Key Concepts

1. **Role**: Defines a set of permissions within a namespace.
2. **ClusterRole**: Defines a set of permissions cluster-wide or within a specific namespace.
3. **RoleBinding**: Grants the permissions defined in a Role to a user or a group within a namespace.
4. **ClusterRoleBinding**: Grants the permissions defined in a ClusterRole to a user or a group cluster-wide or within a specific namespace.

---

#### Common RBAC Operations

1. **Creating a Role**
    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: Role
    metadata:
      namespace: <namespace>
      name: <role-name>
    rules:
    - apiGroups: [""]
      resources: ["pods"]
      verbs: ["get", "watch", "list"]
    ```

2. **Creating a ClusterRole**
    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRole
    metadata:
      name: <clusterrole-name>
    rules:
    - apiGroups: [""]
      resources: ["nodes"]
      verbs: ["get", "list", "watch"]
    ```

3. **Creating a RoleBinding**
    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: RoleBinding
    metadata:
      name: <rolebinding-name>
      namespace: <namespace>
    subjects:
    - kind: User
      name: <username>
      apiGroup: rbac.authorization.k8s.io
    roleRef:
      kind: Role
      name: <role-name>
      apiGroup: rbac.authorization.k8s.io
    ```

4. **Creating a ClusterRoleBinding**
    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
      name: <clusterrolebinding-name>
    subjects:
    - kind: User
      name: <username>
      apiGroup: rbac.authorization.k8s.io
    roleRef:
      kind: ClusterRole
      name: <clusterrole-name>
      apiGroup: rbac.authorization.k8s.io
    ```

---

#### Practical Exercises

1. **Create a Role and RoleBinding to Allow Reading Pods in a Namespace**
    - Create a YAML file for the Role:
      ```yaml
      apiVersion: rbac.authorization.k8s.io/v1
      kind: Role
      metadata:
        namespace: default
        name: pod-reader
      rules:
      - apiGroups: [""]
        resources: ["pods"]
        verbs: ["get", "list"]
      ```
    - Create a YAML file for the RoleBinding:
      ```yaml
      apiVersion: rbac.authorization.k8s.io/v1
      kind: RoleBinding
      metadata:
        name: read-pods
        namespace: default
      subjects:
      - kind: User
        name: jane
        apiGroup: rbac.authorization.k8s.io
      roleRef:
        kind: Role
        name: pod-reader
        apiGroup: rbac.authorization.k8s.io
      ```
    - Apply the YAML files:
      ```bash
      kubectl apply -f role.yaml
      kubectl apply -f rolebinding.yaml
      ```

2. **Create a ClusterRole and ClusterRoleBinding to Allow Viewing Nodes Across the Cluster**
    - Create a YAML file for the ClusterRole:
      ```yaml
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRole
      metadata:
        name: node-viewer
      rules:
      - apiGroups: [""]
        resources: ["nodes"]
        verbs: ["get", "list"]
      ```
    - Create a YAML file for the ClusterRoleBinding:
      ```yaml
      apiVersion: rbac.authorization.k8s.io/v1
      kind: ClusterRoleBinding
      metadata:
        name: view-nodes
      subjects:
      - kind: User
        name: john
        apiGroup: rbac.authorization.k8s.io
      roleRef:
        kind: ClusterRole
        name: node-viewer
        apiGroup: rbac.authorization.k8s.io
      ```
    - Apply the YAML files:
      ```bash
      kubectl apply -f clusterrole.yaml
      kubectl apply -f clusterrolebinding.yaml
      ```

3. **Check User Permissions**
    - Verify the permissions of a user:
      ```bash
      kubectl auth can-i get pods --as=jane --namespace=default
      kubectl auth can-i list nodes --as=john
      ```

---

#### Tips for the CKA Exam

1. **Understand the Scope**: Differentiate between Roles and ClusterRoles in terms of their scope (namespace vs. cluster-wide).
2. **Practice Commands**: Get comfortable with creating and managing RBAC resources using `kubectl`.
3. **Use Dry-Run**: Use the `--dry-run` option to validate your configurations before applying them.
4. **Troubleshoot Permissions**: Use `kubectl auth can-i` to troubleshoot and verify user permissions.

---

#### Sample Questions

1. **Question**: How do you create a Role that allows listing and watching ConfigMaps in the `dev` namespace?
    **Answer**:
    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: Role
    metadata:
      namespace: dev
      name: configmap-watcher
    rules:
    - apiGroups: [""]
      resources: ["configmaps"]
      verbs: ["list", "watch"]
    ```

2. **Question**: How do you bind a user `alice` to a ClusterRole that allows managing secrets across all namespaces?
    **Answer**:
    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
      name: manage-secrets
    subjects:
    - kind: User
      name: alice
      apiGroup: rbac.authorization.k8s.io
    roleRef:
      kind: ClusterRole
      name: secret-manager
      apiGroup: rbac.authorization.k8s.io
    ```

3. **Question**: How do you check if a user `bob` can delete services in the `staging` namespace?
    **Answer**:
    ```bash
    kubectl auth can-i delete services --as=bob --namespace=staging
    ```

---

#### References

- Kubernetes Documentation on [RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
- Kubernetes [Role-Based Access Control](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) Best Practices
- Kubernetes [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

---

This guide provides a comprehensive overview of managing RBAC in Kubernetes, crucial for the CKA exam. Practice creating and managing these resources to become proficient in handling RBAC scenarios in a real-world Kubernetes environment.



# Use Kubeadm to install a basic cluster
# Manage a highly-available Kubernetes cluster
# Provision underlying infrastructure to deploy a Kubernetes cluster
# Perform a version upgrade on a Kubernetes cluster using Kubeadm
# Implement etcd backup and restore
