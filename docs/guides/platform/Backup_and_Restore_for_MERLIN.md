# Backup and restore IBM i Modernization Engine For Lifecycle Integration 

# Overview

The document introduces the detailed steps for backup and restore IBM i Modernization Engine For Lifecycle Integration with minio, OADP and a customer plugin ‘cpdbr-velero-plugin’.

## Set Minio bucket storage service in OpenShift environment 

Before configure the OADP service, a bucket storage must be configured to work as backup location. 
Note: the following command need to run to the bastion server.

### Download minio and install

```
wget https://github.com/vmware-tanzu/velero/releases/download/v1.7.1/velero-v1.7.1-linux-ppc64le.tar.gz

tar zxvf velero-v1.7.1-linux-ppc64le.tar.gz

cd velero-v1.7.1-linux-ppc64le/examples/minio

oc apply -f 00-minio-deployment.yaml
```

### Initialize PVC for minio

```
vim minio-config-pvc.yml

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: minio-config-pvc
  namespace: velero
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  storageClassName: nfs-storage-provisioner
  resources:
    requests:
      storage: 1Gi

oc apply -f minio-config-pvc.yml

vim minio-storage-pvc.yaml

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: minio-storage-pvc
  namespace: velero
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  storageClassName: nfs-storage-provisioner
  resources:
    requests:
      storage: 100Gi

oc apply -f minio-storage-pvc.yaml

oc set volume deployment.apps/minio --add --overwrite --name=config --mount-path=/config --type=persistentVolumeClaim --claim-name="minio-config-pvc" -n velero

oc set volume deployment.apps/minio --add --overwrite --name=storage --mount-path=/storage --type=persistentVolumeClaim --claim-name="minio-storage-pvc" -n velero

oc set resources deployment minio -n velero --requests=cpu=500m,memory=256Mi --limits=cpu=1,memory=1Gi

oc get pods -n velero
```

### Expose the minio and get the route
```
oc expose svc minio -n velero
route.route.openshift.io/minio exposed

oc get route minio -n velero
NAME    HOST/PORT                                     PATH   SERVICES   PORT   TERMINATION   WILDCARD
minio   minio-velero.apps.amp-fb35.9.5.36.56.nip.io          minio      9000                 None
```

### Configure the mc command
```
Note: Reference to https://docs.min.io/docs/minio-client-quickstart-guide.html

Wget https://dl.min.io/client/mc/release/linux-ppc64le/mc

Chomd +x mc

./mc --help
```

### Use the mc command to create the bucket in minio

```
./mc alias set minio http:// minio-velero.apps.amp-fb35.9.5.36.56.nip.io minio minio123
Note: minio and minio123 is the default user and password, which should be find /velero-v1.7.1-linux-ppc64le/examples/minio/ 00-minio-deployment.yaml
        env:
        - name: MINIO_ACCESS_KEY
          value: "minio"
        - name: MINIO_SECRET_KEY
          value: "minio123"

./mc mb -p minio/velero
Bucket created successfully `minio/velero`.

./mc ls minio
```

## Install OADP operator

### Prepare customer plugin
```
podman pull docker.io/ibmcom/cpdbr-velero-plugin:4.0.0-beta1-1-ppc64le

podman tag docker.io/ibmcom/cpdbr-velero-plugin:4.0.0-beta1-1-ppc64le the-registry/cpdbr-velero-plugin:4.0.0-beta1-1-ppc64le

podman push the-registry/cpdbr-velero-plugin:4.0.0-beta1-1-ppc64le
```
### Prepare OADP
Search OADP or Velero in openshift console and install it.

Note, The version of OADP must be higher than 0.5.6 and install it. Reference to: https://github.com/openshift/oadp-operator/blob/apiv1/docs/install_olm.md#create-the-dataprotectionapplication-custom-resource
```
oc annotate namespace oadp-operator openshift.io/node-selector=""

vim credentials-velero

[default]
aws_access_key_id=minio
aws_secret_access_key=minio123

oc create secret generic cloud-credentials --namespace oadp-operator --from-file cloud=./credentials-velero
```

### Create data protection instance
In the openshift console, open the  OADP operator. Find the DataProtectionApplication, and click create instance. Edit the yml file like the following script.
``` 
apiVersion: oadp.openshift.io/v1alpha1
kind: DataProtectionApplication
metadata:
  name: velero-sample
  namespace: oadp-operator
spec:
  backupLocations:
    - velero:
        config:
          region: minio
          s3ForcePathStyle: 'true'
          s3Url: 'http://minio-velero.apps.amp-fb35.9.5.36.92.nip.io'
        credential:
          key: cloud
          name: cloud-credentials
        default: true
        objectStorage:
          bucket: velero
          prefix: cpdbackup
        provider: aws
  configuration:
    restic:
      enable: true
    velero:
      customPlugins:
        - image: >-
            Your-registry/cpdbr-velero-plugin:4.0.0-beta1-1-ppc64le
          name: cpdbr-velero-plugin
      defaultPlugins:
        - openshift
        - aws
        – csi
And then wait a moment to check all pods status, make sure all of them ready.
oc get all -n oadp-operator
NAME                                                     READY   STATUS    RESTARTS   AGE
pod/oadp-velero-sample-1-aws-registry-678ffd4bd9-fqng6   1/1     Running   0          2m1s
pod/openshift-adp-controller-manager-548768c576-pz4g6    1/1     Running   0          25m
pod/restic-6wk8f                                         1/1     Running   0          2m1s
pod/restic-pcx8s                                         1/1     Running   0          2m1s
pod/restic-ph256                                         1/1     Running   0          2m1s
pod/velero-5fd9d9b5dd-gph7f                              1/1     Running   0          2m1s

NAME                                                       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/oadp-velero-sample-1-aws-registry-svc              ClusterIP   172.30.19.147   <none>        5000/TCP   2m1s
service/openshift-adp-controller-manager-metrics-service   ClusterIP   172.30.55.104   <none>        8443/TCP   25m
service/openshift-adp-velero-metrics-svc                   ClusterIP   172.30.67.84    <none>        8085/TCP   2m1s

NAME                    DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
daemonset.apps/restic   3         3         3       3            3           <none>          2m1s

NAME                                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/oadp-velero-sample-1-aws-registry   1/1     1            1           2m1s
deployment.apps/openshift-adp-controller-manager    1/1     1            1           25m
deployment.apps/velero                              1/1     1            1           2m1s

NAME                                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/oadp-velero-sample-1-aws-registry-678ffd4bd9   1         1         1       2m1s
replicaset.apps/openshift-adp-controller-manager-548768c576    1         1         1       25m
replicaset.apps/velero-5fd9d9b5dd                              1         1         1       2m1s

NAME                                                               HOST/PORT                                                                              PATH   SERVICES                                PORT    TERMINATION   WILDCARD
route.route.openshift.io/oadp-velero-sample-1-aws-registry-route   oadp-velero-sample-1-aws-registry-route-oadp-operator.apps.amp-fb35.9.5.36.56.nip.io          oadp-velero-sample-1-aws-registry-svc   <all>                 None
```

### Configure the cpdbr-cli
Run the following command in bastion server.
```
wget http://icpfs1.svl.ibm.com/zen/cp4d-builds/4.0.0/dev/utils/cpdbr-oadp/latest/lib/ppc64le/cpdbr-oadp
chmod +x cpdbr-oadp
./cpdbr-oadp client config set namespace=oadp-operator
```
For the detailed usage, run ./cpdbr-oadp  --help 

## Backup and Restore
Note: before backup and restore, please make sure the NFS server is set as no_root_squash, or run vim /etc/exports with /export *(rw,sync,no_root_squash), and then restart NFS server.

### Run command to start backup process
```
./cpdbr-oadp backup create --include-namespaces=nas3 --exclude-resources='Event,Event.events.k8s.io' --default-volumes-to-restic --snapshot-volumes=false --cleanup-completed-resources nas3backup --log-level=debug –verbose
```
Note: 
* nas3 is the Merlin operator namespace name. Add additional namespaces after nas, separated by commas.
* nas3backup is the backup instance name, which is replaced with users' namespace.

Success logs as following:
```
oadp namespace: oadp-operator, processing request...
resolved namespaces [nas3] ...
W0226 09:17:52.188481  140375 warnings.go:67] batch/v1beta1 CronJob is deprecated in v1.21+, unavailable in v1.25+; use batch/v1 CronJob
pre-processing rules will be applied for the following resource(s):
   (0) nas3/deployment/merlin-operator: op=<mode=quiesce,type=pod-scale>
         priority=90
   (1) nas3/deployment/vault: op=<mode=quiesce,type=pod-scale>
         priority=90
   (2) nas3/deployment/ibm-common-service-operator: op=<mode=quiesce,type=pod-scale>
         priority=90
   (3) nas3/deployment/keycloak: op=<mode=quiesce,type=pod-scale>
         priority=90
   (4) nas3/statefulset/engine: op=<mode=quiesce,type=pod-scale>
         priority=90
   (5) nas3/statefulset/merlin: op=<mode=quiesce,type=pod-scale>
         priority=90
   (6) nas3/statefulset/postgres: op=<mode=quiesce,type=pod-scale>
         priority=90
 
running 7 async ops in namespaces [nas3]...
waiting for 7 async ops in namespaces [nas3]...
waiting for quiesce to be completed...
waiting for quiesce to be completed...
waiting for quiesce to be completed...
waiting for quiesce to be completed...
waiting for quiesce to be completed...
nas3/deployment/keycloak scaled down successfully
waiting for quiesce to be completed...
nas3/deployment/ibm-common-service-operator scaled down successfully
waiting for quiesce to be completed...
nas3/deployment/vault scaled down successfully
nas3/deployment/merlin-operator scaled down successfully
nas3/statefulset/merlin scaled down successfully
nas3/statefulset/postgres scaled down successfully
nas3/statefulset/engine scaled down successfully
status: succeeded
   nas3/deployment/merlin-operator: op=<mode=quiesce,type=pod-scale>, status=succeeded
   nas3/deployment/vault: op=<mode=quiesce,type=pod-scale>, status=succeeded
   nas3/deployment/ibm-common-service-operator: op=<mode=quiesce,type=pod-scale>, status=succeeded
   nas3/deployment/keycloak: op=<mode=quiesce,type=pod-scale>, status=succeeded
   nas3/statefulset/engine: op=<mode=quiesce,type=pod-scale>, status=succeeded
   nas3/statefulset/merlin: op=<mode=quiesce,type=pod-scale>, status=succeeded
   nas3/statefulset/postgres: op=<mode=quiesce,type=pod-scale>, status=succeeded
 
configmap pre-backup hooks took 11.896119058s
configmap pre-backup hooks completed
creating pod cpdbr-vol-mnt in namespace nas3
creating done in namespace nas3
annotation processing in namespace nas3
annotation processing in namespace nas3
annotate pvc "merlin-engine-backend" with cpdbr.cpd.ibm.com/volume-backup-method=restic
annotate pvc "merlin-gui-file-backend" with cpdbr.cpd.ibm.com/volume-backup-method=restic
annotate pvc "merlin-postgresql-claim" with cpdbr.cpd.ibm.com/volume-backup-method=restic
backup request "nas3backup" submitted successfully.
waiting for backup to complete...
.................................................................................................................................................................................................................................................................................................
backup completed with status: Completed.
deleting pod cpdbr-vol-mnt in namespace nas3
deletion done in namespace nas3
W0226 09:23:38.618939  140375 warnings.go:67] batch/v1beta1 CronJob is deprecated in v1.21+, unavailable in v1.25+; use batch/v1 CronJob
post-processing rules will be applied for the following resource(s):
   (0) nas3/statefulset/postgres: op=<mode=unquiesce,type=pod-scale>
         priority=90
   (1) nas3/statefulset/merlin: op=<mode=unquiesce,type=pod-scale>
         priority=90
   (2) nas3/statefulset/engine: op=<mode=unquiesce,type=pod-scale>
         priority=90
   (3) nas3/deployment/vault: op=<mode=unquiesce,type=pod-scale>
         priority=90
   (4) nas3/deployment/merlin-operator: op=<mode=unquiesce,type=pod-scale>
         priority=90
   (5) nas3/deployment/keycloak: op=<mode=unquiesce,type=pod-scale>
         priority=90
   (6) nas3/deployment/ibm-common-service-operator: op=<mode=unquiesce,type=pod-scale>
         priority=90
 
running 7 async ops in namespaces [nas3]...
scaling up nas3/statefulset/postgres to its original replicas(1)...
scaling up nas3/deployment/merlin-operator to its original replicas(1)...
scaling up nas3/deployment/vault to its original replicas(1)...
scaling up nas3/statefulset/merlin to its original replicas(1)...
scaling up nas3/statefulset/engine to its original replicas(1)...
scaling up nas3/deployment/ibm-common-service-operator to its original replicas(1)...
scaling up nas3/deployment/keycloak to its original replicas(1)...
status: succeeded
   nas3/statefulset/postgres: op=<mode=unquiesce,type=pod-scale>, status=succeeded
   nas3/statefulset/merlin: op=<mode=unquiesce,type=pod-scale>, status=succeeded
   nas3/statefulset/engine: op=<mode=unquiesce,type=pod-scale>, status=succeeded
   nas3/deployment/vault: op=<mode=unquiesce,type=pod-scale>, status=succeeded
   nas3/deployment/merlin-operator: op=<mode=unquiesce,type=pod-scale>, status=succeeded
   nas3/deployment/keycloak: op=<mode=unquiesce,type=pod-scale>, status=succeeded
   nas3/deployment/ibm-common-service-operator: op=<mode=unquiesce,type=pod-scale>, status=succeeded
 
configmap post-backup hooks took 1.650912114s
configmap post-backup hooks completed
annotation processing in namespace nas3
annotation "backup.velero.io/backup-volumes" does not exist in pod "engine-0", skipping...
annotation "backup.velero.io/backup-volumes" does not exist in pod "merlin-0", skipping...
annotation "backup.velero.io/backup-volumes" does not exist in pod "merlin-operator-76bcfb4969-pzpwn", skipping...
annotation "backup.velero.io/backup-volumes" does not exist in pod "postgres-0", skipping...
annotation "backup.velero.io/backup-volumes" does not exist in pod "vault-75584d7fc9-tj77f", skipping...
annotation processing in namespace nas3
removing annotation "cpdbr.cpd.ibm.com/volume-backup-method" from pvc "merlin-engine-backend"
removing annotation "cpdbr.cpd.ibm.com/volume-backup-method" from pvc "merlin-gui-file-backend"
removing annotation "cpdbr.cpd.ibm.com/volume-backup-method" from pvc "merlin-postgresql-claim"
backup took 5m48.309030505s
 
backup command completed
post-backup hooks are invoked, but applications may take longer to come up.
please use 'oc get po', 'oc get sts', 'oc get deploy' to check workload status.
```

### Delete the Merlin instance, but make sure the namespace still exist. 
Use following command to start restore process
```
./cpdbr-oadp restore create --from-backup=nas3backup nas3t5 --include-cluster-resources=true --log-level=debug –verbose

Note: nas3t5 is the restore name which can be replaced.

See the log as following:
resolved namespaces [nas3]
restore request "nas3t5" submitted successfully.
waiting for restore to complete...
.....................................................
restore completed with status: Completed.
deleting pod cpdbr-vol-mnt in namespace nas3
deletion done in namespace nas3
W0226 10:48:07.683918  142289 warnings.go:67] batch/v1beta1 CronJob is deprecated in v1.21+, unavailable in v1.25+; use batch/v1 CronJob
post-processing rules will be applied for the following resource(s):
   (0) nas3/statefulset/merlin: op=<mode=unquiesce,type=pod-scale>
         priority=90
   (1) nas3/statefulset/engine: op=<mode=unquiesce,type=pod-scale>
         priority=90
   (2) nas3/statefulset/postgres: op=<mode=unquiesce,type=pod-scale>
         priority=90
   (3) nas3/deployment/ibm-common-service-operator: op=<mode=unquiesce,type=pod-scale>
         priority=90
   (4) nas3/deployment/vault: op=<mode=unquiesce,type=pod-scale>
         priority=90
   (5) nas3/deployment/merlin-operator: op=<mode=unquiesce,type=pod-scale>
         priority=90
   (6) nas3/deployment/keycloak: op=<mode=unquiesce,type=pod-scale>
         priority=90
 
running 7 async ops in namespaces [nas3]...
scaling up nas3/deployment/ibm-common-service-operator to its original replicas(1)...
scaling up nas3/statefulset/postgres to its original replicas(1)...
scaling up nas3/deployment/vault to its original replicas(1)...
scaling up nas3/statefulset/engine to its original replicas(1)...
scaling up nas3/statefulset/merlin to its original replicas(1)...
scaling up nas3/deployment/merlin-operator to its original replicas(1)...
scaling up nas3/deployment/keycloak to its original replicas(1)...
status: succeeded
   nas3/statefulset/merlin: op=<mode=unquiesce,type=pod-scale>, status=succeeded
   nas3/statefulset/engine: op=<mode=unquiesce,type=pod-scale>, status=succeeded
   nas3/statefulset/postgres: op=<mode=unquiesce,type=pod-scale>, status=succeeded
   nas3/deployment/ibm-common-service-operator: op=<mode=unquiesce,type=pod-scale>, status=succeeded
   nas3/deployment/vault: op=<mode=unquiesce,type=pod-scale>, status=succeeded
   nas3/deployment/merlin-operator: op=<mode=unquiesce,type=pod-scale>, status=succeeded
   nas3/deployment/keycloak: op=<mode=unquiesce,type=pod-scale>, status=succeeded
 
configmap post-restore hooks took 1.748747619s
configmap post-restore hooks completed
restore took 1m35.551002154s
 
restore command completed
post-restore hooks are invoked, but applications may take longer to come up.
```
Wait a moment and check the pods status in the openshift console, all pods will be ready status.  
* Note: the merlin-operator-XXX maybe not be ready in time, wait a long time and then it will be ready status.  
* Note: If the ibm-common-service-operator-XXXX still not ready, delete the pod and operators, then install a new one “IBM Cloud Pak foundational services” in the namespace.
