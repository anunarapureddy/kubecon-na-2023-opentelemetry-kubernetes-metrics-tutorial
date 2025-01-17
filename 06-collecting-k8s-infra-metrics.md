# Collecting Kubernetes infrastracture metrics

This section of tutorial will focus specifically how to collect your infrastructure metrics with OpenTelemetry Operator. The collection of infrastructure metrics consists of a few components that will be introduced. Parts of this document are based on ["Important Components for Kubernetes"](https://opentelemetry.io/docs/kubernetes/collector/components/#kubeletstats-receiver) by OpenTelemetry authors which is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Some parts have been adjusted for the purpose of this tutorial.

## Prerequisite - Service Account

Many Kubernetes related components in this part of tutorial uses the Kubernetes API, therefore they require proper permissions to work correctly. For most cases, you should give the service account running the Collector the following permissions via a ClusterRole. Apply the following YAML configuration file to your cluster to create all a service account with all necessary permissions:

```bash
kubectl apply -f https://raw.githubusercontent.com/pavolloffay/kubecon-eu-2023-opentelemetry-kubernetes-tutorial/main/manifests/service-account.yaml
```

## Kubelet Stats Receiver

Each Kubernetes node runs a kubelet that includes an API server. The Kubernetes Receiver connects to that kubelet via the API server to collect metrics about the node and the workloads running on the node. Due to the nature of this component, we recommend to run it as a daemon set on each node.

There are different methods for authentication, but typically a service account is used (as is also the case for this tutorial). By default, metrics will be collected for pods and nodes, but you can configure the receiver to collect container and volume metrics as well. The receiver also allows configuring how often the metrics are collected:

```yaml
receivers:
  kubeletstats:
    collection_interval: 20s
    auth_type: 'serviceAccount'
    endpoint: '${env:K8S_NODE_NAME}:10250'
```

For specific details about which metrics are collected, see
[Default Metrics](https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/receiver/kubeletstatsreceiver/documentation.md).
For specific configuration details, see
[Kubeletstats Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/receiver/kubeletstatsreceiver).

## Kubernetes Cluster Receiver

The Kubernetes Cluster Receiver collects metrics and entity events about the
cluster as a whole using the Kubernetes API server. Use this receiver to answer
questions about pod phases, node conditions, and other cluster-wide questions.
Since the receiver gathers telemetry for the cluster as a whole, only one
instance of the receiver is needed across the cluster in order to collect all
the data.

There are different methods for authentication, but typically a service account
is used (as is also the case for this tutorial). For node conditions, the receiver only collects `Ready` by default, but it can
be configured to collect more. The receiver can also be configured to report a
set of allocatable resources, such as `cpu` and `memory`:

```yaml
k8s_cluster:
  auth_type: serviceAccount
  node_conditions_to_report:
    - Ready
    - MemoryPressure
  allocatable_types_to_report:
    - cpu
    - memory
```

To learn more about the metrics that are collected, see
[Default Metrics](https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/receiver/k8sclusterreceiver/documentation.md)
For configuration details, see
[Kubernetes Cluster Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/k8sclusterreceiver).


## Prometheus Receiver

To effectively monitor your Kubernetes setup, it's crucial to gather specific metrics from different sources. Some of these metrics are built right into the kubelet, while others require additional deployment.

**Built-in Exporters:**
1. kube-apiserver
2. kubelet
3. cAdvisor
4. kube-service-endpoints
5. kubernetes-pods

**Exporters Requiring Deployment:**
1. kube-state-metrics
2. node-exporter 
3. Blackbox Exporter

For scrape configuration, the [Prometheus upstream repository](https://raw.githubusercontent.com/prometheus/prometheus/main/documentation/examples/prometheus-kubernetes.yml) provides a comprehensive reference. In the purpose of this tutorial, we'll setup the OpenTelemetry Collector to collect metrics from the embedded exporters.

**Setting Up OpenTelemetry Collector for Kubernetes Metrics**

By applying the below manifest, you willinstall the OpenTelemetry Collector using the provided Prometheus scrape configurations. These configurations are specified in the `opentelemetry-collector-config.yaml` file, covering essential scrape settings for embedded exporters (kube-apiserver, kubelet, cAdvisor, kube-service-endpoints, and kubernetes-pods).

```bash
kubectl apply -f https://raw.githubusercontent.com/pavolloffay/kubecon-na-2023-opentelemetry-kubernetes-metrics-tutorial/main/backend/06-collector-prom-k8s-metrics
```

TODO: Add Grafana dashboard links

---
[Next steps](./07-correlation.md)
