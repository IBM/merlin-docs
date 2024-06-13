# Merlin Requirements

IBM i Modernization Engine for Lifecycle Integration can be [installed from either OpenShift web console](docs/guides/openshift/merlininstall.md) or CLI (`oc` command).

## Supported product versions

See [Supported product versions](./guides/platform/Install_IBM_i_Modernization_Engine_for_Lifecycle_Integration.md).

## Required OpenShift resources

| Name                 | CPU Request | CPU Limit | Memory Request | Memory Limit | Note                              |
|----------------------|-------------|-----------|----------------|--------------|-----------------------------------|
| Merlin               | 2.5         | 5         | 7Gi            | 15Gi         |                                   |
| IBM i Developer Tool | 1.3         | 6.6       | 1.2Gi          | 9Gi          | The resource is per each instance |
| IBM i CI/CD          | 0.5         | 1         | 1Gi            | 2Gi          | The resource is per each instance |
| Developer workspace        | 0.08        | 1.4       | 320Mi          | 3.25Gi       | The resource is per user          |

> **Meaning of a instance for IBM i Developer and IBM i CI/CD tools:** If admin installs one of these two tools in a OpenShift project, that means one instance.\
> **Unit of CPU:** CPU is measured in units called millicores. If a node has 2 cores, the node’s CPU capacity would be represented as 2000m.\
> **What is the difference between Request and Limit?** Request can be compared to the minimum amount required, where as Limit indicates the most that will be used.

You can read more about OpenShift Quotas and Limit Ranges on the [official documentation](https://docs.openshift.com/container-platform/3.11/dev_guide/compute_resources.html).

