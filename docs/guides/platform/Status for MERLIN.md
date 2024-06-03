# Merlin Status Definition

<PageDescription>

The `IBMiMerlin` is a Custom Resource Definition used to deploy and manage a Merlin instance.

</PageDescription>

<AnchorLinks>
  <AnchorLink>Conditions</AnchorLink>
  <AnchorLink>Custom Images</AnchorLink>
  <AnchorLink>Nodes</AnchorLink>
  <AnchorLink>Phase</AnchorLink>
  <AnchorLink>Versions</AnchorLink>
</AnchorLinks>

## Example

```
$ oc get merlins
NAME     IMAGE                                      EXPOSED   RECONCILED   AGE
merlin   cp.stg.icr.io/cp/ibmi-amp/amp-gui:master   true      True         6d16h

$ oc describe merlins/merlin
...
Status:
  Application URL:  https://merlin-changle-pantest.apps.amp-fb35.nip.io/AMP
  Conditions:
    Last Transition Time:  2021-12-29T09:17:41Z
    Last Update Time:      2022-01-05T01:23:44Z
    Status:                True
    Type:                  DependenciesSatisfied
    Last Transition Time:  2021-12-29T09:19:07Z
    Last Update Time:      2022-01-05T01:25:10Z
    Status:                True
    Type:                  Reconciled
```

## Application URL

The `Application URL` status property is a string flag that stands for the URL of Merlin install.

## Conditions

The `DependenciesSatisfied` condition will be set `True` when the CustomResourceDefinition merlins.merlin.ibm.com is ready.

The `Reconciled` condition, for example, will be set `True` when all of the Pods are `Ready`.
