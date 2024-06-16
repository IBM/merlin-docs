# IBM i Developer Workspaces operator CRDs

## <span translate="no">CheCluster</span>/v1

<span translate="no">CheCluster</span> is the schema for the <span translate="no">checlusters</span> API.

### spec

**Description:** Defines the desired state of CheCluster.

**Type:**  object

| Property            | Type        | Description                       |
|---------------------|-------------|-----------------------------------|
|spec                 |object       |Defines the desired state of CheCluster|
|spec.version         |string       |Version of the csv deploying       |
|spec.license         |object       |License - license spec determines whether a license has been accepted|
|spec.license.accept  |boolean      |License acceptance switch          |
|spec.auth            |object       |Configuration settings related to the Authentication used by the Che installation|
|spec.database        |object       |Configuration settings related to the database used by the Che installation|
|spec.devWorkspace    |object       |Dev Workspace operator configuration|
|spec.imagePuller     |object       |Kubernetes Image Puller configuration|
|spec.k8s             |object       |Configuration settings specific to Che installations made on upstream Kubernetes|
|spec.metrics         |object       |Configuration settings related to the metrics collection used by the Che installation|
|spec.server          |object       |General configuration settings related to the Che server and the plugin and devfile registries|
|spec.storage         |object       |Configuration settings related to the persistent storage used by the Che installation|

### status

**Description:** Defines the observed state of CheCluster.

**Type:**  object

| Property            | Type        | Description                       |
|---------------------|-------------|-----------------------------------|
|status.cheClusterRunning|string|Status of a Che installation. Can be `Available`, `Unavailable`, or `Available, Rolling Update in Progress`|
|status.cheURL|string|Public URL to the Che server|
|status.cheVersion|string|Current installed Che version|
|status.dbProvisioned|boolean|Indicates that a PostgreSQL instance has been correctly provisioned or not|
|status.devfileRegistryURL|string|Public URL to the devfile registry|
|status.devworkspaceStatus|object|The status of the Devworkspace subsyste|
|status.gitHubOAuthProvisioned|boolean|Indicates whether an Identity Provider instance, Keycloak or RH-SSO, has been configured to integrate with the GitHub OAuth|
|status.helpLink|string|A URL that points to some URL where to find help related to the current Operator status|
|status.keycloakProvisioned|boolean|Indicates whether an Identity Provider instance, Keycloak or RH-SSO, has been provisioned with realm, client and user|
|status.keycloakURL|string|Public URL to the Identity Provider server, Keycloak or RH-SSO|
|status.message|string|A human readable message indicating details about why the Pod is in this condition|
|status.openShiftOAuthUserCredentialsSecret|string|OpenShift OAuth secret in `openshift-config` namespace that contains user credentials for HTPasswd identity provider|
|status.openShiftoAuthProvisioned|boolean|Indicates whether an Identity Provider instance, Keycloak or RH-SSO, has been configured to integrate with the OpenShift OAuth|
|status.pluginRegistryURL|string|Public URL to the plugin registry|
|status.reason|string|A brief CamelCase message indicating details about why the Pod is in this state|









## <span translate="no">CheCluster</span>/v2

<span translate="no">CheCluster</span> is the schema for the <span translate="no">checlusters</span> API.

### spec

**Description:** Defines the desired state of CheCluster.

**Type:**  object

| Property            | Type        | Description                       |
|---------------------|-------------|-----------------------------------|
|spec                 |object       |Defines the desired state of CheCluster|
|spec.components|object|Che components configuration.|
|spec.containerRegistry|object|Configuration of an alternative registry that stores Che images.|
|spec.devEnvironments|object|Development environment default configuration options.|
|spec.gitServices|object|A configuration that allows users to work with remote Git repositories.|
|spec.networking|object|Networking, Che authentication, and TLS configuration.|

### status

**Description:** Defines the observed state of CheCluster.

**Type:**  object

| Property            | Type        | Description                       |
|---------------------|-------------|-----------------------------------|
|status.chePhase|string|Specifies the current phase of the Che deployment.|
|status.cheURL|string|Public URL of the Che server.|
|status.cheVersion|string|Currently installed Che version.|
|status.devfileRegistryURL|string|The public URL of the internal devfile registry.|
|status.gatewayPhase|string|Specifies the current phase of the gateway deployment.|
|status.message|string|A human readable message indicating details about why the Che deployment is in the current phase.|
|status.pluginRegistryURL|string|The public URL of the internal plug-in registry.|
|status.reason|string|A brief CamelCase message indicating details about why the Che deployment is in the current phase.|
|status.workspaceBaseDomain|string|The resolved workspace base domain. This is either the copy of the explicitly defined property of the same name in the spec or, if it is undefined in the spec and we're running on OpenShift, the automatically resolved base domain for routes.|