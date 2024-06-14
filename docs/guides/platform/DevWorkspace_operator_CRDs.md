# DevWorkspace operator CRDs

## DevWorkspaceOperatorConfig/v1alpha1

DevWorkspaceOperatorConfig is the schema for the <span translate="no">devworkspaceoperatorconfigs</span> API.

### config

**Description:** OperatorConfiguration defines configuration options for the DevWorkspace Operator.

**Type:**  object

| Property                          | Type        | Description                       |
|-----------------------------------|-------------|-----------------------------------|
|config                             |object       |Defines configuration options for the DevWorkspace Operator.|
|config.enableExperimentalFeatures  |boolean      |EnableExperimentalFeatures turns on in-development features of the controller. This option should generally not be enabled, as any capabilities are subject to removal without notice.|
|config.routing                     |object       |Routing defines configuration options related to DevWorkspace networking|
|config.routing.clusterHostSuffix   |string       |ClusterHostSuffix is the hostname suffix to be used for DevWorkspace endpoints. On OpenShift, the DevWorkspace Operator will attempt to determine the appropriate value automatically. Must be specified on Kubernetes.|
|config.routing.defaultRoutingClass |string       |DefaultRoutingClass specifies the routingClass to be used when a DevWorkspace specifies an empty `.spec.routingClass`. Supported routingClasses can be defined in other controllers. If not specified, the default value of "basic" is used.|
|config.routing.proxyConfig         |object       |ProxyConfig defines the proxy settings that should be used for all DevWorkspaces. These values are propagated to workspace containers as environment variables.. On OpenShift, the operator automatically reads values from the \"cluster\" proxies.config.openshift.io object and this value only needs to be set to override those defaults. Values for httpProxy and httpsProxy override the cluster configuration directly. Entries for noProxy are merged with the noProxy values in the cluster configuration. To ignore automatically read values from the cluster, set values in fields to the empty string (\"\"). Changes to the proxy configuration are detected by the DevWorkspace Operator and propagated to DevWorkspaces. However, changing the proxy configuration for the DevWorkspace Operator itself requires restarting the controller deployment.|
|config.routing.proxyConfig.httpProxy|string      |HttpProxy is the URL of the proxy for HTTP requests, in the format http://USERNAME:PASSWORD@SERVER:PORT/. To ignore automatically detected proxy settings for the cluster, set this field to an empty string ("").|
|config.routing.proxyConfig.httpsProxy|string     |HttpsProxy is the URL of the proxy for HTTPS requests, in the format http://USERNAME:PASSWORD@SERVER:PORT/. To ignore automatically detected proxy settings for the cluster, set this field to an empty string ("").|
|config.routing.proxyConfig.noProxy|string     |NoProxy is a comma-separated list of hostnames and/or CIDRs for which the proxy should not be used. Ignored when HttpProxy and HttpsProxy are unset. To ignore automatically detected proxy settings for the cluster, set this field to an empty string ("").|
|config.workspace                    |object       |Workspace defines configuration options related to how DevWorkspaces are managed|
|config.workspace.cleanupOnStop|boolean|CleanupOnStop governs how the Operator handles stopped DevWorkspaces. If set to true, additional resources associated with a DevWorkspace. The default value is false.|
|config.workspace.containerSecurityContext|object|ContainerSecurityContext overrides the default ContainerSecurityContext used for all workspace-related containers created by the DevWorkspace Operator. If set, defined values are merged into the default configuration|
|config.workspace.defaultContainerResources|object|DefaultContainerResources defines the resource requirements (memory/cpu limit/request) used for container components that do not define limits or requests.|
|config.workspace.defaultStorageSize|object|DefaultStorageSize defines an optional struct with fields to specify the sizes of Persistent Volume Claims for storage classes used by DevWorkspaces.|
|config.workspace.defaultTemplate|object|DefaultTemplate defines an optional DevWorkspace Spec Template which gets applied to the workspace if the workspace's Template Spec Components are not defined. The DefaultTemplate will overwrite the existing Template Spec, with the exception of Projects (if any are defined).|
|config.workspace.deploymentStrategy|string|DeploymentStrategy defines the deployment strategy to use to replace existing DevWorkspace pods with new ones. The available deployment strategies are "Recreate" and "RollingUpdate".|
|config.workspace.idleTimeout|string|IdleTimeout determines how long a workspace should sit idle before being automatically scaled down. Proper functionality of this configuration property requires support in the workspace being started. If not specified, the default value of "15m" is used.|
|config.workspace.ignoredUnrecoverableEvents|array|IgnoredUnrecoverableEvents defines a list of Kubernetes event names that should be ignored when deciding to fail a DevWorkspace startup. This option should be used if a transient cluster issue is triggering false-positives. Events listed here will not trigger DevWorkspace failures.|
|config.workspace.imagePullPolicy|string|ImagePullPolicy defines the imagePullPolicy used for containers in a DevWorkspace For additional information, see Kubernetes documentation for imagePullPolicy. If not specified, the default value of "Always" is used.|
|config.workspace.persistUserHome|object|PersistUserHome defines configuration options for persisting the `/home/user/` directory in workspaces.|
|config.workspace.podSecurityContext|object|PodSecurityContext overrides the default PodSecurityContext used for all workspace-related pods created by the DevWorkspace Operator. If set, defined values are merged into the default configuration|
|config.workspace.progressTimeout|string|ProgressTimeout determines the maximum duration a DevWorkspace can be in a "Starting" or "Failing" phase without progressing before it is automatically failed. Duration should be specified in a format parseable by Go's time package.|
|config.workspace.projectClone|object|ProjectCloneConfig defines configuration related to the project clone init container that is used to clone git projects into the DevWorkspace.|
|config.workspace.pvcName|string|PVCName defines the name used for the persistent volume claim created to support workspace storage when the 'common' storage class is used. If not specified, the default value of `claim-devworkspace` is used.|
|config.workspace.schedulerName|string|SchedulerName is the name of the pod scheduler for DevWorkspace pods. If not specified, the pod scheduler is set to the default scheduler on the cluster.|
|config.workspace.serviceAccount|object|ServiceAccount defines configuration options for the ServiceAccount used for DevWorkspaces.|
|config.workspace.storageClassName|string|StorageClassName defines an optional storageClass to use for persistent volume claims created to support DevWorkspaces|

## DevWorkspaceRouting/v1alpha1

DevWorkspaceRouting is the schema for the <span translate="no">devworkspaceoperatorconfigs</span> API.

### spec

**Description:** DevWorkspaceRoutingSpec defines the desired state of DevWorkspaceRouting.

**Type:**  object

| Property                          | Type        | Description                       |
|-----------------------------------|-------------|-----------------------------------|
|spec                             |object       |Defines  the desired state of DevWorkspaceRouting.|
|spec.devworkspaceId|string|Id for the DevWorkspace being routed|
|spec.endpoints|object|Machines to endpoints map|
|spec.podSelector|object|Selector that should be used by created services to point to the devworkspace Pod|
|spec.routingClass|string|Class of the routing: this drives which DevWorkspaceRouting controller will manage this routing|

### status

**Description:** Defines the observed state of DevWorkspaceRouting.

| Property            | Type        | Description                       |
|---------------------|-------------|-----------------------------------|
|status               |object       |Defines the observed state of DevWorkspaceRouting.|
|status.exposedEndpoints|object|Machine name to exposed endpoint map|
|status.message|string|Message is a user-readable message explaining the current phase (e.g. reason for failure)|
|status.phase|string|Routing reconcile phase|
|status.podAdditions|object|Additions to main devworkspace deployment|

## DevWorkspace/v1alpha1

DevWorkspace is the schema for the <span translate="no">devworkspaces</span> API.

### spec

**Description:** DevWorkspaceSpec defines the desired state of DevWorkspace.

**Type:**  object

| Property                          | Type        | Description                       |
|-----------------------------------|-------------|-----------------------------------|
|spec                             |object       |Defines the desired state of DevWorkspace.|
|spec.routingClass|string|(No Description) |
|spec.started|boolean|(No Description) |
|spec.template|object|Structure of the workspace. This is also the specification of a workspace template.|

### status

**Description:** Defines the observed state of DevWorkspace.

| Property            | Type        | Description                       |
|---------------------|-------------|-----------------------------------|
|status               |object       |Defines the observed state of DevWorkspace.|
|status.conditions|array|Conditions represent the latest available observations of an object's state|
|status.message|string|Message is a short user-readable message giving additional information about an object's state|
|status.phase|string|null|
|status.ideUrl|string|URL at which the Workspace Editor can be joined|
|status.workspaceId|string|Id of the workspace|

## DevWorkspaceTemplate/v1alpha2

DevWorkspaceTemplate is the schema for the <span translate="no">devworkspacetemplates</span> API.

### spec

**Description:** DevWorkspaceTemplateSpec defines the desired state of DevWorkspaceTemplate.

**Type:**  object

| Property                          | Type        | Description                       |
|-----------------------------------|-------------|-----------------------------------|
|spec                             |object       |Defines the desired state of DevWorkspaceTemplate.|
|spec.attributes|object|Map of implementation-dependant free-form YAML attributes.|
|spec.commands|array|Predefined, ready-to-use, devworkspace-related commands|
|spec.components|array|List of the devworkspace components, such as editor and plugins, user-provided containers, or other types of components|
|spec.dependentProjects|array|Additional projects related to the main project in the devfile, contianing names and sources locations|
|spec.events|object|Bindings of commands to events. Each command is referred-to by its name.|
|spec.parent|object|Parent devworkspace template|
|spec.projects|array|Projects worked on in the devworkspace, containing names and sources locations|
|spec.starterProjects|array|StarterProjects is a project that can be used as a starting point when bootstrapping new projects|
|spec.variables|object|Map of key-value variables used for string replacement in the devfile. Values can be referenced via {{variable-key}} to replace the corresponding value in string fields in the devfile.|