Before deploying this YAML manifest, please make sure Prometheus Exporter Monitoring is enabled in AppDynamics Distribution for OpenTelemetry Collector. 

https://docs.appdynamics.com/appd-cloud/en/infrastructure-monitoring/prometheus-cloud-infrastructure-monitoring

When multiple OTel demo instances are being monitored by the same CNAO instance, the 'appdynamics.com/kafka_cluster_name' annotation in kafka exporter service definition should be unique to have each kafka cluster monitored seperately. Please see below:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: opentelemetry-demo-kafka-exporter
  labels:
    helm.sh/chart: opentelemetry-demo-0.22.0  
    opentelemetry.io/name: opentelemetry-demo-kafka
    app.kubernetes.io/instance: opentelemetry-demo
    app.kubernetes.io/component: kafka
    app.kubernetes.io/name: opentelemetry-demo-kafka
    app.kubernetes.io/version: "1.4.0"
    app.kubernetes.io/part-of: opentelemetry-demo
    app.kubernetes.io/managed-by: Helm
  annotations:
    appdynamics.com/exporter_type: "kafka"
    appdynamics.com/kafka_cluster_name: "opentelemetry-demo-kafka-cluster" #modify this line accordingly
    prometheus.io/path: "/metrics"
    prometheus.io/port: "9308"
spec:
  type: ClusterIP
  ports:
  - port: 9308
    protocol: TCP
    targetPort: 9308
  selector:
    opentelemetry.io/name: opentelemetry-demo-kafka
```