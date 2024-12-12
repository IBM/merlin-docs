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
    - IBM i 7.5 PTFs: SI80368, SI81035, SI86076, SI84164, SJ00293, SI86229, SJ03030      
    - IBM i 7.4 PTFs: SI80364, SI81031, SJ00267, SI86178, SJ03026

Note: Japanese CCSIDs 5026 and 290 are not supported



## Deploying IBM i Modernization Engine For Lifecycle Integration

IBM i Modernization Engine For Lifecycle Integration can be ran on OpenShift® Container Platform that runs on Power Host (ppc64le) and OpenShift Container Platform that runs on VMware (amd64). 

### Supported installation mode

OLM All Namespace Mode
