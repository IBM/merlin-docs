# <span translate="no">CicdComponent</span>/v1beta1

<span translate="no">CicdComponent</span> is the schema for the <span translate="no">cicdcomponents</span> API.

## spec

**Description:** Defines the desired state of CicdComponent.

**Type:**  object

| Property            | Type        | Description                       |
|---------------------|-------------|-----------------------------------|
|spec                 |object       |Defines the desired state of CicdComponent|
|spec.version         |string       |Version of the csv deploying       |
|spec.license         |object       |License - license spec determines whether a license has been accepted|
|spec.license.accept  |boolean      |License acceptance switch          |
|spec.ampSupport|string|Describe the properties that IBM i CICD supports|
|spec.applicationImage|string|Application image to be installed|
|spec.applicationName|string|The name of the application this resource is part of. If not specified, it defaults to the name of the CR|
|spec.expose|boolean|A boolean that toggles the external exposure of this deployment via a Route or a Knative Route resource|
|spec.serviceAccountName|string|The name of the OpenShift service account to be used during deployment|
|spec.pullPolicy|string|Policy for pulling container images. Defaults to IfNotPresent|
|spec.pullSecret|string|Secret for pulling container images|
|spec.storageClassName|string|Name of the StorageClass required by the IBM i PersistentVolumeClaim|
|spec.configurationProfileName|string|Name of the Configuration Profile|

## status

**Description:** Defines the observed state of CicdComponent.

**Type:**  object

| Property            | Type        | Description                       |
|---------------------|-------------|-----------------------------------|
|status               |object       |Defines the observed state of CicdComponent|
|status.conditions    |array        |(No Description)      |
|status.applicationURL|string       |Public URL to the CicdComponent platform|