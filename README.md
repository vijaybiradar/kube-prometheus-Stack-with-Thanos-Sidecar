To install Thanos using Helm, follow these steps:

Create a Namespace for Thanos (Optional):
If you want to install Thanos in a specific namespace, create the namespace first:
```
kubectl create ns thanos
```
Install Thanos Chart:
Use Helm to install the Thanos chart. Here's the command to install Thanos with the release name thanos:
```
helm install thanos oci://registry-1.docker.io/bitnamicharts/thanos -n thanos
```
Verify Installation:
After the installation completes, you can check the status of the release using:
```
helm ls -n thanos
```
Access Thanos Components:
The installation notes provided by Helm will contain instructions on how to access Thanos Query. You can follow these instructions to access Thanos components.

Export Values (Optional):
If you want to save the configuration values used during installation, you can export them to a YAML file using the following command:
```
helm get values thanos -n thanos -a --revision 1 -o yaml > fullvalues.yaml
```
The provided output indicates that Thanos has been successfully installed. You can now proceed to access and use Thanos Query and other components as needed.
```
cloud_user@k8s-control:~$ kubectl create ns thanos
namespace/thanos created
```
```
cloud_user@k8s-control:~$ helm install thanos oci://registry-1.docker.io/bitnamicharts/thanos -n thanos
Pulled: registry-1.docker.io/bitnamicharts/thanos:13.2.2
Digest: sha256:52c25311d641eb950ed37d0b59bf10e8b99115eef974d6fe0554a225a6546a76
NAME: thanos
LAST DEPLOYED: Sat Feb 24 09:06:14 2024
NAMESPACE: thanos
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: thanos
CHART VERSION: 13.2.2
APP VERSION: 0.34.1

** Please be patient while the chart is being deployed **

Thanos chart was deployed enabling the following components:
- Thanos Query

Thanos Query can be accessed through following DNS name from within your cluster:

    thanos-query.thanos.svc.cluster.local (port 9090)

To access Thanos Query from outside the cluster execute the following commands:

1. Get the Thanos Query URL by running these commands:

    export SERVICE_PORT=$(kubectl get --namespace thanos -o jsonpath="{.spec.ports[0].port}" services thanos-query)
    kubectl port-forward --namespace thanos svc/thanos-query ${SERVICE_PORT}:${SERVICE_PORT} &
    echo "http://127.0.0.1:${SERVICE_PORT}"

2. Open a browser and access Thanos Query using the obtained URL.

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:
  - query.resources
  - queryFrontend.resources
+info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
```
```
cloud_user@k8s-control:~$ helm ls -n thanos
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
thanos  thanos          1               2024-02-24 09:06:14.217994494 +0000 UTC deployed        thanos-13.2.2   0.34.1
```
```
cloud_user@k8s-control:~$ kubectl get pod -n thanos
NAME                                     READY   STATUS    RESTARTS   AGE
thanos-query-76879f7bf4-zz8t6            1/1     Running   0          109s
thanos-query-frontend-7c7dff7c84-f982f   1/1     Running   0          109s
```
```
cloud_user@k8s-control:~$ helm get values thanos -n thanos -a --revision 1 -o yaml >fullvalues.yaml
```
```
cloud_user@k8s-control:~$ cat fullvalues.yaml
auth:
  basicAuthUsers: {}
bucketCacheConfig: ""
bucketweb:
  affinity: {}
  args: []
  automountServiceAccountToken: true
  autoscaling:
    enabled: false
    maxReplicas: ""
    minReplicas: ""
    targetCPU: ""
    targetMemory: ""
  command: []
  containerPorts:
    http: 8080
  containerSecurityContext:
    allowPrivilegeEscalation: false
    capabilities:
      drop:
      - ALL
    enabled: true
    privileged: false
    readOnlyRootFilesystem: true
    runAsNonRoot: true
    runAsUser: 1001
    seLinuxOptions: null
    seccompProfile:
      type: RuntimeDefault
  customLivenessProbe: {}
  customReadinessProbe: {}
  customStartupProbe: {}
  dnsConfig: {}
  dnsPolicy: ""
  enabled: false
  extraEnvVars: []
  extraEnvVarsCM: ""
  extraEnvVarsSecret: ""
  extraFlags: []
  extraVolumeMounts: []
  extraVolumes: []
  hostAliases: []
  ingress:
    annotations: {}
    apiVersion: ""
    enabled: false
    extraHosts: []
    extraRules: []
    extraTls: []
    hostname: thanos-bucketweb.local
    ingressClassName: ""
    path: /
    pathType: ImplementationSpecific
    secrets: []
    selfSigned: false
    tls: false
  initContainers: []
  lifecycleHooks: {}
  livenessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  logFormat: logfmt
  logLevel: info
  networkPolicy:
    allowExternal: true
    allowExternalEgress: true
    enabled: true
    extraEgress: []
    extraIngress: []
    ingressNSMatchLabels: {}
    ingressNSPodMatchLabels: {}
  nodeAffinityPreset:
    key: ""
    type: ""
    values: []
  nodeSelector: {}
  pdb:
    create: false
    maxUnavailable: ""
    minAvailable: 1
  podAffinityPreset: ""
  podAnnotations: {}
  podAntiAffinityPreset: soft
  podLabels: {}
  podSecurityContext:
    enabled: true
    fsGroup: 1001
    fsGroupChangePolicy: Always
    supplementalGroups: []
    sysctls: []
  priorityClassName: ""
  readinessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  refresh: 30m
  replicaCount: 1
  resources: {}
  resourcesPreset: none
  revisionHistoryLimit: 10
  schedulerName: ""
  service:
    annotations: {}
    clusterIP: ""
    externalTrafficPolicy: Cluster
    extraPorts: []
    labelSelectorsOverride: {}
    labels: {}
    loadBalancerIP: ""
    loadBalancerSourceRanges: []
    nodePorts:
      http: ""
    ports:
      http: 8080
    type: ClusterIP
  serviceAccount:
    annotations: {}
    automountServiceAccountToken: false
    create: true
    name: ""
  sidecars: []
  startupProbe:
    enabled: false
    failureThreshold: 15
    initialDelaySeconds: 5
    periodSeconds: 5
    successThreshold: 1
    timeoutSeconds: 1
  timeout: 5m
  tolerations: []
  topologySpreadConstraints: []
  updateStrategy:
    type: RollingUpdate
clusterDomain: cluster.local
common:
  exampleValue: common-chart
  global:
    imagePullSecrets: []
    imageRegistry: ""
    storageClass: ""
commonAnnotations: {}
commonLabels: {}
compactor:
  affinity: {}
  args: []
  automountServiceAccountToken: true
  command: []
  consistencyDelay: 30m
  containerPorts:
    http: 10902
  containerSecurityContext:
    allowPrivilegeEscalation: false
    capabilities:
      drop:
      - ALL
    enabled: true
    privileged: false
    readOnlyRootFilesystem: true
    runAsNonRoot: true
    runAsUser: 1001
    seLinuxOptions: null
    seccompProfile:
      type: RuntimeDefault
  cronJob:
    backoffLimit: ""
    concurrencyPolicy: Forbid
    enabled: false
    failedJobsHistoryLimit: ""
    schedule: 0 */6 * * *
    startingDeadlineSeconds: ""
    successfulJobsHistoryLimit: ""
    suspend: ""
    timeZone: ""
    ttlSecondsAfterFinished: ""
  customLivenessProbe: {}
  customReadinessProbe: {}
  customStartupProbe: {}
  dnsConfig: {}
  dnsPolicy: ""
  enabled: false
  extraEnvVars: []
  extraEnvVarsCM: ""
  extraEnvVarsSecret: ""
  extraFlags: []
  extraVolumeMounts: []
  extraVolumes: []
  hostAliases: []
  ingress:
    annotations: {}
    apiVersion: ""
    enabled: false
    extraHosts: []
    extraRules: []
    extraTls: []
    hostname: thanos-compactor.local
    ingressClassName: ""
    path: /
    pathType: ImplementationSpecific
    secrets: []
    selfSigned: false
    tls: false
  initContainers: []
  lifecycleHooks: {}
  livenessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  logFormat: logfmt
  logLevel: info
  networkPolicy:
    allowExternal: true
    allowExternalEgress: true
    enabled: true
    extraEgress: []
    extraIngress: []
    ingressNSMatchLabels: {}
    ingressNSPodMatchLabels: {}
  nodeAffinityPreset:
    key: ""
    type: ""
    values: []
  nodeSelector: {}
  persistence:
    accessModes:
    - ReadWriteOnce
    annotations: {}
    defaultEmptyDir: true
    enabled: true
    ephemeral: false
    existingClaim: ""
    labels: {}
    size: 8Gi
    storageClass: ""
  podAffinityPreset: ""
  podAnnotations: {}
  podAntiAffinityPreset: soft
  podLabels: {}
  podSecurityContext:
    enabled: true
    fsGroup: 1001
    fsGroupChangePolicy: Always
    supplementalGroups: []
    sysctls: []
  priorityClassName: ""
  readinessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  resources: {}
  resourcesPreset: none
  restartPolicy: ""
  retentionResolution1h: 10y
  retentionResolution5m: 30d
  retentionResolutionRaw: 30d
  revisionHistoryLimit: 10
  schedulerName: ""
  service:
    annotations: {}
    clusterIP: ""
    externalTrafficPolicy: Cluster
    extraPorts: []
    labelSelectorsOverride: {}
    labels: {}
    loadBalancerIP: ""
    loadBalancerSourceRanges: []
    nodePorts:
      http: ""
    ports:
      http: 9090
    type: ClusterIP
  serviceAccount:
    annotations: {}
    automountServiceAccountToken: false
    create: true
    name: ""
  sidecars: []
  startupProbe:
    enabled: false
    failureThreshold: 15
    initialDelaySeconds: 5
    periodSeconds: 5
    successThreshold: 1
    timeoutSeconds: 1
  tolerations: []
  topologySpreadConstraints: []
  updateStrategy:
    type: Recreate
existingHttpConfigSecret: ""
existingObjstoreSecret: ""
existingObjstoreSecretItems: []
extraDeploy: []
fullnameOverride: ""
global:
  imagePullSecrets: []
  imageRegistry: ""
  storageClass: ""
httpConfig: ""
https:
  autoGenerated: false
  ca: ""
  caFilename: ca.crt
  cert: ""
  certFilename: tls.crt
  clientAuthType: ""
  enabled: false
  existingSecret: ""
  key: ""
  keyFilename: tls.key
image:
  digest: ""
  pullPolicy: IfNotPresent
  pullSecrets: []
  registry: docker.io
  repository: bitnami/thanos
  tag: 0.34.1-debian-12-r0
indexCacheConfig: ""
kubeVersion: ""
metrics:
  enabled: false
  prometheusRule:
    additionalLabels: {}
    default:
      create: false
      disabled: {}
      sidecarJobRegex: .*thanos-sidecar.*
    enabled: false
    groups: []
    namespace: ""
    runbookUrl: https://github.com/thanos-io/thanos/tree/main/mixin/runbook.md#alert-name-
  serviceMonitor:
    enabled: false
    extraParameters: {}
    interval: ""
    jobLabel: ""
    labels: {}
    metricRelabelings: []
    namespace: ""
    relabelings: []
    scrapeTimeout: ""
    selector: {}
minio:
  affinity: {}
  apiIngress:
    annotations: {}
    apiVersion: ""
    enabled: false
    extraHosts: []
    extraPaths: []
    extraRules: []
    extraTls: []
    hostname: minio.local
    ingressClassName: ""
    path: /
    pathType: ImplementationSpecific
    secrets: []
    selfSigned: false
    servicePort: minio-api
    tls: false
  args: []
  auth:
    existingSecret: ""
    forceNewKeys: false
    forcePassword: false
    rootPassword: ""
    rootUser: admin
    useCredentialsFiles: false
  clientImage:
    digest: ""
    registry: docker.io
    repository: bitnami/minio-client
    tag: 2023.12.29-debian-11-r1
  clusterDomain: cluster.local
  command: []
  common:
    exampleValue: common-chart
    global:
      imagePullSecrets: []
      imageRegistry: ""
      storageClass: ""
  commonAnnotations: {}
  commonLabels: {}
  containerPorts:
    api: 9000
    console: 9001
  containerSecurityContext:
    allowPrivilegeEscalation: false
    capabilities:
      drop:
      - ALL
    enabled: true
    privileged: false
    readOnlyRootFilesystem: false
    runAsNonRoot: true
    runAsUser: 1001
    seccompProfile:
      type: RuntimeDefault
  customLivenessProbe: {}
  customReadinessProbe: {}
  customStartupProbe: {}
  defaultBuckets: thanos
  deployment:
    updateStrategy:
      type: Recreate
  disableWebUI: false
  enabled: false
  extraDeploy: []
  extraEnvVars: []
  extraEnvVarsCM: ""
  extraEnvVarsSecret: ""
  extraVolumeMounts: []
  extraVolumes: []
  fullnameOverride: ""
  global:
    imagePullSecrets: []
    imageRegistry: ""
    storageClass: ""
  hostAliases: []
  image:
    debug: false
    digest: ""
    pullPolicy: IfNotPresent
    pullSecrets: []
    registry: docker.io
    repository: bitnami/minio
    tag: 2023.12.23-debian-11-r3
  ingress:
    annotations: {}
    apiVersion: ""
    enabled: false
    extraHosts: []
    extraPaths: []
    extraRules: []
    extraTls: []
    hostname: minio.local
    ingressClassName: ""
    path: /
    pathType: ImplementationSpecific
    secrets: []
    selfSigned: false
    servicePort: minio-console
    tls: false
  initContainers: []
  kubeVersion: ""
  lifecycleHooks: {}
  livenessProbe:
    enabled: true
    failureThreshold: 5
    initialDelaySeconds: 5
    periodSeconds: 5
    successThreshold: 1
    timeoutSeconds: 5
  metrics:
    prometheusAuthType: public
    prometheusRule:
      additionalLabels: {}
      enabled: false
      namespace: ""
      rules: []
    serviceMonitor:
      apiVersion: ""
      enabled: false
      honorLabels: false
      interval: 30s
      jobLabel: ""
      labels: {}
      metricRelabelings: []
      namespace: ""
      paths:
      - /minio/v2/metrics/cluster
      - /minio/v2/metrics/node
      relabelings: []
      scrapeTimeout: ""
      selector: {}
      tlsConfig: {}
  mode: standalone
  nameOverride: ""
  namespaceOverride: ""
  networkPolicy:
    allowExternal: true
    enabled: false
    extraFromClauses: []
  nodeAffinityPreset:
    key: ""
    type: ""
    values: []
  nodeSelector: {}
  pdb:
    create: false
    maxUnavailable: ""
    minAvailable: 1
  persistence:
    accessModes:
    - ReadWriteOnce
    annotations: {}
    enabled: true
    existingClaim: ""
    mountPath: /bitnami/minio/data
    size: 8Gi
    storageClass: ""
  podAffinityPreset: ""
  podAnnotations: {}
  podAntiAffinityPreset: soft
  podLabels: {}
  podSecurityContext:
    enabled: true
    fsGroup: 1001
    fsGroupChangePolicy: OnRootMismatch
  priorityClassName: ""
  provisioning:
    args: []
    buckets: []
    cleanupAfterFinished:
      enabled: false
      seconds: 600
    command: []
    config: []
    containerSecurityContext:
      allowPrivilegeEscalation: false
      capabilities:
        drop:
        - ALL
      enabled: true
      privileged: false
      readOnlyRootFilesystem: false
      runAsNonRoot: true
      runAsUser: 1001
      seccompProfile:
        type: RuntimeDefault
    enabled: false
    extraCommands: []
    extraVolumeMounts: []
    extraVolumes: []
    groups: []
    nodeSelector: {}
    podAnnotations: {}
    podLabels: {}
    podSecurityContext:
      enabled: true
      fsGroup: 1001
    policies: []
    resources:
      limits: {}
      requests: {}
    schedulerName: ""
    users: []
    usersExistingSecrets: []
  readinessProbe:
    enabled: true
    failureThreshold: 5
    initialDelaySeconds: 5
    periodSeconds: 5
    successThreshold: 1
    timeoutSeconds: 1
  resources:
    limits: {}
    requests: {}
  schedulerName: ""
  service:
    annotations: {}
    clusterIP: ""
    externalTrafficPolicy: Cluster
    extraPorts: []
    headless:
      annotations: {}
    loadBalancerIP: ""
    loadBalancerSourceRanges: []
    nodePorts:
      api: ""
      console: ""
    ports:
      api: 9000
      console: 9001
    type: ClusterIP
  serviceAccount:
    annotations: {}
    automountServiceAccountToken: true
    create: true
    name: ""
  sidecars: []
  startupProbe:
    enabled: false
    failureThreshold: 60
    initialDelaySeconds: 0
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 5
  statefulset:
    drivesPerNode: 1
    podManagementPolicy: Parallel
    replicaCount: 4
    updateStrategy:
      type: RollingUpdate
    zones: 1
  terminationGracePeriodSeconds: ""
  tls:
    autoGenerated: false
    enabled: false
    existingSecret: ""
    mountPath: ""
  tolerations: []
  topologySpreadConstraints: []
  volumePermissions:
    containerSecurityContext:
      runAsUser: 0
    enabled: false
    image:
      digest: ""
      pullPolicy: IfNotPresent
      pullSecrets: []
      registry: docker.io
      repository: bitnami/os-shell
      tag: 11-debian-11-r93
    resources:
      limits: {}
      requests: {}
nameOverride: ""
objstoreConfig: ""
query:
  affinity: {}
  args: []
  automountServiceAccountToken: true
  autoscaling:
    enabled: false
    maxReplicas: ""
    minReplicas: ""
    targetCPU: ""
    targetMemory: ""
  command: []
  containerPorts:
    grpc: 10901
    http: 10902
  containerSecurityContext:
    allowPrivilegeEscalation: false
    capabilities:
      drop:
      - ALL
    enabled: true
    privileged: false
    readOnlyRootFilesystem: true
    runAsNonRoot: true
    runAsUser: 1001
    seLinuxOptions: null
    seccompProfile:
      type: RuntimeDefault
  customLivenessProbe: {}
  customReadinessProbe: {}
  customStartupProbe: {}
  dnsConfig: {}
  dnsDiscovery:
    enabled: true
    sidecarsNamespace: ""
    sidecarsService: ""
  dnsPolicy: ""
  enabled: true
  existingSDConfigmap: ""
  extraEnvVars: []
  extraEnvVarsCM: ""
  extraEnvVarsSecret: ""
  extraFlags: []
  extraVolumeMounts: []
  extraVolumes: []
  grpc:
    client:
      serverName: ""
      tls:
        autoGenerated: false
        ca: ""
        cert: ""
        enabled: false
        existingSecret: {}
        key: ""
    server:
      tls:
        autoGenerated: false
        ca: ""
        cert: ""
        enabled: false
        existingSecret: {}
        key: ""
  hostAliases: []
  ingress:
    annotations: {}
    apiVersion: ""
    enabled: false
    extraHosts: []
    extraRules: []
    extraTls: []
    grpc:
      annotations: {}
      apiVersion: ""
      enabled: false
      extraHosts: []
      extraRules: []
      extraTls: []
      hostname: thanos-grpc.local
      ingressClassName: ""
      path: /
      pathType: ImplementationSpecific
      secretName: ""
      secrets: []
      selfSigned: false
      tls: false
    hostname: thanos.local
    ingressClassName: ""
    path: /
    pathType: ImplementationSpecific
    secretName: ""
    secrets: []
    selfSigned: false
    tls: false
  initContainers: []
  lifecycleHooks: {}
  livenessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  logFormat: logfmt
  logLevel: info
  networkPolicy:
    allowExternal: true
    allowExternalEgress: true
    enabled: true
    extraEgress: []
    extraIngress: []
    ingressNSMatchLabels: {}
    ingressNSPodMatchLabels: {}
  nodeAffinityPreset:
    key: ""
    type: ""
    values: []
  nodeSelector: {}
  pdb:
    create: false
    maxUnavailable: ""
    minAvailable: 1
  podAffinityPreset: ""
  podAnnotations: {}
  podAntiAffinityPreset: soft
  podAntiAffinityPresetTopologyKey: ""
  podLabels: {}
  podSecurityContext:
    enabled: true
    fsGroup: 1001
    fsGroupChangePolicy: Always
    supplementalGroups: []
    sysctls: []
  priorityClassName: ""
  pspEnabled: false
  rbac:
    create: false
    rules: []
  readinessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  replicaCount: 1
  replicaLabel:
  - replica
  resources: {}
  resourcesPreset: none
  revisionHistoryLimit: 10
  schedulerName: ""
  sdConfig: ""
  service:
    additionalHeadless: false
    annotations: {}
    clusterIP: ""
    externalTrafficPolicy: Cluster
    extraPorts: []
    headless:
      annotations: {}
    labelSelectorsOverride: {}
    labels: {}
    loadBalancerIP: ""
    loadBalancerSourceRanges: []
    nodePorts:
      http: ""
    ports:
      http: 9090
    type: ClusterIP
  serviceAccount:
    annotations: {}
    automountServiceAccountToken: false
    create: true
    name: ""
  serviceGrpc:
    additionalHeadless: false
    annotations: {}
    clusterIP: ""
    externalTrafficPolicy: Cluster
    extraPorts: []
    headless:
      annotations: {}
    labelSelectorsOverride: {}
    labels: {}
    loadBalancerIP: ""
    loadBalancerSourceRanges: []
    nodePorts:
      grpc: ""
    ports:
      grpc: 10901
    type: ClusterIP
  sidecars: []
  startupProbe:
    enabled: false
    failureThreshold: 15
    initialDelaySeconds: 5
    periodSeconds: 5
    successThreshold: 1
    timeoutSeconds: 1
  stores: []
  tolerations: []
  topologySpreadConstraints: []
  updateStrategy:
    type: RollingUpdate
queryFrontend:
  affinity: {}
  args: []
  automountServiceAccountToken: true
  autoscaling:
    enabled: false
    maxReplicas: ""
    minReplicas: ""
    targetCPU: ""
    targetMemory: ""
  command: []
  config: ""
  containerPorts:
    http: 9090
  containerSecurityContext:
    allowPrivilegeEscalation: false
    capabilities:
      drop:
      - ALL
    enabled: true
    privileged: false
    readOnlyRootFilesystem: true
    runAsNonRoot: true
    runAsUser: 1001
    seLinuxOptions: null
    seccompProfile:
      type: RuntimeDefault
  customLivenessProbe: {}
  customReadinessProbe: {}
  customStartupProbe: {}
  dnsConfig: {}
  dnsPolicy: ""
  enabled: true
  existingConfigmap: ""
  extraEnvVars: []
  extraEnvVarsCM: ""
  extraEnvVarsSecret: ""
  extraFlags: []
  extraVolumeMounts: []
  extraVolumes: []
  hostAliases: []
  ingress:
    annotations: {}
    apiVersion: ""
    enabled: false
    extraHosts: []
    extraRules: []
    extraTls: []
    hostname: thanos.local
    ingressClassName: ""
    overrideAlertQueryURL: true
    path: /
    pathType: ImplementationSpecific
    secrets: []
    selfSigned: false
    tls: false
  initContainers: []
  lifecycleHooks: {}
  livenessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  logFormat: logfmt
  logLevel: info
  networkPolicy:
    allowExternal: true
    allowExternalEgress: true
    enabled: true
    extraEgress: []
    extraIngress: []
    ingressNSMatchLabels: {}
    ingressNSPodMatchLabels: {}
  nodeAffinityPreset:
    key: ""
    type: ""
    values: []
  nodeSelector: {}
  pdb:
    create: false
    maxUnavailable: ""
    minAvailable: 1
  podAffinityPreset: ""
  podAnnotations: {}
  podAntiAffinityPreset: soft
  podLabels: {}
  podSecurityContext:
    enabled: true
    fsGroup: 1001
    fsGroupChangePolicy: Always
    supplementalGroups: []
    sysctls: []
  priorityClassName: ""
  pspEnabled: false
  rbac:
    create: false
    rules: []
  readinessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  replicaCount: 1
  resources: {}
  resourcesPreset: none
  revisionHistoryLimit: 10
  schedulerName: ""
  service:
    annotations: {}
    clusterIP: ""
    externalTrafficPolicy: Cluster
    extraPorts: []
    labelSelectorsOverride: {}
    labels: {}
    loadBalancerIP: ""
    loadBalancerSourceRanges: []
    nodePorts:
      http: ""
    ports:
      http: 9090
    type: ClusterIP
  serviceAccount:
    annotations: {}
    automountServiceAccountToken: false
    create: true
    name: ""
  sidecars: []
  startupProbe:
    enabled: false
    failureThreshold: 15
    initialDelaySeconds: 5
    periodSeconds: 5
    successThreshold: 1
    timeoutSeconds: 1
  tolerations: []
  topologySpreadConstraints: []
  updateStrategy:
    type: RollingUpdate
receive:
  affinity: {}
  args: []
  automountServiceAccountToken: true
  autoscaling:
    enabled: false
    maxReplicas: ""
    minReplicas: ""
    targetCPU: ""
    targetMemory: ""
  command: []
  config: []
  containerPorts:
    grpc: 10901
    http: 10902
    remote: 19291
  containerSecurityContext:
    allowPrivilegeEscalation: false
    capabilities:
      drop:
      - ALL
    enabled: true
    privileged: false
    readOnlyRootFilesystem: true
    runAsNonRoot: true
    runAsUser: 1001
    seLinuxOptions: null
    seccompProfile:
      type: RuntimeDefault
  customLivenessProbe: {}
  customReadinessProbe: {}
  customStartupProbe: {}
  dnsConfig: {}
  dnsPolicy: ""
  enabled: false
  existingConfigmap: ""
  extraEnvVars: []
  extraEnvVarsCM: ""
  extraEnvVarsSecret: ""
  extraFlags: []
  extraVolumeMounts: []
  extraVolumes: []
  grpc:
    server:
      tls:
        autoGenerated: false
        ca: ""
        cert: ""
        enabled: false
        existingSecret: {}
        key: ""
  hostAliases: []
  ingress:
    annotations: {}
    apiVersion: ""
    enabled: false
    extraHosts: []
    extraRules: []
    extraTls: []
    hostname: thanos-receive.local
    ingressClassName: ""
    path: /
    pathType: ImplementationSpecific
    secrets: []
    selfSigned: false
    tls: false
  initContainers: []
  lifecycleHooks: {}
  livenessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  logFormat: logfmt
  logLevel: info
  minReadySeconds: 0
  mode: standalone
  networkPolicy:
    allowExternal: true
    allowExternalEgress: true
    enabled: true
    extraEgress: []
    extraIngress: []
    ingressNSMatchLabels: {}
    ingressNSPodMatchLabels: {}
  nodeAffinityPreset:
    key: ""
    type: ""
    values: []
  nodeSelector: {}
  pdb:
    create: false
    maxUnavailable: ""
    minAvailable: 1
  persistence:
    accessModes:
    - ReadWriteOnce
    annotations: {}
    enabled: true
    existingClaim: ""
    size: 8Gi
    storageClass: ""
  podAffinityPreset: ""
  podAnnotations: {}
  podAntiAffinityPreset: soft
  podLabels: {}
  podManagementPolicy: OrderedReady
  podSecurityContext:
    enabled: true
    fsGroup: 1001
    fsGroupChangePolicy: Always
    supplementalGroups: []
    sysctls: []
  priorityClassName: ""
  readinessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  replicaCount: 1
  replicaLabel: replica
  replicationFactor: 1
  resources: {}
  resourcesPreset: none
  revisionHistoryLimit: 10
  schedulerName: ""
  service:
    additionalHeadless: false
    annotations: {}
    clusterIP: ""
    externalTrafficPolicy: Cluster
    extraPorts: []
    headless:
      annotations: {}
    labelSelectorsOverride: {}
    labels: {}
    loadBalancerIP: ""
    loadBalancerSourceRanges: []
    nodePorts:
      grpc: ""
      http: ""
      remote: ""
    ports:
      grpc: 10901
      http: 10902
      remote: 19291
    type: ClusterIP
  serviceAccount:
    annotations: {}
    automountServiceAccountToken: false
    create: true
    name: ""
  sidecars: []
  startupProbe:
    enabled: false
    failureThreshold: 15
    initialDelaySeconds: 5
    periodSeconds: 5
    successThreshold: 1
    timeoutSeconds: 1
  statefulsetLabels: {}
  tolerations: []
  topologySpreadConstraints: []
  tsdbRetention: 15d
  updateStrategy:
    type: RollingUpdate
receiveDistributor:
  affinity: {}
  args: []
  automountServiceAccountToken: true
  autoscaling:
    enabled: false
    maxReplicas: ""
    minReplicas: ""
    targetCPU: ""
    targetMemory: ""
  command: []
  containerSecurityContext:
    allowPrivilegeEscalation: false
    capabilities:
      drop:
      - ALL
    enabled: true
    privileged: false
    readOnlyRootFilesystem: true
    runAsNonRoot: true
    runAsUser: 1001
    seLinuxOptions: null
    seccompProfile:
      type: RuntimeDefault
  customLivenessProbe: {}
  customReadinessProbe: {}
  customStartupProbe: {}
  dnsConfig: {}
  dnsPolicy: ""
  enabled: false
  extraEnvVars: []
  extraEnvVarsCM: ""
  extraEnvVarsSecret: ""
  extraFlags: []
  extraVolumeMounts: []
  extraVolumes: []
  hostAliases: []
  initContainers: []
  lifecycleHooks: {}
  livenessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  logFormat: logfmt
  logLevel: info
  nodeAffinityPreset:
    key: ""
    type: ""
    values: []
  nodeSelector: {}
  pdb:
    create: false
    maxUnavailable: ""
    minAvailable: 1
  podAffinityPreset: ""
  podAnnotations: {}
  podAntiAffinityPreset: soft
  podLabels: {}
  podSecurityContext:
    enabled: true
    fsGroup: 1001
    fsGroupChangePolicy: Always
    supplementalGroups: []
    sysctls: []
  priorityClassName: ""
  readinessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  replicaCount: 1
  replicaLabel: replica
  replicationFactor: 1
  resources: {}
  resourcesPreset: none
  revisionHistoryLimit: 10
  schedulerName: ""
  serviceAccount:
    annotations: {}
    automountServiceAccountToken: false
    create: true
    name: ""
  sidecars: []
  startupProbe:
    enabled: false
    failureThreshold: 15
    initialDelaySeconds: 5
    periodSeconds: 5
    successThreshold: 1
    timeoutSeconds: 1
  tolerations: []
  topologySpreadConstraints: []
  updateStrategy:
    type: RollingUpdate
ruler:
  affinity: {}
  alertmanagers: []
  alertmanagersConfig: ""
  args: []
  automountServiceAccountToken: true
  autoscaling:
    enabled: false
    maxReplicas: ""
    minReplicas: ""
    targetCPU: ""
    targetMemory: ""
  clusterName: ""
  command: []
  config: ""
  containerPorts:
    grpc: 10901
    http: 10902
  containerSecurityContext:
    allowPrivilegeEscalation: false
    capabilities:
      drop:
      - ALL
    enabled: true
    privileged: false
    readOnlyRootFilesystem: true
    runAsNonRoot: true
    runAsUser: 1001
    seLinuxOptions: null
    seccompProfile:
      type: RuntimeDefault
  customLivenessProbe: {}
  customReadinessProbe: {}
  customStartupProbe: {}
  dnsConfig: {}
  dnsDiscovery:
    enabled: true
  dnsPolicy: ""
  enabled: false
  evalInterval: 1m
  existingConfigmap: ""
  extraEnvVars: []
  extraEnvVarsCM: ""
  extraEnvVarsSecret: ""
  extraFlags: []
  extraVolumeMounts: []
  extraVolumes: []
  hostAliases: []
  ingress:
    annotations: {}
    apiVersion: ""
    enabled: false
    extraHosts: []
    extraRules: []
    extraTls: []
    hostname: thanos-ruler.local
    ingressClassName: ""
    path: /
    pathType: ImplementationSpecific
    secrets: []
  initContainers: []
  lifecycleHooks: {}
  livenessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  logFormat: logfmt
  logLevel: info
  networkPolicy:
    allowExternal: true
    allowExternalEgress: true
    enabled: true
    extraEgress: []
    extraIngress: []
    ingressNSMatchLabels: {}
    ingressNSPodMatchLabels: {}
  nodeAffinityPreset:
    key: ""
    type: ""
    values: []
  nodeSelector: {}
  pdb:
    create: false
    maxUnavailable: ""
    minAvailable: 1
  persistence:
    accessModes:
    - ReadWriteOnce
    annotations: {}
    enabled: true
    existingClaim: ""
    size: 8Gi
    storageClass: ""
  podAffinityPreset: ""
  podAnnotations: {}
  podAntiAffinityPreset: soft
  podLabels: {}
  podManagementPolicy: OrderedReady
  podSecurityContext:
    enabled: true
    fsGroup: 1001
    fsGroupChangePolicy: Always
    supplementalGroups: []
    sysctls: []
  priorityClassName: ""
  queryURL: ""
  readinessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  replicaCount: 1
  replicaLabel: replica
  resources: {}
  resourcesPreset: none
  revisionHistoryLimit: 10
  schedulerName: ""
  service:
    additionalHeadless: false
    annotations: {}
    clusterIP: ""
    externalTrafficPolicy: Cluster
    extraPorts: []
    headless:
      annotations: {}
    labelSelectorsOverride: {}
    labels: {}
    loadBalancerIP: ""
    loadBalancerSourceRanges: []
    nodePorts:
      grpc: ""
      http: ""
    ports:
      grpc: 10901
      http: 9090
    type: ClusterIP
  serviceAccount:
    annotations: {}
    automountServiceAccountToken: false
    create: true
    name: ""
  sidecars: []
  startupProbe:
    enabled: false
    failureThreshold: 15
    initialDelaySeconds: 5
    periodSeconds: 5
    successThreshold: 1
    timeoutSeconds: 1
  tolerations: []
  topologySpreadConstraints: []
  updateStrategy:
    type: RollingUpdate
storegateway:
  affinity: {}
  args: []
  automountServiceAccountToken: true
  autoscaling:
    enabled: false
    maxReplicas: ""
    minReplicas: ""
    targetCPU: ""
    targetMemory: ""
  command: []
  config: ""
  containerPorts:
    grpc: 10901
    http: 10902
  containerSecurityContext:
    allowPrivilegeEscalation: false
    capabilities:
      drop:
      - ALL
    enabled: true
    privileged: false
    readOnlyRootFilesystem: true
    runAsNonRoot: true
    runAsUser: 1001
    seLinuxOptions: null
    seccompProfile:
      type: RuntimeDefault
  customLivenessProbe: {}
  customReadinessProbe: {}
  customStartupProbe: {}
  dnsConfig: {}
  dnsPolicy: ""
  enabled: false
  existingConfigmap: ""
  extraEnvVars: []
  extraEnvVarsCM: ""
  extraEnvVarsSecret: ""
  extraFlags: []
  extraVolumeMounts: []
  extraVolumes: []
  grpc:
    server:
      tls:
        autoGenerated: false
        ca: ""
        cert: ""
        enabled: false
        existingSecret: {}
        key: ""
  hostAliases: []
  ingress:
    annotations: {}
    apiVersion: ""
    enabled: false
    extraHosts: []
    extraRules: []
    extraTls: []
    grpc:
      annotations: {}
      apiVersion: ""
      enabled: false
      extraHosts: []
      extraRules: []
      extraTls: []
      hostname: thanos-grpc.local
      ingressClassName: ""
      path: /
      pathType: ImplementationSpecific
      secrets: []
      selfSigned: false
      tls: false
    hostname: thanos-storegateway.local
    ingressClassName: ""
    path: /
    pathType: ImplementationSpecific
    secrets: []
    selfSigned: false
    tls: false
  initContainers: []
  lifecycleHooks: {}
  livenessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  logFormat: logfmt
  logLevel: info
  networkPolicy:
    allowExternal: true
    allowExternalEgress: true
    enabled: true
    extraEgress: []
    extraIngress: []
    ingressNSMatchLabels: {}
    ingressNSPodMatchLabels: {}
  nodeAffinityPreset:
    key: ""
    type: ""
    values: []
  nodeSelector: {}
  pdb:
    create: false
    maxUnavailable: ""
    minAvailable: 1
  persistence:
    accessModes:
    - ReadWriteOnce
    annotations: {}
    enabled: true
    existingClaim: ""
    labels: {}
    size: 8Gi
    storageClass: ""
  podAffinityPreset: ""
  podAnnotations: {}
  podAntiAffinityPreset: soft
  podLabels: {}
  podManagementPolicy: OrderedReady
  podSecurityContext:
    enabled: true
    fsGroup: 1001
    fsGroupChangePolicy: Always
    supplementalGroups: []
    sysctls: []
  priorityClassName: ""
  readinessProbe:
    enabled: true
    failureThreshold: 6
    initialDelaySeconds: 30
    periodSeconds: 10
    successThreshold: 1
    timeoutSeconds: 30
  replicaCount: 1
  resources: {}
  resourcesPreset: none
  revisionHistoryLimit: 10
  schedulerName: ""
  service:
    additionalHeadless: false
    annotations: {}
    clusterIP: ""
    externalTrafficPolicy: Cluster
    extraPorts: []
    headless:
      annotations: {}
    labelSelectorsOverride: {}
    labels: {}
    loadBalancerIP: ""
    loadBalancerSourceRanges: []
    nodePorts:
      grpc: ""
      http: ""
    ports:
      grpc: 10901
      http: 9090
    type: ClusterIP
  serviceAccount:
    annotations: {}
    automountServiceAccountToken: false
    create: true
    name: ""
  sharded:
    enabled: false
    hashPartitioning:
      shards: ""
    service:
      clusterIPs: []
      grpc:
        nodePorts: []
      http:
        nodePorts: []
      loadBalancerIPs: []
    timePartitioning:
    - max: ""
      min: ""
  sidecars: []
  startupProbe:
    enabled: false
    failureThreshold: 15
    initialDelaySeconds: 5
    periodSeconds: 5
    successThreshold: 1
    timeoutSeconds: 1
  tolerations: []
  topologySpreadConstraints: []
  updateStrategy:
    type: RollingUpdate
volumePermissions:
  enabled: false
  image:
    digest: ""
    pullPolicy: IfNotPresent
    pullSecrets: []
    registry: docker.io
    repository: bitnami/os-shell
    tag: 12-debian-12-r16
```


**#with overide values**

yaml
Copy code
objstoreConfig: |-
  type: s3
  config:
    bucket: thanos
    endpoint: {{ include "thanos.minio.fullname" . }}.monitoring.svc.cluster.local:9000
    access_key: minio
    secret_key: KEY
    insecure: true  
querier:
  stores:
    - SIDECAR-SERVICE-IP-ADDRESS-1:10901
    - SIDECAR-SERVICE-IP-ADDRESS-2:10901
bucketweb:
  enabled: true
compactor:
  enabled: true
storegateway:
  enabled: true
ruler:
  enabled: true
  alertmanagers:
    - http://prometheus-operator-alertmanager.monitoring.svc.cluster.local:9093
  config: |-
    groups:
      - name: "metamonitoring"
        rules:
          - alert: "PrometheusDown"
            expr: absent(up{prometheus="monitoring/prometheus-operator"})    
minio:
  enabled: true
  accessKey:
    password: "minio"
  secretKey:
    password: "KEY"
  defaultBuckets: "thanos"
To install Thanos in the Thanos namespace using this configuration file, you can use the following Helm upgrade command:

```
helm upgrade -i thanos bitnami/thanos --namespace thanos --values values.yaml
```
This command will upgrade or install the Thanos chart in the thanos namespace using the specified values.yaml configuration file.



 installing kube-prometheus-stack with Thanos sidecars. Here are the steps:

Create or update the prometheus-europe.yaml and prometheus-united-states.yaml files with the Thanos sidecar configurations:

prometheus-europe.yaml:

```
nameOverride: "eu"
namespaceOverride: "europe"
nodeExporter:
  enabled: false
grafana:
  enabled: false
alertmanager:
  enabled: false
kubeStateMetrics:
  enabled: false
prometheus:
  prometheusSpec:
    replicas: 2
    replicaExternalLabelName: "replica"
    prometheusExternalLabelName: "cluster"
    thanos:
      baseImage: quay.io/thanos/thanos
      version: v0.24.0
thanosService:
  enabled: enabled
  annotations: {}
  labels: {}
  externalTrafficPolicy: Cluster
  type: ClusterIP
```
prometheus-united-states.yaml:

```
nameOverride: "us"
namespaceOverride: "united-states"
nodeExporter:
  enabled: false
grafana:
  enabled: false
alertmanager:
  enabled: false
kubeStateMetrics:
  enabled: false
prometheus:
  prometheusSpec:
    replicaExternalLabelName: "replica"
    prometheusExternalLabelName: "cluster"
    thanos:
      baseImage: quay.io/thanos/thanos
      version: v0.24.0
thanosService:
  enabled: enabled
  annotations: {}
  labels: {}
  externalTrafficPolicy: Cluster
  type: ClusterIP
```
Upgrade Prometheus in each region with the new configuration:

```
helm -n europe upgrade -i prometheus-europe prometheus-community/kube-prometheus-stack -f prometheus-europe.yaml
```
```
helm -n united-states upgrade -i prometheus-united-states prometheus-community/kube-prometheus-stack -f prometheus-united-states.yaml
```

For Europe:

```
helm upgrade -n europe -i prometheus-europe prometheus-community/kube-prometheus-stack \
  --set prometheus.thanos.create=true \
  --set prometheus.service.type=ClusterIP \
  --set prometheus.thanos.service.type=LoadBalancer \
  -f prometheus-europe.yaml
```
For United States:

```
helm upgrade -n united-states -i prometheus-united-states prometheus-community/kube-prometheus-stack \
  --set prometheus.thanos.create=true \
  --set prometheus.service.type=ClusterIP \
  --set prometheus.thanos.service.type=LoadBalancer \
  -f prometheus-united-states.yaml
```



Check the status of Prometheus pods in each region:

```
kubectl -n europe get pods -l app.kubernetes.io/name=prometheus
```
```
kubectl -n united-states get pods -l app.kubernetes.io/name=prometheus
```


Step 4: Configure Grafana to use Thanos as a data source
Follow these steps:

From the Grafana dashboard, click the “Add data source” button.

On the “Choose data source type” page, select “Prometheus”.
![image](https://github.com/vijaybiradar/thanos/assets/38376802/ab40566a-6090-400d-b458-34d9bb387149)


Grafana data source

On the “Settings” page, set the URL for the Prometheus server to http://NAME:PORT, where NAME is the DNS name for the Thanos service obtained at the end of Step 2 and PORT is the corresponding service port. Leave all other values at their default.

![image](https://github.com/vijaybiradar/thanos/assets/38376802/31fe56b9-e358-40a2-9c49-0ad0eb65356c)

![image](https://github.com/vijaybiradar/thanos/assets/38376802/bada3baf-42da-4e6c-ab99-fa10b553f236)

Grafana data source configuration

Click “Save & Test” to save and test the configuration. If everything is configured correctly, you should see a success message like the one below.
![image](https://github.com/vijaybiradar/thanos/assets/38376802/9697d760-59cb-41aa-8742-1c16267d3dd2)


Grafana test

Step 5: Test the multi-cluster monitoring system
At this point, you can start deploying applications into your “data producer” clusters and collating the metrics in Thanos and Grafana. For demonstration purposes, this guide will deploy a MariaDB replication cluster using Bitnami’s MariaDB Helm chart in each “data producer” cluster and display the metrics generated by each MariaDB service in Grafana.

Deploy MariaDB in each cluster with one master and one slave using the production configuration with the commands below. Replace the MARIADB-ADMIN-PASSWORD and MARIADB-REPL-PASSWORD placeholders with the database administrator account and replication account password respectively. You can also optionally create a MariaDB user account for application use by specifying values for the USER-PASSWORD, USER-NAME and DB-NAME placeholders.




https://medium.com/@dikshantdevops/elevating-observability-unleashing-the-power-of-kube-prometheus-stack-with-thanos-sidecar-9d0c159bef2e
https://medium.com/nerd-for-tech/deep-dive-into-thanos-part-ii-8f48b8bba132
https://medium.com/hiredscore-engineering/using-thanos-to-store-prometheus-on-many-kubernetes-clusters-fd24b63873d8
https://medium.com/@kakashiliu/deploy-prometheus-operator-with-thanos-60210eff172b
