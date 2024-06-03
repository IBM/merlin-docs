# Configure CatalogSource to get upgrade source
This document will describe how to get the available upgrade sources when you are upgrading from a Red Hat OpenShift cluster with internet access. All the operations require the cluster administrator's authority.    
The IBM Operator Catalog image provides a catalog of IBM offerings and is enabled in the OpenShift Operator catalog by deploying a CatalogSource resource. The user can update the image location which is specified in the deployed CatalogSource object by using the OpenShift CLI or web console to get the stable upgrade source. The user can choose one of the methods to change the image location.   
## Configure CatalogSource with OpenShift Web Console 
- Log in to Openshift GUI, Select **Administration** -> **Cluster Settings**, click **Global Configuration**, find **OperatorHub** and click. 
- From the list of deployed catalog sources, find **ibm-operator-catalog** and click, and then click the **YAML** tab.
- Edit the image location as `icr.io/cpopen/ibm-operator-catalog:latest` and click Save.
```yaml
spec:
  displayName: IBM-Operator
  image: >-
   icr.io/cpopen/ibm-operator-catalog:latest
  publisher: IBM
  sourceType: grpc
  updateStrategy:
    registryPoll:
      interval: 45m
```
## Configure CatalogSource with Openshift CLI 
- Log in to the OpenShift cluster by using the oc login command
- Run the following command to verify that a CatalogSource object is deployed for the IBM Operator Catalog:    

```
oc get catalogsources -n openshift-marketplace 

NAME                   DISPLAY               TYPE   PUBLISHER   AGE
ibm-operator-catalog   IBM-operator          grpc   IBM         24h
...
```  
- Use below commands to partially update the ibm-operator-catalog CatalogSource object.  
```
oc patch catalogsource ibm-operator-catalog -n openshift-marketplace -p '{"spec":{"image":icr.io/cpopen/ibm-operator-catalog:latest}}' --type merge 
```  
or   
```
oc edit catalogsource ibm-operator-catalog -n openshift-marketplace 
```  

After those steps, the user could follow [How to manually upgrade Merlin operator](./upgrade_merlin_operator.md) and [How to manually upgrade IBM i Developer Integrated Development Environment and IBMI i CI/CD Tools](./upgrade_tools.md) to upgrade there Merlin operator and tools.