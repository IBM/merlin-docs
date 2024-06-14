# Data Storage

IBM i Modernization Engine For Lifecycle Integration requires storage, which must be provisioned before IBM i Modernization Engine For Lifecycle Integration is installed.

Red Hat® OpenShift® Container Platform uses the Kubernetes persistent volume (PV) framework. PVs are storage resources in the cluster, and persistent volume claims (PVCs) are storage requests that are made on those PVs by Merlin. For more information about persistent storage in OpenShift clusters, see [Understanding persistent storage](https://docs.openshift.com/container-platform/4.14/storage/understanding-persistent-storage.html) in the Red Hat OpenShift documentation.

## Recommended storage providers

The persistent storage that you choose must support the RWO (read-write-once) access mode and support the block and file storage types. This storage is used for PostgreSQL, the engine and the gui pod.  
IBM i Developer and IBM i CI/CD can use the same storage providers as IBM i Modernization Engine for Lifecycle Integration platform.

Recommended storage providers might include:  

* IBM Storage Suite for IBM Cloud Paks
  * Red Hat OpenShift Data Foundation 4.x (formerly Red Hat OpenShift Container Storage), from version 4.2 or higher
  * IBM Cloud Block storage and IBM Cloud File storage
  * File storage from IBM Spectrum Fusion/Scale (ideal for RWX)
  * Block storage from IBM Spectrum Virtualize, FlashSystem, or DS8K

* Portworx Storage, version 2.5.5 or above
* Amazon Elastic File Storage (EFS) for RWX mode access

If object storage is used, the following storage provider must be validated across all relevant capabilities:

* Object storage from IBM Cloud Object Storage or Red Hat Ceph
* Amazon Cloud Object Storage (S3)

Note: To learn more about configuring storage, see the Red Hat Documentation.

## Configure a default storage class
Before install IBM i Modernization Engine For Lifecycle Integration, a default storage class should be configured by setting storageclass.kubernetes.io/is-default-class: 'true' as following:

```
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: nfs-storage-provisioner
  annotations:
    storageclass.kubernetes.io/is-default-class: 'true'
provisioner: nfs-storage
parameters:
  archiveOnDelete: 'false'
reclaimPolicy: Delete
volumeBindingMode: Immediate
```

## Persistent storage sizing

Persistent storage requirements depend on the size of the stored data sets that your IBM i Developer Tool deployment processes. The minimum PVC size is 20G for IBM i Developer Tool.

The access mode is shown for each of the PVs, and is ReadWriteOnce (RWO). For more information about access modes, see Access modes in the Red Hat OpenShift documentation.

The storage type is shown for each of the PVs, and is either Filesystem or Block. For more information, see Block volume support in the Red Hat OpenShift documentation.

IBM i Modernization Engine For Lifecycle Integration persists technical data that is related to configuration and management of the service in any of the follow

### IBM i Modernization Engine for Lifecycle Integration persistent volumes
The default names of the three IBM i Modernization Engine for Lifecycle Integration persistent volumes and their usages are as follows:

| PVC name              | Size          | Access Mode | Description                                      |
|-----------------------|---------------|----------------|----------------------------------|
|merlin-engine-backend 	|2 GB |	ReadWriteOnce | Engine volume for engine logs|
|merlin-gui-file-backend|	1 GB |	ReadWriteOnce |GUI volume for configuration files|
|merlin-postgresql-claim |	5 GB  |  ReadWriteOnce| A volume for database|

### IBM i CI/CD Tool persistent volume
The default name of the IBM i CI/CD Tool persistent volume and its usages are as follows:

| PVC name              | Size          | Access Mode | Description                                      |
|-----------------------|---------------|--------------|------------------------------------|
|merlin-cicd-backend |	5 GB  |  ReadWriteOnce| A volume for cicd logs|

### IBM i Developer Tool persistent volume
The default name of the IBM i Developer Tool persistent volume and its usages are as follows:

| PVC name              | Size          | Access Mode | Description                                      |
|-----------------------|---------------|--------------|------------------------------------|
|postgres-data |	1 GB  |  ReadWriteOnce| A volume for database|

