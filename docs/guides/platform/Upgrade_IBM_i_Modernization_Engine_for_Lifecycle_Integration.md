# Upgrade IBM i Modernization Engine for Lifecycle Integration

This document discusses topics related to IBM i Developer Tool.

## Versioning

To understand the upgrade and rollback procedures, it is important to understand how components of IBM i Developer Tool are versioned. There is a distinction between (1) the OLM operator that abstracts the installation and management of the components and (2) the operand that is the components themselves. The operator represents the IBM i Developer Tool installer. The operand is the set of IBM i Developer Tool components configured by the resource.

The operator and operand are versioned independently: but each version of the operator can install a range of the operand. The desired version of the operand is set in the spec.version field of the IBM i Developer Customer Resource resource.

## Upgrade

The versions of the operator and operand are decoupled. Upgrading IBM i Developer Tool to the latest version requires updates to both of operator and operand.

Procedure

    The cluster administrator pulls in an update to the OLM catalog of IBM provided operators, including updates to the catalog for IBM i Developer Tool.
    Based on the OLM Subscription, the IBM i Developer Tool operator is upgraded either automatically or after manual approval.
        This will roll out an update to the operator-controller-manager pod, but the IBM i Developer Customer Resource component will not be changed.
    The namespace administrator chooses an available version and updates the spec.version field on the IBM i Developer Customer Resource custom resource.
    The change to the desired version triggers reconciliation of the IBM i Developer Customer Resource resource and upgrades the IBM i Developer Tool components.

Rollback

The versions of the operator and operand are decoupled. A given version of the operator may support multiple versions of the operand. If an issue is encountered after an upgrade, the previous stable state can be restored by changing the IBM i Developer Customer Resource resource’s spec.version back to the previous value. OLM does not support reverting the version of the installed operator, but this should not be necessary to restore a stable state to the IBM i Developer Tool components.
Procedure

    The namespace administrator sets the spec.version field on the IBM i Developer Customer Resource custom resource back to its previous value.
    The change to the desired version triggers reconciliation of the IBM i Developer Tool components.