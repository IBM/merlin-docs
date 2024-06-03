# Tracking license consumption of IBM i Modernization Engine For Lifecycle Integration

IBM i Modernization Engine For Lifecycle Integration contains an integrated service for measuring the license usage at the cluster level for license evidence purposes.

## Overview

The integrated licensing solution collects and stores the license usage information which can be used for audit purposes and for tracking license consumption in cloud environments. The solution works in the background and does not require any configuration. Only one instance of the License Service is deployed per cluster regardless of the number of Cloud Paks and containerized products that you have installed on the cluster.

## Validating if License Service is deployed on the cluster

To ensure license reporting continuity for license compliance purposes make sure that License Service is successfully deployed. It is recommended to periodically verify whether it is active.

To validate whether License Service is deployed and running on the cluster, you can, for example, log in to the cluster and run the following command:
```
kubectl get pods --all-namespaces | grep ibm-licensing | grep -v operator
```

The following response is a confirmation of successful deployment:
```
      1/1     Running
```
## Archiving license usage data before decommissioning the cluster

Remember to archive the license usage evidence before you decommission the cluster where IBM i Modernization Engine For Lifecycle Integration was deployed. Retrieve the audit snapshot for the period when IBM i Modernization Engine For Lifecycle Integration was on the cluster and store it in case of audit.

For more information about the licensing solution, see License Service documentation.
