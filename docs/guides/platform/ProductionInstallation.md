# Installing IBM i Modernization Engine for Lifecycle Integration (Production installation)

Prepare and install an online or offline production deployment of IBM i Modernization Engine For Lifecycle Integration with the following steps.

## Prepare your environment for installing IBM i Modernization Engine For Lifecycle Integration.

    Depending on whether your OpenShift cluster is connected to the internet, follow the instructions to prepare for an online or offline (airgap) deployment. If your OpenShift cluster is connected to the internet, then use the Preparing to install (online) topic, otherwise you must use the Preparing to install (offline) topic.

    Select one of:
        Preparing to install (online)
        Preparing to install (offline)

    Install IBM i Modernization Engine For Lifecycle Integration.

    Now that you have prepared your environment for installation, use the following instructions to install an online or offline deployment of IBM i Modernization Engine For Lifecycle Integration.
        Production installation (online, offline)

## The resource needed for the setup
| ComponentName              | CPU Request        | CPULimit | Memory Request | Memory Limit | Note|
|----------------------------|--------------------|----------|-------------------------------|-----|
|Merlin 	                 |2.5                 |5	     |7Gi             |15Gi         |     |
|IBM i Developer Tool        |0.5                 |2.7       |1.5             |3Gi	       | The resource is per each instance |
|IBM i CI/CD                 |	500m              | 1        | 1Gi            | 2Gi              | The resource is per each instance|

## Consumed VPC per each workspace
| Devfile Installed              | VPC        | 
|-----------------------|---------------|
|IBM i vsix + ARCAD vsix | 3 |
|Python | 4 | 
|PHP | 4.5|

    

