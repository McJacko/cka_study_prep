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


# Understand persistent volume claims primitive


# Know how to configure applications with persistent storage
