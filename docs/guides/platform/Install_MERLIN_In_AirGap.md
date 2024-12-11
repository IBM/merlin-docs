# Install IBM i Modernization Engine for Lifecycle Integration in AirGap environment

IBM i Modernization Engine for Lifecycle Integration can be installed on a RedHat OpenShift Container Platform in that has no internet connectivity.

## Prerequisites

An OpenShift Container Platform cluster must be installed and meet the resource requirements. For the supported OpenShift Container Platform versions and requirements, see [Supported product versions](./guides/platform/Install_IBM_i_Modernization_Engine_for_Lifecycle_Integration.md).

A bastion server must be configured. For more information, see [Prepare a bastion host](#prepare-a-bastion-host).

A local Docker registry that is accessible from both the bastion server and the OpenShift Container Platform cluster nodes must be available. For more information, see [Prepare a Docker registry](#prepare-a-local-docker-registry).

## Prepare a bastion host

Prepare a bastion host that can access the OpenShift Container Platform cluster, the local Docker registry, and the internet. The bastion host must be on a Linux® platform with any operating system that the IBM Catalog Management Plug-in and the OpenShift Container Platform CLI support.

Complete these steps on the bastion node:

* Install Docker or Podman  
To install Docker, run these commands:
```bash
yum check-update
yum install docker
```
Start the Docker service
```bash
systemctl enable docker
systemctl start docker
```

* Install httpd-tools
```bash
yum install httpd-tools
```

* Install the oc OpenShift Container Platform CLI tool. For more information, see [OpenShift Container Platform CLI tools](https://docs.openshift.com/container-platform/4.14/cli_reference/openshift_cli/getting-started-cli.html).

* Install the IBM Catalog Management Plug-in

Download and install the most recent version of IBM Catalog Management Plug-in for IBM Cloud Paks from the [IBM/ibm-pak](https://www.ibm.com/links?url=https%3A%2F%2Fgithub.com%2FIBM%2Fibm-pak). Extract the binary file by entering the following command:
```
tar -xf oc-ibm_pak-linux-amd64.tar.gz
```
Run the following command to move the file to the `/usr/local/bin` directory.
```
mv oc-ibm_pak-linux-amd64 /usr/local/bin/oc-ibm_pak
```
You can confirm that oc ibm-pak -h is installed by running the following command:
```
oc ibm-pak --help
```
The plug-in usage is displayed.

For more information on plug-in commands, see [command-help](https://www.ibm.com/links?url=https%3A%2F%2Fgithub.com%2FIBM%2Fibm-pak%2Fblob%2Fmain%2Fdocs%2Fcommand-help.md).

### Prepare a local Docker registry

A local Docker registry must be created to mirror all images in the local environment. The registry must meet the following requirements:

* Support Docker Manifest V2, Schema 2.
* Support multi-architecture images. Note: Do not use OpenShift image registry as the local registry. The OpenShift registry does not support multi-architecture images.
* Is accessible from both the bastion server and the OpenShift Container Platform cluster nodes.
* Has the username and password of a user who can write to the target registry from the bastion host.
* Has the username and password of a user who can read from the target registry that is on the OpenShift cluster nodes.
* Allow path separators in the image name.

Below are the steps in Prepare the multi-architecture registry to create a multi-architecture registry.

#### Docker Prerequisites

A Red Hat® Enterprise Linux® (RHEL) server must be used as the registry host.

The registry host must have access to the internet.

#### Create the Docker registry

Complete these steps to create the Docker registry.

Install the required packages.

```bash
yum -y install docker httpd-tools
```

Start the Docker service.

```bash
systemctl enable docker
systemctl start docker
```

Create folders for the registry.

```bash
mkdir -p /opt/registry/{auth,certs,data}
```

Provide a certificate for the registry. If there is a certificate from a trusted certificate authority (CA), proceed with the next step. Otherwise, a self-signed certificate should be generated.

Change to the /opt/registry/certs directory.

```bash
cd /opt/registry/certs
```

Generate a certificate.

```bash
openssl req -newkey rsa:4096 -nodes -sha256 \
    -keyout domain.key -x509 -days 365 \
    -out domain.crt \
    -addext "subjectAltName=DNS:<the_registry_hostname>"
```

At the prompts, provide the required values for the certificate:

```text
Country Name (two-letter code)    
Specify the two-letter ISO country code for the location. See the ISO 3166 country codes standard.
State or Province Name (full name)    
Enter the full name of the state or province.
Locality Name (for example, city)    
Enter the name of the city.
Organization Name (for example, company)    
Enter the company name.
Organizational Unit Name (for example, section)    
Enter the department name.
Common Name (for example, the name or the hostname of the server)    
Enter the hostname for the registry host. Ensure that the hostname is in DNS and that it resolves to the expected IP address.
Email Address    
Enter the email address. For more information, see the req description in the OpenSSL documentation.
```

> Note: For the common name, make sure to enter a hostname that can be resolved to an IP address when log in to the Docker registry.

Generate a username and a password in the bcrpt format for the registry.

```bash
htpasswd -bBc /opt/registry/auth/htpasswd <registry_user_name> <registry_password>
```

Create the Docker registry container to host the registry.

```bash
docker run --name mirror-registry -p <the_registry_host_port>:5000 \
  -v /opt/registry/data:/var/lib/registry:z \
  -v /opt/registry/auth:/auth:z \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
  -v /opt/registry/certs:/certs:z \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  -e REGISTRY_COMPATIBILITY_SCHEMA1_ENABLED=true \
  -d docker.io/library/registry:2
```

> Note: For the_registry_host_port, specify the port that the Docker registry uses to serve content.

If the registry is behind a firewall, open the ports that the registry requires.

```bash
firewall-cmd --add-port=<the_registry_host_port>/tcp --zone=internal --permanent
firewall-cmd --add-port=<the_registry_host_port>/tcp --zone=public --permanent
firewall-cmd --reload
```

If a self-signed certificate is being use, add it to the list of trusted certificates.

```bash
cp /opt/registry/certs/domain.crt /etc/pki/ca-trust/source/anchors/
update-ca-trust
```

Verify that the registry is available.

```bash
curl -u <registry_user_name>:<registry_password> -k https://<the_registry_host_name>:<the_registry_host_port>/v2/_catalog
```

See these parameter descriptions:

```text
registry_user_name is the username to access the registry.
registry_password is the password of the registry user.
the_registry_host_name is the registry domain name that was specified in the certificate. For example, registry.example.com.
the_registry_host_port is the port that the Docker registry uses to serve content.
```

Following is a sample response:

```text
    {"repositories":[]}
```

Create a Docker certs folder and place the certificate in the folder.

```bash
mkdir -p /etc/docker/certs.d/<the_registry_host_name>:<the_registry_host_port>
cp /opt/registry/certs/domain.crt /etc/docker/certs.d/<the_registry_host_name>:<the_registry_host_port>/ca.crt
```

#### Configure the registry

After creating the registry, configure the Docker registry:

* Create registry namespaces.
* Create a separate registry namespace for each public registry source.
  * `cpopen` - Namespace to store all Operator images from the `icr.io/cpopen` namespace.
  * `cp/ibmi-merlin` - Namespace to store the IBM images from the `cp.icr.io/cp/ibmi-merlin` repository. The `cp/ibmi-merlin` namespace is for the images in the IBM Entitled Registry that require a product entitlement key and credentials to pull.
* Verify that each namespace meets the following requirements:
  * Supports auto-repository creation.
  * Has credentials of a user who can write and create repositories. The bastion host uses these credentials.
  * Has credentials of a user who can read all repositories. The OpenShift Container Platform cluster uses these credentials.

### Prepare to install IBM i Modernization Engine for Lifecycle Integration

Complete these steps on the bastion host.

#### Create environment variables for the installer and image inventory

Create the following environment variables with the installer CASE name and the image inventory.

```bash
export MERLIN_CASE_NAME=ibm-merlin
export CICD_CASE_NAME=ibm-merlin-cicd
export IDE_CASE_NAME=ibm-merlin-development-environment
export DEVWORKSPACE_CASE_NAME=ibm-merlin-devworkspace
export CASE_VERSION=<case_version>
```

To find the CASE name and version, see [IBM: Product CASE to Application Version](https://www.ibm.com/links?url=https%3A%2F%2Fibm.github.io%2Fcloud-pak).


#### Download the IBM i Modernization Engine for Lifecycle Integration installer and image inventory to the bastion host.

```bash
oc ibm-pak get $MERLIN_CASE_NAME --version $CASE_VERSION
oc ibm-pak get $CICD_CASE_NAME --version $CASE_VERSION
oc ibm-pak get $IDE_CASE_NAME --version $CASE_VERSION
oc ibm-pak get $DEVWORKSPACE_CASE_NAME --version $CASE_VERSION
```

#### Log in to the OpenShift Container Platform cluster as a cluster administrator

Following is an example command to log in to the OpenShift Container Platform cluster:

```bash
oc login <cluster host:port> --username=<cluster admin user> --password=<cluster admin password>
```

### Complete these steps to mirror the images and configure the cluster

> Note: Do not use the tilde within double quotation marks in any command. For example, do not use args "--registry <registry> --user <registry userid> --pass <registry password> --inputDir ~/offline". The tilde does not expand and the commands might fail.

* Store authentication credentials for all source Docker registries.

* The IBM i Modernization Engine for Lifecycle Integration installer is stored in a public registry and does not require authentication. However, most of the components, require one or more authenticated registries. The following registries require authentication: cp.icr.io, registry.redhat.io
* Run the following command to configure authentication credentials for the registry

```bash
export REGISTRY_AUTH_FILE=~/.ibm-pak/auth.json
export TARGET_REGISTRY=<the_registry_hostname>:<the_registry_host_port>
export TARGET_REGISTRY_USERNAME=<registry_user_name>
export TARGET_REGISTRY_PASSWORD=<registry_password>
docker login $TARGET_REGISTRY -u $TARGET_REGISTRY_USERNAME -p $TARGET_REGISTRY_PASSWORD
docker login cp.icr.io -u cp -p <the-entitlement-key>
docker login registry.redhat.io -u <redhat-user> -p <redhat-token>
```

The command stores and caches the registry credentials in a file on the file system in the $REGISTRY_AUTH_FILE location.

#### Configure a global image pull secret and ImageContentSourcePolicy.

Follow the instructions to [Create the entitlement key secret](./guides/platform/Install_MERLIN_Online?id=create-the-entitlement-key-secret).

The documented steps in the link enable your cluster to have proper authentication credentials in place to pull images from your local docker registry as specified in the image-content-source-policy.yaml that is applied to your cluster in the next step.

Run the following command to generate the image-mapping.txt file:
```
oc ibm-pak generate mirror-manifests $MERLIN_CASE_NAME $TARGET_REGISTRY --version $CASE_VERSION
oc ibm-pak generate mirror-manifests $CICD_CASE_NAME $TARGET_REGISTRY --version $CASE_VERSION
oc ibm-pak generate mirror-manifests $IDE_CASE_NAME $TARGET_REGISTRY --version $CASE_VERSION
oc ibm-pak generate mirror-manifests $DEVWORKSPACE_CASE_NAME $TARGET_REGISTRY --version $CASE_VERSION
```

Run the following command to create ImageContentSourcePolicy:
```
oc apply -f  ~/.ibm-pak/data/mirror/$MERLIN_CASE_NAME/$CASE_VERSION/image-content-source-policy.yaml
```

#### Verify that the ImageContentSourcePolicy resource is created.
```
oc get imageContentSourcePolicy
```

Optional: If an insecure registry is being used, the local registry must be added to the cluster insecureRegistries list.
```
oc patch image.config.openshift.io/cluster --type=merge -p '{"spec":{"registrySources":{"insecureRegistries":["'${TARGET_REGISTRY}'"]}}}'
```

Verify the cluster node status.
```
oc get nodes
```

After the imageContentsourcePolicy and global image pull secret are applied, wait until all the nodes show a Ready status.

#### Mirror the images to the local registry.
```
oc image mirror \
  -f ~/.ibm-pak/data/mirror/$MERLIN_CASE_NAME/$CASE_VERSION/images-mapping.txt \
  --filter-by-os '.*'  \
  -a $REGISTRY_AUTH_FILE \
  --insecure  \
  --skip-multiple-scopes \
  --max-per-registry=1 \
  --continue-on-error=true
```
```
oc image mirror \
  -f ~/.ibm-pak/data/mirror/$CICD_CASE_NAME/$CASE_VERSION/images-mapping.txt \
  --filter-by-os '.*'  \
  -a $REGISTRY_AUTH_FILE \
  --insecure  \
  --skip-multiple-scopes \
  --max-per-registry=1 \
  --continue-on-error=true
```
```
oc image mirror \
  -f ~/.ibm-pak/data/mirror/$IDE_CASE_NAME/$CASE_VERSION/images-mapping.txt \
  --filter-by-os '.*'  \
  -a $REGISTRY_AUTH_FILE \
  --insecure  \
  --skip-multiple-scopes \
  --max-per-registry=1 \
  --continue-on-error=true
```
```
oc image mirror \
  -f ~/.ibm-pak/data/mirror/$DEVWORKSPACE_CASE_NAME/$CASE_VERSION/images-mapping.txt \
  --filter-by-os '.*'  \
  -a $REGISTRY_AUTH_FILE \
  --insecure  \
  --skip-multiple-scopes \
  --max-per-registry=1 \
  --continue-on-error=true
```

### Create the IBM i Modernization Engine for Lifecycle Integration catalog source

#### Create a catalog source for IBM i Modernization Engine for Lifecycle Integration
```
oc apply -f ~/.ibm-pak/data/mirror/$MERLIN_CASE_NAME/$CASE_VERSION/catalog-sources.yaml
oc apply -f ~/.ibm-pak/data/mirror/$CICD_CASE_NAME/$CASE_VERSION/catalog-sources.yaml
oc apply -f ~/.ibm-pak/data/mirror/$IDE_CASE_NAME/$CASE_VERSION/catalog-sources.yaml
oc apply -f ~/.ibm-pak/data/mirror/$DEVWORKSPACE_CASE_NAME/$CASE_VERSION/catalog-sources.yaml
```

Verify that the catalog sources for the IBM i Modernization Engine for Lifecycle Integration installer are created.
```
oc get pods -n openshift-marketplace
oc get catalogsource -n openshift-marketplace
```

### Install IBM i Modernization Engine for Lifecycle Integration

After the catalog source are created, follow the [instructions](./guides/platform/Install_MERLIN_Online?id=install-the-operator) to install Merlin operator and deploy Merlin instance using OpenShift web console.