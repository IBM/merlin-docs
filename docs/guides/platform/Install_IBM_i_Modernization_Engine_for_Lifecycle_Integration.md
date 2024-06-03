# Install IBM i Modernization Engine for Lifecycle Integration


IBM i Modernization Engine for Lifecycle Integration can be installed from either OpenShift web console, or oc command line. 

* [Install IBM i Modernization Engine for Lifecycle Integration online](./guides/platform/Install_MERLIN_Online.md)

* [Install IBM i Modernization Engine for Lifecycle Integration in airgap environment](./guides/platform/Install_MERLIN_In_AirGap.md)

## Supported product versions

OpenShift Container Platform: 4.14

IBM i: 7.4, 7.5
- Minimum HTTP Group PTFs:
    - IBM i 7.5 SF99952: IBM HTTP Server for i Group level 05
    - IBM i 7.4 SF99662: IBM HTTP Server for i Group level 25
- 5770WDS:
    - IBM i 7.5 PTFs: SI79659, SI81047, SI81049, SI83058
    - IBM i 7.4 PTFs: SI76101, SI81006, SI81023, SI83048
- 5770SS1:
    - IBM i 7.5 PTFs: SI80368, SI81035, SI86076, SI84164, SJ00293, SI86229      
    - IBM i 7.4 PTFs: SI80364, SI81031, SJ00267, SI86178

Note: Japanese CCSIDs 5026 and 290 are not supported


## The resource needed for the setup
| ComponentName              | CPU Request        | CPULimit | Memory Request | Memory Limit | Note|
|----------------------------|--------------------|----------|-----------------|--------------|-----|
|Merlin 	                 |2.5                 |5	     |7Gi             |15Gi         |     |
|IBM i Developer Tool        |0.5                 |2.7       |1.5             |3Gi	       | The resource is per each instance |
|IBM i CI/CD                 |	500m              | 1        | 1Gi            | 2Gi              | The resource is per each instance|



## Deploying IBM i Modernization Engine For Lifecycle Integration

IBM i Modernization Engine For Lifecycle Integration can be ran on OpenShift® Container Platform that runs on Power Host (ppc64le) and OpenShift Container Platform that runs on VMware (amd64). 

### Supported installation mode

OLM Own Namespace Mode

OLM All Namespace Mode

### Installation Mode Coexistence

Operators must not be installed in a "mixed mode" with some in Own and some in All Namespace mode
 
* Dangers to Installing an Operator more than once 

One of the main inhibitors to installing an Operator more than once to a cluster is the lifecycle of the CustomResourceDefinition (CRD) API. The CRD is an API interface that the Operator watches on the cluster.  New and emerging requirements will cause this API to evolve over time.  When this API evolves, it can lead to the need to convert one version of the API to another, or to breaking API changes.

In Kubernetes, this is handled via a conversion webhook, which is a cluster scoped resource. This means that if more than one version of an Operator is installed on a cluster and they have different conversion webhooks, there is a leader election problem.  The Kubernetes control plane does not know which conversion webhook version to pick.  This can lead to many problems in an installation/upgrade for an Operator and the resources it manages.  Operators are designed to be extensions of the control plane and not multi-tenant on the cluster.  The applications they manage can be multi-tenant on the cluster, but the Operator cannot. 

The Custom Resource API Versioning page goes into more details on this and this page will call out some high level topics to be aware of.
