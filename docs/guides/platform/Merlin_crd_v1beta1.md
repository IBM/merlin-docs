# Merlin/v1beta1

Merlin is the schema for the <span translate="no">merlins</span> API.

## spec

**Description:** Defines the desired state of Merlin.

**Type:**  object

| Property            | Type        | Description                       |
|---------------------|-------------|-----------------------------------|
|spec                 |object       |Defines the desired state of Merlin|
|spec.version         |string       |Version of the csv deploying       |
|spec.license         |object       |License - license spec determines whether a license has been accepted|
|spec.license.accept  |boolean      |License acceptance switch          |
|spec.storageClassName|string       |Name of the StorageClass required by the Merlin PersistentVolumeClaim|
|spec.configurationProfileName|string|Name of the Configuration Profile|
|spec.pullPolicy|string|Policy for pulling container images. Defaults to Always|
|spec.pullSecret|string|Name of the Pull Secret|

## status

**Description:** Defines the observed state of Merlin.

**Type:**  object

| Property            | Type        | Description                       |
|---------------------|-------------|-----------------------------------|
|status               |object       |Defines the observed state of Merlin|
|status.conditions    |array        |(No Description)      |
|status.applicationURL|string       |Public URL to the Merlin platform|
|status.keycloakURL   |string       |Public URL to the Identity Provider server|
|status.opVersion     |string       |The current version of Merlin platform|