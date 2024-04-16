# Understand storage classes, persistent volumes

When studying for the Certified Kubernetes Administrator (CKA) exam, a deep understanding of how Kubernetes handles storage, particularly through StorageClasses and Persistent Volumes (PVs), is crucial. These components are essential for managing stateful applications that require storage resources to persist beyond the lifecycle of individual pods. Here’s a detailed explanation of these concepts:

### Persistent Volumes (PVs)

A Persistent Volume (PV) in Kubernetes is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using StorageClasses. It is a resource in the cluster just like a node is a cluster resource. PVs are volume plugins like Volumes, but have a lifecycle independent of any individual pod that uses the PV.

Here are some key aspects of Persistent Volumes:

- **Provisioning**: PVs can be provisioned statically or dynamically.
  - **Static Provisioning**: The cluster administrator creates a number of PVs. They carry the details of the real storage which is available for use by cluster users.
  - **Dynamic Provisioning**: When none of the static PVs the administrator created match a user's `PersistentVolumeClaim` (PVC), the cluster may try to dynamically create a volume depending on the StorageClass used. This is often managed by a provisioner that is running in the cluster.

- **Lifecycle and States**: PVs have a lifecycle independent of any pod. The lifecycle states include:
  - **Available**: A free resource that has not yet been bound to a PVC.
  - **Bound**: The volume is bound to a PVC.
  - **Released**: The PVC has been deleted, but the resource is not yet reclaimed by the cluster.
  - **Failed**: The volume has failed its automatic reclamation.

- **Reclaim Policy**: This defines what happens to a PV after it is released from its claim. Options include:
  - **Retain**: Manual reclamation.
  - **Recycle**: Basic scrub (e.g., `rm -rf /thevolume/*`) and made available again to new PVCs.
  - **Delete**: The volume (e.g., AWS EBS, Google PD) is deleted.

### Storage Classes

StorageClasses provide a way for administrators to define different "classes" of storage (e.g., fast vs slow storage, encrypted, etc.) that are available within a cluster. Users don't need to be burdened with the details of how the storage is provided or its technological specifics. Instead, they can use claims that abstract the properties, by specifying a StorageClass.

Key features of StorageClasses include:

- **Provisioner**: Defines the volume plugin used for provisioning PVs. This could be internal Kubernetes volume plugins like `kubernetes.io/gce-pd` or it could be an external provisioner like those supplied by storage vendors.
- **Parameters**: StorageClasses allow parameters to be set that will be passed to the volume plugin, such as the type of disk (SSD, HDD), the replication factor, or any other available options depending on the storage backend.
- **Reclaim Policy**: Overrides the default reclaim policy of the PersistentVolume.
- **Allow Volume Expansion**: Indicates whether the storage class allows for expanding volumes.

### PersistentVolumeClaims (PVCs)

PVCs are the request for storage by a user. They are similar to pods in that pods consume node resources, whereas PVCs consume PV resources. Users request volume resources by creating a PVC which specifies size, and access modes among other properties.

### Example Scenario

Here’s a brief flow of how these components interact:
1. An administrator creates a StorageClass aimed at a specific provisioner and sets parameters specific to their environment.
2. A user creates a PVC that specifies the `storageClassName` and requests a certain size.
3. Kubernetes dynamically provisions a PV that matches the requirements of the PVC using the specified StorageClass, if no static PV matches.
4. The user's pod can then use the PVC as a volume.

Understanding how these components interact provides the flexibility and power to manage storage resources in a Kubernetes environment effectively, a crucial skill set for a Kubernetes Administrator.

# Understand volume mode, access modes and reclaim policies for volumes

### Volume Modes

Volume modes in Kubernetes define how a volume is mounted on a host and subsequently made available to a pod. There are two primary volume modes:

1. **Filesystem Mode**: This is the default mode where the volume is mounted on the host as a file system. Pods interact with the volume as if it were a regular directory in their file system.

2. **Block Mode**: In this mode, a volume is attached to the host as a raw block device. It can be consumed by a pod as a raw block device, which is useful for applications that need lower-level access to storage, such as database applications that manage their own storage algorithms and indexing.

The volume mode is specified in the PersistentVolume and PersistentVolumeClaim specifications. Not all storage backends support both modes, so it's important to verify the capabilities of the underlying storage system.

### Access Modes

Access modes in Kubernetes define how a volume can be accessed from a single or multiple nodes:

1. **ReadWriteOnce (RWO)**: The volume can be mounted as read-write by a single node. This is the most common access mode for workloads that require a single point of write access.

2. **ReadOnlyMany (ROX)**: The volume can be mounted as read-only by many nodes. This mode is suitable for scenarios where the data needs to be consumed by multiple pods, such as shared configuration data or public datasets.

3. **ReadWriteMany (RWX)**: The volume can be mounted as read-write by many nodes. This is essential for applications that require concurrent write access from multiple pods, typically seen in distributed applications and file servers.

The support for these access modes can vary depending on the underlying storage provider and the configuration of the environment. It's important to choose the correct access mode that matches the needs of your applications while also being supported by your storage backend.

### Reclaim Policies

Reclaim policies in Kubernetes dictate what happens to a persistent volume when the associated persistent volume claim (PVC) is deleted. This policy is set on the PersistentVolume resource and can have one of the following values:

1. **Retain**: Under the retain policy, when a PVC is deleted, the Persistent Volume remains in the cluster and is moved to the 'Released' state. The data on the volume is intact and the volume is considered "released" but not available for other claims. Manual intervention is required to reclaim the resource or delete the data.

2. **Delete**: This policy automatically deletes the volume from the underlying storage infrastructure when the PVC is deleted. This is useful in dynamic environments and services where the lifecycle of the storage is tightly coupled with the lifecycle of the PVC.

3. **Recycle** (deprecated): This policy was used to scrub the data from the volume and make it available again for a new claim. Due to its limited support and operational complexity, it is generally deprecated in favor of dynamic provisioning.

Understanding these components and how they interact with each other is crucial for efficiently managing storage in Kubernetes. These concepts not only help in ensuring that the applications have the necessary resources but also play a critical role in data integrity and access control in a multi-tenant environment.

# Understand persistent volume claims primitive

In Kubernetes, a **Persistent Volume Claim (PVC)** is a user-level abstraction that requests specific storage resources defined by a Persistent Volume (PV). This abstraction enables users to consume abstract storage resources without needing to know the details about the underlying storage infrastructure. PVCs are crucial for managing storage in Kubernetes, allowing for the dynamic provisioning of storage resources and the decoupling of storage configuration from the usage in pods.

### Fundamental Aspects of PVCs

#### 1. **Definition and Purpose**
   - PVCs allow a user to specify the size of the storage, access modes, and sometimes specific performance characteristics (through StorageClasses) that are required.
   - They are designed to abstract the details of how storage is provided from how it is consumed, thereby facilitating a separation of concerns that is core to the design principles of Kubernetes.

#### 2. **Usage Workflow**
   - **Requesting Storage**: A user creates a PVC that specifies characteristics like storage size and access modes.
   - **Binding**: Kubernetes matches an available PV to a PVC based on the requirements outlined in the PVC. If a suitable PV is not available, and a StorageClass is defined, the cluster may dynamically provision a new PV to meet the PVC requirements.
   - **Using the Volume**: Once bound, the PVC can be used as a volume in a Kubernetes pod. The lifecycle of the PVC is now tied to the lifecycle of the pod unless the PVC is defined to persist beyond the pod's life.

#### 3. **Components of a PVC Specification**
   - **Metadata**: Like all Kubernetes resources, a PVC requires basic metadata like name and namespace.
   - **Spec**: This is where the requirements for storage are defined:
     - **Access Modes**: Such as `ReadWriteOnce`, `ReadOnlyMany`, and `ReadWriteMany`.
     - **Resources**: Requests for storage resources, specifically the amount of space required.
     - **StorageClassName**: Optionally, the name of a StorageClass that should be used to dynamically provision storage.
   - **Status**: Shows the current status of the PVC, such as `Bound`, `Pending`, or `Available`. The `Bound` status includes a reference to the Persistent Volume that satisfies the claim.

### Example PVC Definition
Here’s a simple example of a PVC YAML file in Kubernetes:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
  namespace: default
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: standard
```
In this example:
- The PVC requests a storage volume of 1 gigabyte that can be mounted with read-write permissions by one node at a time.
- It specifies the use of the `standard` StorageClass, which might correspond to a default class provided by the cluster configuration that handles dynamic provisioning.

### Key Considerations for Security Engineers
As a security engineer, when dealing with PVCs, several considerations should be kept in mind:
- **Access Controls**: Ensure appropriate RBAC (Role-Based Access Control) policies are in place to control who can create, update, and delete PVCs.
- **Data Security**: Consider the security implications of the storage itself, including encryption at rest and in transit. This might be configured at the StorageClass level.
- **Compliance and Auditing**: Be aware of the compliance requirements related to data storage and ensure that logs and monitoring are in place for access to and operations on PVCs.

Understanding and effectively managing PVCs are essential for ensuring that applications have the necessary storage resources they require, while also maintaining the security and compliance posture critical in modern cloud-native environments.

# Know how to configure applications with persistent storage
