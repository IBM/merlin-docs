# Security considerations
There are several aspects that need to be considered to ensure operating in a secure environment.

This topic provides security recommendations for setting up Data In Motion Encryption (DIME), Data At Rest Encryption (DARE), Network Policies and Certificates. It is intended to help you create a secure implementation of the application.

> Note: Merlin does not support the multi-factor authentication capability introduced with IBM i 7.6.

## Data at rest

All user sensitive data at rest is stored in Hashicorp Vault. Hashicorp Vault is used to secure, store and tightly control access to tokens, passwords, certificates, encryption keys for protecting secrets and other sensitive data using a UI, CLI, or HTTP API. Merlin leverages HashiCorp Vault as the Active Encryption solution to store sensitive data such as credentials. Users do not need to directly work with HashiCorp Vault. Vault encrypts data in transit (with TLS) and at rest (using AES 256-bit CBC encryption). This protects sensitive data from unauthorized access in two major ways: as it travels across the network as well as in storage in the cloud and data centers. But it is necessary for OpenShift administrator to understand that Vault is used underneath. For more information, please read the content of the following link: https://learn.hashicorp.com/vault. 

## Data in motion
Data in motion is encrypted using transport layer security (TLS 1.2) to secure all inbound and outbound requests from Merlin. 

## Network Policies
Network Policies are an application-centric construct which allow Merlin to specify how a pod is allowed to communicate with various network "entities" over the network. NetworkPolicies apply to a connection with a pod on one or both ends, and are not relevant to other connections.

Four Network Policies are created by Merlin
* engine - defines that 'engine-0' pod is only allowed to be accessed by other pods, which are in the same namespace and owned by same Merlin instance, and the workspaces created by the same Merlin instance.
* postgres - defines that the 'postgres-0' can only be accessed by the engine, vault, and keycloak pods in the same namespace. 
* keycloak - defines that the 'keycloak-0' can only be accessed by the engine, and vault pods in the same namespace. 
* vault - defines that the 'vault-0' can only be accessed by the engine pod in the same namespace. 

## Manage certificate for Merlin
Merlin uses cert-manager to manage all the certifications. 
For more information refer to https://www.ibm.com/docs/en/cloud-paks/cp-management/1.2.0?topic=management-certificate-manager-cert-manager

Merlin first creates a SelfSigned issuer resource. The SelfSigned issuer is for initially bootstrapping a CA issuer. Then it issues a root certificate and uses that root as a CA issuer. 
The CA issuer will sign the Merlin services certificate based on the private key.

The CA certificate is stored in the secret merlin-ca-secret, and the Merlin services certificate is stored in the secret merlin-https-cert.

Merlin external routes are re-encrypt secured routes, see https://docs.openshift.com/container-platform/4.10/networking/routes/secured-routes.html for more detail about secured routes. The route resource is using the Openshift default ingress certificate and the reencrypt TLS termination to destination CA certificate to enable the Ingress Controller to trust the service’s certificate.

There are two steps to do if you want to use a custom certificate for both Merlin services and routes:
1. To use a custom certificate for Merlin services, manually create the merlin-ca-secret which contains the TLS key and certificate under the same namespace within Merlin before installing Merlin. When installing Merlin, Merlin will not create the root CA but instead it will use the custom one as the root CA to sign the Merlin services certificate.

    Use the following oc admin command to create the secret:

    ```
    oc create secret tls merlin-ca-secret --cert=tls.crt --key=tls.key -n <merlin-namespace>
    ```
    **Note**: You should prepare the tls.crt and tls.key before run the command.

2. To use a custom certificate for Merlin routes, refer to Openshift documentation https://docs.openshift.com/container-platform/4.10/security/certificates/replacing-default-ingress-certificate.html

    **Note**: This will replace the Openshift cluster default certificate.

## Manage Merlin secrets
There are several secrets created when Merlin platform has been deployed. The key secrets are listed as follows:
* merlin-ca-secret
* merlin-credential-secret
* merlin-https-cert
* merlin-jks-pkcs12-secret
* merlin-third-party-cert

## Verifying container image security
### Customer Image Verification
**Important**: Customer verification of container image signatures is not required at this point in time.  This documentation will provide a way for teams to enable their customers to verify the image signatures, but the customer can chose not to run the verification.  

After the customer downloads the images to their bastion server, they must do image signature verification before they transfer the downloaded images to their disconnected network.  Customers have the option to disable image signature verification, but this is a customer specific setting.  There are a few different options that can be used to verify the image signature, depending on the tooling the customer has, and this section will provide some examples and guidance around verifying the image signature. For more detailed signature verification documentation, visit the IBM CISO Code Sign Service Documentation.

### Enabling Customers to Verify the Image Signature
The customer needs access to the public portion of the pgp key used to sign the image,  in order to verify the image signature of the downloaded image.  The ```$ucl pgp-key -n <alias>``` command will export the pgp key to your gpg key ring.  The private portion of this key will be a pointer to the HSM where the actual key data is stored, and the public portion will be readily available in the key ring.  This public key data will need to be provided to the customer, in order for them to verify the image signature.  The way a customer gets this public key data MUST be declared in the product offering's README file or Knowledge Center.  

### Portieris
Portieris is the open source version of CISE, which is an admission controller for Kubernetes.  Portieris is being enhanced to support Red Hat signed image verification, and this will work out of the box on Red Hat OpenShift.  The admission controller will be installed to the cluster, and only allow images it can verify to be spun up and run inside your cluster.   

### OCP Verification 

OCP verification is verification using any tool that relies on the atomic framework like: ```skopeo```, ```podman```, or ```oc```.  

In OCP 4, signature validation for Red Hat signed images comes automatically when the image is pulled to the platform.  The /etc/containers/policy.json file is what drives the verification of image signatures in OCP. Follow these steps to configure automatic signature verification on OCP 4: 

**Note**: If the customer wants to take advantage of this feature for the oc image, then the public key for your offering must be imported to the disk of the machines for the cluster 

  1. Configure client to read from the entitled registry
  2. Create the necessary configuration files 
    a. /etc/containers/policy.json
    b. Any connection info files for the registry pulling images from ex: us.icr.io.yaml
  3. Base64 encode the files and export to the environment
  4. Create a machine config that will write those files to disk on the worker nodes
  5. Apply the machine config yaml to the cluster
  6. Repeat for the master nodes
  7. Verify the changes took place by describing the machine configs, refer to https://docs.openshift.com/container-platform/4.9/security/container_security/security-container-signature.html

### Configure client to read from the entitled registry

In order to configure the client binary to read from the entitled registry, there must be credentials provided to the docker config file ```$HOME/.docker/config.json```.

Note: If using ```podman``` to perform the image pull and verification, then a ```podman login``` to the entitled registry is all that is required to provide the necessary auth information.
  
    {
        "auths": {
          "us.icr.io": {
            "username": "<Username (cp, iamapikey, ekey)>",
          "password": "<API Key>"
        }
      },
    ...
    }

Example Policy JSON File for Entitled Registry: 
```/etc/containers/policy.json```

    {
      "default": [
        {
          "type": "insecureAcceptAnything"
        }
      ],
      "transports": {
        "docker": {
          "cp.icr.io": [
            {
              "type": "signedBy",
              "keyType": "GPGKeys",
              "keyPath": "PATH_TO_PUBLIC_GPG_KEY_ON_MACHINE"
            }
          ]
        },
        "docker-daemon": {
          "": [
            {
              "type": "insecureAcceptAnything"
            }
          ]
        }
      }
    } 
 
### Disabling Image Signature Verification 

To turn off image signature verification the /etc/containers/policy.json file must be updated. 

    {
      "default": [
        {
          "type": "insecureAcceptAnything"
        }
      ],
      "transports": {}
    }
