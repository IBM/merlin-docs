# How to Update Merlin and Merlin tools CA and Certificate for 1.0.1 
**Note**: These steps are need for upgrading Merlin from 1.0.0 to 1.0.1. A new installation of Merlin 1.0.1 **DOES NOT** need these steps. 

Merlin 1.0.1 renews the Merlin CA and Certificate duration, the following steps need to be followed if you want to adopt it. A more automatic way inside Merlin will be provided in the feature release.

## Merlin platform
```
oc project <merlin-namespace>
```
```
oc delete operandrequest merlin-operandrequest
```
```
oc delete certificates merlin-serving-ca-cert merlin-serving-cert
```
```
oc delete secret merlin-https-cert merlin-ca-secret
```
```
oc get pods | grep merlin-operator
```
command output like this:  
merlin-operator-7c868bb499-s62gc               1/1     Running   0             27h  
```
oc delete pod merlin-operator-7c868bb499-s62gc
```
**Note**: The pod name should be matched with the above ```oc get pods``` command.  

Wait for the secret merlin-https-cert merlin-ca-secret re-generated in ```<merlin-namespace>```, restart all pods in ```<merlin-namespace>```.

```
oc delete routes keycloak merlin rest
```

Wait for these routes re-generated.

**Note**: Merlin admin may need to Unseal the Vault server and set Token through Merlin GUI->Connections->Vault Control to refresh Vault.

## IBM i Developer tools
Since the Merlin platform services certificates had been updated, so we need to update the truststore of IBM i Developer which contians the Merlin platform services certificates, so that the IBM i Developer can connect to Merlin platform services thought TLS. 
```
oc get secret merlin-https-cert -n <merlin-namespace> -o jsonpath='{.data.ca\.crt}' | base64 -d > ca.crt
```
```
oc delete configmap merlin-https-cert -n <ide-namespace>
```
```
oc create configmap merlin-https-cert -n <ide-namespace>  --from-file=ca.crt
```
Restart all pods in ```<ide-namespace>```. 

After the certificates updated, you don’t need to delete and recreate your existing workspace, you just need to restart it either using Close Workspace inside the IDE or the Restart Workspace in the Dashboard.

**Note**: This section is actually updating the truststore of IBM i Developer tools workspace which stores the certificates that using for connecting to Merlin Platform services but not the certificates that IBM i Developer tools workspace provided for external communications.

## IBM i CI/CD tools
```
oc delete secret merlin-https-cert -n <cicd-namespace>
```
```
oc get secret merlin-https-cert --namespace=<merlin-namespace> -o yaml | sed 's/namespace: .*/namespace: <cicd-namespace>/' | oc apply -f -
```
Restart all pods in ```<cicd-namespace>```.

```
oc delete routes ibmi-cicd jenkins -n <cicd-namespace>
```

Wait for these routes re-generated.
## IBM i servers
Go to Merlin GUI->Connections->Templates->Install Certificate, click Run.

# How to Update Merlin and Merlin tools expired Certificates

## Merlin platform
The Merlin CA and certificates and their related secrets will be renewed automatically. All you need to do are restart each pods of the Merlin platform.

## IBM i Developer tools, IBM i CI/CD tools and IBM i servers 
Same as Update Merlin and Merlin tools CA and Certificate for 1.0.1.
