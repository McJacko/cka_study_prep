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


### Use Kubeadm to Install a Basic Cluster

---

#### Introduction to Kubeadm

Kubeadm is a tool that simplifies the process of creating a Kubernetes cluster. It performs the actions necessary to get a minimum viable cluster up and running, such as generating certificates, creating a Kubernetes control plane, and deploying essential services.

---

#### Prerequisites

1. **Supported OS**: Ensure you are using a supported OS like Ubuntu 18.04+, CentOS 7+, or others.
2. **Networking**: Disable swap and ensure the nodes can communicate with each other.
3. **Hardware**: Minimum of 2GB RAM per node and 2 CPUs.
4. **Packages**: Docker, kubeadm, kubelet, and kubectl installed on all nodes.

---

#### Step-by-Step Guide to Install a Kubernetes Cluster Using Kubeadm

1. **Preparing the Nodes**
    - **Update the package index and install dependencies**:
      ```bash
      sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl
      ```
    - **Add the Kubernetes GPG key and repository**:
      ```bash
      sudo curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
      echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
      ```
    - **Install kubelet, kubeadm, and kubectl**:
      ```bash
      sudo apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl
      sudo apt-mark hold kubelet kubeadm kubectl
      ```

2. **Disable Swap**
    ```bash
    sudo swapoff -a
    sudo sed -i '/ swap / s/^/#/' /etc/fstab
    ```

3. **Initialize the Control Plane Node**
    - **Run the kubeadm init command on the control plane node**:
      ```bash
      sudo kubeadm init --pod-network-cidr=192.168.0.0/16
      ```
    - **Set up kubeconfig for the default user**:
      ```bash
      mkdir -p $HOME/.kube
      sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
      sudo chown $(id -u):$(id -g) $HOME/.kube/config
      ```

4. **Deploy a Pod Network (Weave, Calico, Flannel, etc.)**
    - **For example, using Calico**:
      ```bash
      kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
      ```

5. **Join Worker Nodes to the Cluster**
    - **Retrieve the join command from the control plane node**:
      ```bash
      kubeadm token create --print-join-command
      ```
    - **Run the join command on each worker node**:
      ```bash
      sudo kubeadm join <control-plane-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>
      ```

---

#### Practical Exercises

1. **Initialize a Single Control Plane Node Cluster**
    - **Prepare your control plane node**:
      ```bash
      sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl
      sudo curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
      echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
      sudo apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl
      sudo apt-mark hold kubelet kubeadm kubectl
      sudo swapoff -a
      sudo sed -i '/ swap / s/^/#/' /etc/fstab
      ```
    - **Initialize the control plane**:
      ```bash
      sudo kubeadm init --pod-network-cidr=192.168.0.0/16
      mkdir -p $HOME/.kube
      sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
      sudo chown $(id -u):$(id -g) $HOME/.kube/config
      ```
    - **Deploy the Calico network**:
      ```bash
      kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
      ```

2. **Join Worker Nodes to the Control Plane**
    - **On each worker node**:
      ```bash
      sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl
      sudo curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
      echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
      sudo apt-get update
      sudo apt-get install -y kubelet kubeadm kubectl
      sudo apt-mark hold kubelet kubeadm kubectl
      sudo swapoff -a
      sudo sed -i '/ swap / s/^/#/' /etc/fstab
      ```
    - **Run the join command** (from the output of `kubeadm token create --print-join-command` on the control plane node):
      ```bash
      sudo kubeadm join <control-plane-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>
      ```

3. **Verify the Cluster**
    - **Check the nodes in the cluster**:
      ```bash
      kubectl get nodes
      ```

---

#### Tips for the CKA Exam

1. **Understand the Components**: Familiarize yourself with the purpose of `kubeadm`, `kubelet`, and `kubectl`.
2. **Practice Commands**: Run through the entire process multiple times to become comfortable with the steps.
3. **Troubleshoot**: Be prepared to troubleshoot common issues like network plugin failures or node communication issues.
4. **Understand Networking**: Know how to deploy and verify various network plugins (e.g., Calico, Flannel).

---

#### Sample Questions

1. **Question**: How do you initialize a Kubernetes cluster with a specific Pod Network CIDR?
    **Answer**:
    ```bash
    sudo kubeadm init --pod-network-cidr=192.168.0.0/16
    ```

2. **Question**: How do you set up kubeconfig for a non-root user?
    **Answer**:
    ```bash
    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    ```

3. **Question**: How do you join a worker node to a cluster?
    **Answer**:
    ```bash
    sudo kubeadm join <control-plane-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>
    ```

---

#### References

- Kubernetes Documentation on [Kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/)
- Kubernetes [Install and Set Up kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- Networking options: [Calico](https://docs.projectcalico.org/getting-started/kubernetes/), [Flannel](https://github.com/coreos/flannel), [Weave](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/)

---

This guide covers the essential steps and commands required to set up a basic Kubernetes cluster using Kubeadm, which is a critical part of the CKA exam. Make sure to practice each step to gain proficiency and confidence in setting up and managing Kubernetes clusters.



# Manage a highly-available Kubernetes cluster
# Provision underlying infrastructure to deploy a Kubernetes cluster
# Perform a version upgrade on a Kubernetes cluster using Kubeadm
# Implement etcd backup and restore
