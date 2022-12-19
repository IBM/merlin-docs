This topic mainly focuses on Merlin for OpenShift Local, more about OpenShift Local, refer to 

* https://developers.redhat.com/products/openshift-local/getting-started
* https://github.com/crc-org/crc
* https://crc.dev/crc/

## What is the guide for?

This guide exists for users who potentially have hardware available for OpenShift Local testing. This guide may let you setup an instance of Merlin inside of OpenShift Local for testing purposes only. We do not recommend using OpenShift Local as a production development environment.

## System requirements

Red Hat OpenShift Local at present is only supported on AMD64 and Intel 64 processor architectures. It has the following minimum hardware requirements:

- 4 physical CPU cores
- 9 GB of free memory
- 35 GB of storage space

And the following operating system requirements:

- Microsoft Windows 10 Fall Creators Update (version 1709) or later
- macOS 11 Big Sur or later
- The latest two Red Hat Enterprise Linux/CentOS 7, 8 and 9 minor releases and on the latest two stable Fedora releases.

**Note:**

The OpenShift Local requires these (above) minimum hardware resources to run the smallest OpenShift Container Platform. Some workloads may require more resources. We recommend the following resources for running OpenShift Local:

- 8 physical CPU cores
- 64 GB of free memory
- 256 GB of storage space

## Install Red Hat OpenShift Local

1. Download the latest release of Red Hat OpenShift Local for your platform from https://console.redhat.com/openshift/create/local
2. Extract the contents of the archive and copy the ```crc``` binary to ```/usr/bin```
3. Install and configure (for example)

    ```sh
    $ crc setup
    $ crc config set memory 61440 (Size in MiB)
    $ crc config set cpus 8
    $ crc config set disk-size 256 (Size in GiB)
    $ crc config set pull-secret-file /path/yourSecretFile
    ```

4. Configure Red Hat OpenShift Local remote access

    ```sh
    # Start the cluster:
    $ crc start
    # Ensure that the cluster remains running during this procedure.

    # Install the haproxy package and other utilities:
    $ sudo dnf install haproxy /usr/sbin/semanage
    
    # Modify the firewall to allow communication with the cluster:
    $ sudo systemctl enable --now firewalld
    $ sudo firewall-cmd --add-service=http --permanent
    $ sudo firewall-cmd --add-service=https --permanent
    $ sudo firewall-cmd --add-service=kube-apiserver --permanent
    $ sudo firewall-cmd --reload
    
    # Allow HAProxy to listen on TCP port 6443 to serve kube-apiserver on that port
    $ sudo semanage port -a -t http_port_t -p tcp 6443

    # Create a backup of the default haproxy configuration:
    $ sudo cp /etc/haproxy/haproxy.cfg{,.bak}

    # Configure haproxy for use with the cluster:
    $ export CRC_IP=$(crc ip)
    $ sudo tee /etc/haproxy/haproxy.cfg &>/dev/null <<EOF
    global
        log /dev/log local0

    defaults
        balance roundrobin
        log global
        maxconn 100
        mode tcp
        timeout connect 5s
        timeout client 500s
        timeout server 500s

    listen apps
        bind 0.0.0.0:80
        server crcvm $CRC_IP:80 check

    listen apps_ssl
        bind 0.0.0.0:443
        server crcvm $CRC_IP:443 check

    listen api
        bind 0.0.0.0:6443
        server crcvm $CRC_IP:6443 check
    EOF
    
    # Start the haproxy service:
    $ sudo systemctl start haproxy
    ```

## Configure OpenShift Local for Merlin

1. Configure the mirror of staging and product repository 

    ```sh
    $ cat <<EOF >mirror-config.yaml
    apiVersion: operator.openshift.io/v1alpha1
    kind: ImageContentSourcePolicy
    metadata:
      name: mirror-config
    spec:
      repositoryDigestMirrors:
        - mirrors:
            - cp.stg.icr.io/cp
          source: cp.icr.io/cp
        - mirrors:
            - cp.stg.icr.io/cp
          source: icr.io/cpopen
    EOF
    
    $ oc apply -f mirror-config.yaml
   ```

2. Install Merlin operator and deploy Merlin instance
   * See our [operator installation guide](/guides/openshift/merlininstall?id=installing-merlin-operator).

## Access OpenShift Local and Merlin

1. Adding entries to `/etc/hosts` on your local laptop or desktop computer to access the OpenShift Local (replace the `192.168.96.16` to your OpenShift Local IP address).

    ```sh
    # OpenShift routes
    192.168.96.16 api.crc.testing canary-openshift-ingress-canary.apps-crc.testing console-openshift-console.apps-crc.testing default-route-openshift-image-registry.apps-crc.testing downloads-openshift-console.apps-crc.testing oauth-openshift.apps-crc.testing
    ```

2. Adding entries to `/etc/hosts` on your local laptop or desktop computer to access the Merlin platform on OpenShift Local(replace the `192.168.96.16` to your OpenShift Local IP address, and replace ```<merlin-namespace>``` to the merlin namespace that you installed).

    ```sh
    # Merlin platform routes
    192.168.96.16 keycloak-<merlin-namespace>.apps-crc.testing
    192.168.96.16 merlin-<merlin-namespace>.apps-crc.testing
    192.168.96.16 rest-<merlin-namespace>.apps-crc.testing
    ```

3. Adding entries to `/etc/hosts` on your local laptop or desktop computer to access Merlin CICD tool on the OpenShift Local (replace the `192.168.96.16` to your OpenShift Local IP address, and replace `<merlin-cicd-namespace>` to the merlin CICD namespace that you installed).

    ```sh
    # Merlin CICD routes
    192.168.96.16 ibmi-cicd-<merlin-cicd-namespace>.apps-crc.testing
    192.168.96.16 jenkins-<merlin-cicd-namespace>.apps-crc.testing
    ```

4. Adding entries to `/etc/hosts` on your local laptop or desktop computer to access Merlin IDE tool on the OpenShift Local (replace the `192.168.96.16` to your OpenShift Local IP address, , and replace `<merlin-ide-namespace>` to the merlin IDE namespace that you installed).

    ```sh
    # Merlin IDE server routes
    192.168.96.16 plugin-registry-<merlin-ide-namespace>.apps-crc.testing
    192.168.96.16 devfile-registry-<merlin-ide-namespace>.apps-crc.testing 
    192.168.96.16 codeready-<merlin-ide-namespace>.apps-crc.testing
    
    # Merlin IDE workspace routes
    192.168.96.16 route0p2o546n-admin-workspace.apps-crc.testing
    192.168.96.16 route2w9tadum-admin-workspace.apps-crc.testing
    192.168.96.16 route6duxnjl3-admin-workspace.apps-crc.testing
    192.168.96.16 route6m0qzgow-admin-workspace.apps-crc.testing
    192.168.96.16 route9n18t6vg-admin-workspace.apps-crc.testing
    192.168.96.16 routeh77h7k8w-admin-workspace.apps-crc.testing
    192.168.96.16 routei7o34lb3-admin-workspace.apps-crc.testing
    192.168.96.16 routenvlnqkfe-admin-workspace.apps-crc.testing
    ```

    These Merlin IDE workspace routes which contain random character suffix can be retrieved by `oc get routes | awk '{print $2}'` in each user's namespace of the workspace.  

	Another way to get routes of Merlin CICD and Merlin IDE is from Merlin web console. Login Merlin -> **Tools** -> **Deployed Tools**, from the project checklist select the CICD or IDE project, right click the Tool icon, click **View Details**->**Resources**.  

## Backup and Restore OpenShift Local with Merlin and Merlin Tools

The OpenShift preset for OpenShift Local provides a regular OpenShift Container Platform installation with a notable difference: It does not have a supported upgrade path to newer OpenShift Container Platform versions. Upgrading the OpenShift Container Platform version may cause issues that are difficult to reproduce.

This section is a recommended solution for Merlin and Merlin tools backup, upgrade and restore.

### 1. As a customized OpenShift bundle

Generate a custom bundle from the running OpenShift cluster

```sh
    $ crc bundle generate
```

The output is similar to this:

```sh
    INFO Stopping the instance, this may take a few minutes...
    INFO Copying the disk image to crc_libvirt_4.10.14_amd64_1661475803
    INFO Generating sha256sum for crc.qcow2...
    INFO Compressing crc_libvirt_4.10.14_amd64_1661475803...
    INFO Bundle is generated in /home/itaas/crc_libvirt_4.10.14_amd64_1661475803.crcbundle
    INFO You need to perform 'crc delete' and 'crc start -b /home/itaas/crc_libvirt_4.10.14_amd64_1661475803.crcbundle' to use this bundle
```

After the `crc bundle generate` completed, the previous Openshift Local instance with Merlin and Merlin tools deployed have been stored into the new crc bundle.  Then you can use `crc start -b path/to/bundle` to start and restore to a new OpenShift Local instance. The new instance will contains all the previous contents.

### 2. The Merlin platform and Merlin tools data

The following steps need to be recorded for usage in the future as part of the backup and restore process.

#### Merlin platform

 1. Merlin UID
    * `oc get Merlin -n <merlin-namespace> -o json | grep uid | awk '{print $2}' | sed 's/\"//g'`
 2. Vault Secret and Token.
    * They **must** have been already written down when you did the Vault initialization.
 3. Keycloak admin password.
    * It is the value of `ADMIN_PASSWORD` in 'merlin-credential-secret', or the one you had already changed.

**Note**: It is recommended to create another admin account, other than using the default admin account.

##### Backup steps:

1. From OpenShift web console, locate to Merlin namespace, open the postgres pod terminal by <br>Workloads->Pods->select pod `postgres-0` -> Terminal.
2. In the Terminal, run the following command to dump the database to a dump file.
   * `pg_dump merlindb > /tmp/merlindb_dumpfile`
3. On your desktop or laptop, run oc commands to download the database dump file from postgres pod. 
   * `oc project <merlin-namespace>`
   * `oc rsync postgres-0:/tmp /tmp`
4. On your desktop or laptop, run oc commands to backup Merlin CA secrets. This will be used when the Merlin project is deleted and re-created. 
   * `oc get secret  merlin-ca-secret -n <merlin-namespace> -o yaml > /tmp/merlin-ca-secret.yml`  
   * `oc get secret merlin-jks-pkcs12-secret -n <merlin-namespace> -o yaml > /tmp/merlin-jks-pkcs12-secret.yaml`  

##### Restore steps:

1. Install Merlin.
   * From OpenShift web console, create the merlin namespace, the namespace name should be same as before.
2. On your desktop or laptop, run oc commands to restore the CA secrets before installing Merlin
   * `oc project <merlin-namespace>`
   * `oc apply -f /tmp/merlin-ca-secret.yml`  
   * `oc apply -f /tmp/merlin-jks-pkcs12-secret.yaml` 
3. Install Merlin operator and create a merlin instance as usual.
4. On your desktop or laptop, run oc commands to upload the database dump file to postgres pod.
   * `oc project <merlin-namespace>`
   * `oc rsync /tmp postgres-0:/tmp`

From OpenShift web console, locate the Merlin namespace, open the postgres pod terminal by <br>Workloads->Pods->select pod `postgres-0` -> Terminal.
   
* In the Terminal, run `psql postgres` to start a psql session.
* In the psql session, run the following command to create an empty merlindb database:
    * `DROP DATABASE IF EXISTS merlindb WITH (FORCE);`
    * `CREATE DATABASE merlindb;`
* Run `\q` to quit the psql sesison.
* In the Terminal, run `psql -f /tmp/merlindb_dumpfile -d merlindb` to restore the merlindb database.
   
Now, you can use admin user and password to access Merlin GUI and use the vault secret and token to unseal vault which you had already written down. 

**Change the Merlin UID information**

1. First, find the owner which match the Merlin UID which had been backed up.
   * `oc get namespace --selector owner=<previous_UID>`
2. Then, get the current Merlin UID.
   * `oc get merlin -n <merlin-namespace> -o json | grep uid | awk '{print $2}' | sed 's/\"//g'`
3. For each namespace in the output of step 4.1, run
   * `oc label namespace <namespace_in_4.1> owner=<current_UID> --overwrite`

#### Merlin CICD

##### Backup steps:
1. On your desktop or laptop, run oc commands to download the Merlin CICD data.  
   * `oc project <cicd-namespace>`
   * `oc rsync ibmi-cicd-0:/merlin/cicd/gui /tmp/cicdbackupfolder`  

##### Restore steps:
1. Install a new Merlin CICD as usual. The Merlin CICD namespace name should be same as before.
2. On your desktop or laptop, run oc commands to upload the dump file to CICD pod.
   * `oc project <cicd-namespace>`
   * `oc rsync /tmp/cicdbackupfolder/gui ibmi-cicd-0:/merlin/cicd/ `  

# Merlin IDE

## Backup steps

On your desktop or laptop, run these oc commands to backup the data of the workspace. Make sure the workspace is running before the backup.

 1. Get the pod name of the running workspace

    ```sh
    oc get pods -n <merlin-user-id>-workspace
    ```

 2. Get the container name of IDE in the running workspace

    ```sh
    oc get pods <pod-name> -n <merlin-user-id>-workspace -o json | grep org.eclipse.che.container.display-name/theia
    ```

 3. Backup the Git configurations, SSH config file and private keys

    From OpenShift web console, select the namespace where the Merlin IDE is installed, open the postgres pod terminal by Workloads -> Pods -> select postgres pod -> Terminal. In the terminal, run the command below to dump the database to a dump file.  

    ```sh
    pg_dump dbche > /tmp/dbche_dumpfile
    ```

    Run the command below on the local machine to download the database dump from the remote machine.

    ```sh
    # Get postgres pod name in the Merlin IDE namespace
    oc get pods -n <merlin-ide-namespace> | grep postgres
    # Download the dump file from remote to local machine
    mkdir /tmp/dbche_dumpfile_dir
    oc rsync -n <merlin-ide-namespace> <postgres-pod-name>:/tmp/dbche_dumpfile /tmp/dbche_dumpfile_dir
    ```

 4. Backup all the user source files and IDE configurations

    ```sh
    oc rsync -n <merlin-user-id>-workspace <pod-name>:/home/theia/.theia /tmp/ -c <IDE-container-name>
    oc rsync -n <merlin-user-id>-workspace <pod-name>:/projects /tmp/ -c <IDE-container-name>
    ```

## Restore steps

 1. Install a new Merlin IDE in a namespace the same as before. Then login and start a new workspace before restoring the data.

 2. Upload the postgres database dumpfile from local to remote machine

    ```sh
    # Get postgres pod name in the Merlin IDE namespace
    oc get pods -n <merlin-ide-namespace> | grep postgres
    # Upload the dump file from local to remote machine
    oc rsync -n <merlin-ide-namespace> /tmp/dbche_dumpfile_dir/ <postgres-pod-name>:/tmp/
    ```

 3. Restore the Git configurations, SSH config file and private keys

    From OpenShift web console, select the namespace where the Merlin IDE is installed, open the postgres pod terminal by Workloads -> Pods -> select postgres pod -> Terminal.  

    In the terminal, run command `psql postgres` to start a psql session.  
    In the psql session, run the following commands to create an empty `dbche` database.

    ```sql
    DROP DATABASE IF EXISTS dbche WITH (FORCE);
    CREATE DATABASE dbche;
    ```

    Run `\q` to quit the psql session.  
    In the terminal, run the command below to restore the `dbche` database.

    ```sh
    psql -f /tmp/dbche_dumpfile -d dbche
    ```

 4. Restore all the user source files and IDE configurations.

    ```sh
    oc rsync -n <merlin-user-id>-workspace tmp/.theia/ <pod-name>:/home/theia/.theia -c <IDE-container-name>
    oc rsync -n <merlin-user-id>-workspace tmp/projects/ <pod-name>:/projects -c <IDE-container-name>
    ```

 5. Restart the workspace to let the restoration take effect.
