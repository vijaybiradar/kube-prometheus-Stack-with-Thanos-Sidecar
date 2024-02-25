Installing Prometheus in Europe and United States using kube-prometheus-stack
Create Namespaces:

```
kubectl create ns europe
```
```
kubectl create ns united-states
```
Prepare Configuration Files:

Create or modify prometheus-europe.yaml with the following content:

```
nameOverride: "eu"
namespaceOverride: "europe"

nodeExporter:
  enabled: false

grafana:
  enabled: true

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
    enabled: true
    annotations: {}
    labels: {}
    externalTrafficPolicy: Cluster
    type: ClusterIP
    portName: grpc
    port: 10901
    targetPort: "grpc"
    httpPortName: http
    httpPort: 10902
    targetHttpPort: "http"
    clusterIP: ""
    nodePort: 30901
    httpNodePort: 30902
```
Create or modify prometheus-united-states.yaml with the following content:

```
nameOverride: "us"
namespaceOverride: "united-states"

nodeExporter:
  enabled: false

grafana:
  enabled: true

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
    enabled: true
    annotations: {}
    labels: {}
    externalTrafficPolicy: Cluster
    type: ClusterIP
    portName: grpc
    port: 10901
    targetPort: "grpc"
    httpPortName: http
    httpPort: 10902
    targetHttpPort: "http"
    clusterIP: ""
    nodePort: 30901
    httpNodePort: 30902
```
Install Prometheus:

For Europe:

```
helm upgrade -n europe -i prometheus-europe prometheus-community/kube-prometheus-stack \
  --set prometheus.thanos.create=true \
  --set prometheus.service.type=ClusterIP \
  --set prometheus.thanos.service.type=LoadBalancer \
  -f prometheus-europe.yaml
```
# Generate overridevalues.yaml
helm get values prometheus-europe -n europe --revision 1 -o yaml > overridevalues_prometheus_europe.yaml

# Generate fullvalues.yaml
helm get values prometheus-europe -n europe -a --revision 1 -o yaml > fullvalues_prometheus_europe.yaml


![image](https://github.com/vijaybiradar/thanos/assets/38376802/6dc64e2e-df19-425e-9182-1e0d7e2db937)
![image](https://github.com/vijaybiradar/thanos/assets/38376802/5b7a33e6-e7b7-4144-b06a-d44cc3434e16)


For United States:

```
helm upgrade -n united-states -i prometheus-united-states prometheus-community/kube-prometheus-stack \
  --set prometheus.thanos.create=true \
  --set prometheus.service.type=ClusterIP \
  --set prometheus.thanos.service.type=LoadBalancer \
  -f prometheus-united-states.yaml
```

# Generate overridevalues.yaml
```
helm get values prometheus-united-states -n united-states --revision 1 -o yaml > overridevalues_prometheus_united_states.yaml
```

# Generate fullvalues.yaml
```
helm get values prometheus-united-states -n united-states -a --revision 1 -o yaml > fullvalues_prometheus_united_states.yaml
```

![image](https://github.com/vijaybiradar/thanos/assets/38376802/ae7ec4d7-e789-4d83-9d66-406165a43c64)

![image](https://github.com/vijaybiradar/thanos/assets/38376802/2a7d3657-875e-415b-899a-1f1743d1a37d)



To install Thanos using Helm, follow these steps:

Create a Namespace for Thanos (Optional):
If you want to install Thanos in a specific namespace, create the namespace first:
```
kubectl create ns thanos
```
Install Thanos Chart with S3-compatible object storage service:

Here's the content of the overridevalues.yaml file::



```
crds:
  install: true

objstoreConfig: |-
  type: s3
  config:
    bucket: thanos-prod-north-virginia
    endpoint: s3.amazonaws.com
    access_key: ${access_key}
    secret_key: ${secret_key}
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
  enabled: false

minio:
  enabled: false

metrics:
  enabled: true
  serviceMonitor:
    enabled: true

```
 Installation:
```
helm upgrade thanos bitnami/thanos -n thanos --values overridevalues.yaml
```
![image](https://github.com/vijaybiradar/thanos/assets/38376802/eb9b9698-f521-4a46-8a46-b0ee368367de)


```
helm ls -n thanos
```
![image](https://github.com/vijaybiradar/thanos/assets/38376802/899d5b12-8189-463f-a632-23e5add1165d)


# Generate fullvalues.yaml
If you want to save the configuration values used during installation, you can export them to a YAML file using the following command:
```
helm get values thanos -n thanos -a --revision 1 -o yaml > fullvalues.yaml
```
# Generate overridevalues.yaml

```
helm get values thanos -n thanos --revision 1 -o yaml > fullvalues.yaml
```

Create a file named “thanos-storage-config.yaml” for S3 Storage configuration:
```
type: s3
config:
    bucket: thanos-prod-north-virginia
    endpoint: s3.amazonaws.com
    access_key: ${access_key}
    secret_key: ${secret_key}
```

Note: To get the secret and access key, you need a policy that allows you to push data to S3. You can use below policy and attach it with a user.
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Statement",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:PutObject",
                "s3:PutObjectAcl"
            ],
            "Resource": [
                "arn:aws:s3:::thanos-prod-north-virginia/*",
                "arn:aws:s3:::thanos-prod-north-virginia"
            ]
        }
    ]
}
```
 Create secret using below command:
```
For Europe (namespace: europe):

```
kubectl -n europe create secret generic thanos-objstore-config --from-file=thanos.yaml=thanos-storage-config.yaml
```
For United States (namespace: united-states):

```
kubectl -n united-states create secret generic thanos-objstore-config --from-file=thanos.yaml=thanos-storage-config.yaml
```
These commands will create the necessary secret containing the Thanos object storage configuration 
```


# Configure Grafana to use Thanos as a data source
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








