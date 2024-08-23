# Install IBM i Modernization Engine for Lifecycle Integration online

If the cluster is connected to the internet, IBM i Modernization Engine For Lifecycle Integration can be installed in the cluster online.

Log in to the OpenShift cluster with oc login before performs any steps that use the Red Hat® OpenShift® Container Platform command-line interface (CLI).

## Preparation procedure

Complete these prerequisite tasks to prepare for the IBM i Modernization Engine For Lifecycle Integration installation.

   * Install OpenShift
   * Install the OpenShift CLI
   * Configure storage
   * Create the entitlement key secret
   * Configure network policies
   * Create the catalog source
   * Install the operator
   * Create the custom resource
   * Verify the installation
   * Access the Merlin console

## Install OpenShift

IBM i Modernization Engine For Lifecycle Integration requires OpenShift to be installed and running. An administrative access to the OpenShift cluster is required.

For information on supported versions of OpenShift, see [Supported product versions](./guides/platform/Install_IBM_i_Modernization_Engine_for_Lifecycle_Integration.md).

Install OpenShift using the instructions in Installing [Red Hat OpenShift Container Platform](https://www.ibm.com/docs/en/openshift?source=https%3A%2F%2Fdocs.openshift.com%2Fcontainer-platform%2F4.8%2Fwelcome%2Findex.html).

## Install the OpenShift CLI

Install the OpenShift command line interface (oc) on the cluster's boot node and run oc login, using the instructions in [Getting started with the OpenShift CLI](https://docs.openshift.com/container-platform/4.8/cli_reference/openshift_cli/getting-started-cli.html).

## Configure storage

The storage configuration must satisfy the sizing requirements. For more information on the storage classes that are needed for installing IBM i Modernization Engine For Lifecycle Integration, see [Configure a default storage class](./guides/platform/Data_Storage_for_MERLIN.md).

## Create the entitlement key secret

Complete the following steps to create a docker-registry secret to enable the deployment to pull the IBM i Modernization Engine For Lifecycle Integration images from the IBM® Entitled Registry.

### Create the entitlement key secret with either of the following methods

Obtain the entitlement key that is assigned to the IBMid. Log in to [MyIBM Container Software Library](https://myibm.ibm.com/products-services/) with the IBMid and password details that are associated with the entitled software. Then configure the global image pull secret on the Openshift environment.

- Extract the current global image pull secret from the cluster into a file in the current directory named .dockerconfigjson:
```
oc extract secret/pull-secret -n openshift-config --to=.
```
- Create a base64 encoded string with entitled registry userid and password:
```
printf "cp:<entitlementkey>" | base64
```
- Edit the .dockerconfigjson file and ADD a new JSON object to the auths object to it with the credentials for the staging repository.  For example:
```
"cp.icr.io": {
"auth": "Replace-With-The-Encoded-String-Got-In-Last-Step",
"email": "Replace-With-Your-Email"
}
```
- Update the global image pull secret with the updated credentials:
```
oc set data secret/pull-secret -n openshift-config --from-file=.dockerconfigjson
```
-	Monitor the node status using the command: oc get nodes
- When the nodes are finish restarting, the cluster is now ready to pull images from the staging registry with production image references.
> Note: If the OpenShift cluster is deployed using Red Hat OpenShift on IBM Cloud, the nodes will not be restarted automatically to pick up this global configuration change.  
The cluster admin needs to restart the nodes following the instructions in this link: [reload nodes](https://cloud.ibm.com/docs/openshift?topic=openshift-registry#:~:text=To%20pick%20up%20the%20global%20configuration%20changes%2C%20reload%20all%20the%20worker%20nodes%20in%20your%20cluster)

## Install IBM catalog source

Add the IBM i Modernization Engine For Lifecycle Integration catalog source to the OpenShift cluster.

Create the catalog source with either of the following methods:

- Option 1: Create the catalog source with the OpenShift console
- Option 2: Create the catalog source with the OpenShift CLI

### Option 1: Create the catalog source with the OpenShift console

- Log in to the OpenShift cluster's console.
- Add the IBM Operators CatalogSource.
- Click the plus icon in the upper right corner to open the Import YAML dialog box, paste in the following content, and then click Create.
```   
    apiVersion: operators.coreos.com/v1alpha1
    kind: CatalogSource
    metadata:
      name: ibm-operator-catalog
      namespace: openshift-marketplace
    spec:
      displayName: ibm-operator-catalog
      publisher: IBM Content
      sourceType: grpc
      image: icr.io/cpopen/ibm-operator-catalog:latest
      updateStrategy:
        registryPoll:
          interval: 45m
```
- Go to Administration > Cluster Settings. Under Global Configuration > OperatorHub > Sources, verify that the ibm-operator-catalog CatalogSource object is present.

### Option 2: Create the catalog source with the OpenShift CLI

- Add the IBM Operators CatalogSource by running the following command:
```
    cat << EOF | oc apply -f -
    apiVersion: operators.coreos.com/v1alpha1
    kind: CatalogSource
    metadata:
      name: ibm-operator-catalog
      namespace: openshift-marketplace
    spec:
      displayName: ibm-operator-catalog
      publisher: IBM Content
      sourceType: grpc
      image: icr.io/cpopen/ibm-operator-catalog:latest
      updateStrategy:
        registryPoll:
          interval: 45m
    EOF
```
- Verify that the ibm-operator-catalog CatalogSource object is present, and is returned by the following command.
```
oc get CatalogSources ibm-operator-catalog -n openshift-marketplace

    Example output:

    oc get CatalogSources ibm-operator-catalog -n openshift-marketplace
    NAME                   DISPLAY                 TYPE   PUBLISHER   AGE
    ibm-operator-catalog   IBM Operator Catalog    grpc   IBM         4h13m
```

## Install the operator

Install Merlin operator with either of the following installation methods:

- Option 1: Install the operator with the OpenShift console
- Option 2: Install the operator with the OpenShift CLI

### Option 1: Install the operator with the OpenShift console

- Log in to the OpenShift cluster's console.
- Click Operators > OperatorHub. The OperatorHub page is displayed.
- In the All Items field, enter IBM i Modernization Engine for Lifecycle Integration. The Merlin Operator is displayed.
- Click the IBM i Modernization Engine for Lifecycle Integration tile. The IBM i Modernization Engine for Lifecycle Integration window is displayed.
- Click Install. The Install Operator page is displayed.
- Enter the following values:

  * Set the Namespace to be `openshift-operators` in which to install the Operator.
  * Set Update Channel to v2.0.
  * Set Approval Strategy to Automatic.

- Click Install and wait for the Merlin operator to install.
- Verify that the Merlin operator is successfully installed.
- Navigate to Operators > Installed Operators, and select the project from the Projects dropdown. IBM i Modernization Engine for Lifecycle Integration and its dependant operator in the project are listed with a status of `Succeeded`.
- Navigate to Workloads > Pods, and select project `ibm-common-services` from the Projects dropdown. make sure the pods listed below are listed with a status of `Running`.
  - ibm-common-service-webhook
  - ibm-namespace-scope-operator
  - operand-deployment-lifecycle-manager
  - secretshare

### Option 2: Install the operator with the OpenShift CLI

Install the Merlin operator with the following command.
```
cat << EOF | oc apply -f -
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: ibmi-merlin-operator
  namespace: openshift-operators
spec:
  channel: v2.0
  installPlanApproval: Automatic
  name: ibmi-merlin-operator
  source: ibm-operator-catalog
  sourceNamespace: openshift-marketplace
EOF
```

After a few minutes, the operator is installed. Verify that the all components are in the `Succeeded` state by running the following command:
```
oc get csv -n openshift-operators | grep ibm-merlin
```

> Note: Make sure the namespace/project for Merlin operator is `openshift-operators`.

## Deploy Merlin instance
Once Merlin operator has been installed in project `openshift-operators`, Merlin instance can be installed. Note that the instance must be installed in the same project `openshift-operators` as the operator.
1. Click on 'Installed Operators' under 'Operators' section in the left navigation panel of OCP. 
2. Two operators will be shown. Click the link of 'IBM i Modernization Engine for LifeCycle Integration' operator.
3. Click on 'Details' tab of Operator details page. 
4. Click on the link of 'Create Instance' of Merlin. 
5. Fill in the following items
```
    * Name
    * Pull Policy
    * ConfigurationProfileName
    * License
    * Version
```
> Note: Don't specify the value of StorageClassName, otherwise it cannot create Merlin instance successfully.
6. Click on the 'Create' button.
7. Check the status of Merlin until the status turns into 'Conditions: DependenciesSatisfied, Reconciled' by running 'oc describe merlins/merlin'. 

```
Status:
  Application URL:  https://merlin-zhulj-new-catalog.apps.amp-fb35.nip.io/merlin
  Conditions:
    Last Transition Time:  2022-04-27T14:58:19Z
    Last Update Time:      2022-04-27T15:01:25Z
    Status:                True
    Type:                  DependenciesSatisfied
    Last Transition Time:  2022-04-27T14:59:44Z
    Last Update Time:      2022-04-27T15:02:53Z
    Status:                True
    Type:                  Reconciled
  Keycloak URL:            https://keycloak-zhulj-new-catalog.apps.amp-fb35.nip.io
  Op Version:              1.0.0

    
* Application URL: The 'Application URL' status property is a string flag that stands for the URL of Merlin install.
* Conditions:
The 'DependenciesSatisfied' condition will be set 'True' when the CustomResourceDefinition merlins.merlin.ibm.com is ready.
The 'Reconciled' condition, for example, will be set 'True' when all of the Pods are 'Ready'.
* Keycloak URL: it is a string that stands for the URL of Keycloak install.
* Op Version: it is a string that stands for the version of Operator.
```

8. After Merlin has been deployed, the Merlin URL can be obtained from the Networking->Routes. 

## Check Pods' status
The below Pods are created after the deployment of Merlin instance. 
```
    * engine
    * keycloak
    * merlin
    * postgres
    * vault
```
## Check Merlin routes
Three routes are created for Merlin platform
```
    * merlin
    * keycloak
    * rest
```
## Check Merlin network policies
Run 'oc get networkpolicy'

```
oc get networkpolicy
NAME       POD-SELECTOR                           AGE
engine     app.kubernetes.io/component=engine     11m
keycloak   app.kubernetes.io/component=keycloak   11m
postgres   app.kubernetes.io/component=postgres   11m
vault      app.kubernetes.io/component=vault      11m
```

## Obtain Merlin platform initial password
Go to Workloads->Secrets. Find merlin-credential-secret. The initial user name and password of Merlin Platform are stored in this secret. The user name is stored in ADMIN_USERNAME, and the password is stored in ADMIN_PASSWORD. 

