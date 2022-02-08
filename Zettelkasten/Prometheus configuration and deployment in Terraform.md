# [Prometheus configuration and deployment in Terraform](https://stackoverflow.com/questions/67567449/prometheus-configuration-and-deployment-in-terraform)
[](https://stackoverflow.com/posts/67567449/timeline)

I have an IaaC project that use Terraform and Helm charts to deploy Prometheus (along with Grafana, Kubernetes and the platform app). I do have Kubernetes metrics on Prometheus. However, I realize that the configuration on deployed Prometheus doesn't really come from the Prometheus config file in the project. I am not sure if it's the problem with configMap or other configuration. Here are the files concerned in the project:

`/terraform/kubernetes/files/prometheus_config_map.yaml`

```yaml
global:
  scrape_interval: 15s
scrape_configs:
- job_name: 'prometheus'
  static_configs:
  - targets: ['localhost:9090']
- job_name: 'kubernetes-pods'
  kubernetes_sd_configs:
  - role: pod
  relabel_configs:
  - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
    action: keep
    regex: true
  - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
    action: replace
    target_label: __metrics_path__
    regex: (.+)
  - source_labels: [__address__, __meta_kubernetes_pod_annotation_prometheus_io_port]
    action: replace
    regex: ([^:]+)(?::\d+)?;(\d+)
    replacement: $1:$2
    target_label: __address__
  - action: labelmap
    regex: __meta_kubernetes_pod_label_(.+)
  - source_labels: [__meta_kubernetes_namespace]
    action: replace
    target_label: kubernetes_namespace
  - source_labels: [__meta_kubernetes_pod_label_component]
    action: replace
    target_label: job
  - source_labels: [__meta_kubernetes_pod_name]
    action: replace
    target_label: kubernetes_pod_name
```

`/terraform/kubernetes/prometheus.tf`

```kotlin
resource "kubernetes_namespace" "prometheus" {
  metadata {
    name = "prometheus"
  }
}

resource "kubernetes_config_map" "prometheus_config" {
  metadata {
    name      = "prometheus-config"
    namespace = "prometheus"
  }

  data = {
    "prometheus.yml" = file("${path.module}/files/prometheus_config_map.yaml")
  }
  depends_on = [
    kubernetes_namespace.prometheus
  ]
}

# Values documentation: https://github.com/bitnami/charts/blob/master/bitnami/kube-prometheus/values.yaml
resource "helm_release" "prometheus" {
  name        = "prometheus"
  repository  = local.helm_repositories.bitnami
  chart       = "kube-prometheus"
  version     = "3.4.0"
  namespace   = "prometheus"
  atomic      = true
  max_history = 5

  values = [
    file("${path.module}/helm_values/security.yaml.tpl"),
    file("${path.module}/helm_values/prometheus.yaml")
  ]

  depends_on = [
    kubernetes_config_map.prometheus_config
  ]
}
```

`/terraform/kubernetes/helm_values/prometheus.yaml`

```shell
prometheus:
  podMetadata:
    annotations:
      container.apparmor.security.beta.kubernetes.io/prometheus-operator: runtime/default
      seccomp.security.alpha.kubernetes.io/pod: runtime/default

nodeAffinityPreset:
  ## Node affinity type
  ## Allowed values: soft, hard
  ##
  type: "hard"
  ## Node label key to match
  ## E.g.
  ## key: "kubernetes.io/e2e-az-name"
  ##
  key: "cloud.google.com/gke-nodepool"
  ## Node label values to match
  ## E.g.
  ## values:
  ##   - e2e-az1
  ##   - e2e-az2
  ##
  values: [
    "project-primary-pool"
  ]

prometheus:  
  configMaps:
    - prometheus-config
```

In this file `/terraform/kubernetes/helm_values/prometheus.yaml`, I tried to remove the last `prometheus:` and moved `configMaps:` to the root as per this [documentation](https://github.com/bitnami/charts/blob/master/bitnami/kube-prometheus/values.yaml) but it broke the Prometheus configuration. As you can see in the file `/terraform/kubernetes/files/prometheus_config_map.yaml`, the `scrape_interval` is `15s` but when I check on Prometheus UI config, all of the scrape_interval is 30s, therefore, for sure the configuration of deployed Prometheus doesn't come from this file. Therefore, I have no way to change the configuration such as scrape_interval

[![promConfig1](https://i.stack.imgur.com/Fsm9v.png)](https://i.stack.imgur.com/Fsm9v.png)

I also notice in this [documentation](https://github.com/bitnami/charts/blob/master/bitnami/kube-prometheus/values.yaml) that I am missing many configuration in the file `/terraform/kubernetes/helm_values/prometheus.yaml` but I am not certain of what to add. Could you please let me know how I can resolve this? Thank you in advance.

[![promCONFIGmount](https://i.stack.imgur.com/y9sNE.png)](https://i.stack.imgur.com/y9sNE.png)

[terraform](https://stackoverflow.com/questions/tagged/terraform "show questions tagged 'terraform'")[prometheus](https://stackoverflow.com/questions/tagged/prometheus "show questions tagged 'prometheus'")[configmap](https://stackoverflow.com/questions/tagged/configmap "show questions tagged 'configmap'")

[Share](https://stackoverflow.com/q/67567449/14867493 "Short permalink to this question")

Edit

Follow

[edited May 17 '21 at 15:16](https://stackoverflow.com/posts/67567449/revisions "show all edits to this post")

asked May 17 '21 at 9:51

[

![](https://www.gravatar.com/avatar/223a3c65e930b5e2644c9b3e808cecae?s=64&d=identicon&r=PG&f=1)

](https://stackoverflow.com/users/15450599/shuti)

[shuti](https://stackoverflow.com/users/15450599/shuti)

1111313 bronze badges

[Add a comment](https://stackoverflow.com/questions/67567449/prometheus-configuration-and-deployment-in-terraform# "Use comments to ask for more information or suggest improvements. Avoid answering questions in comments.")

## 1 Answer

[Active](https://stackoverflow.com/questions/67567449/prometheus-configuration-and-deployment-in-terraform?answertab=active#tab-top "Answers with the latest activity first")[Oldest](https://stackoverflow.com/questions/67567449/prometheus-configuration-and-deployment-in-terraform?answertab=oldest#tab-top "Answers in the order they were provided")[Score](https://stackoverflow.com/questions/67567449/prometheus-configuration-and-deployment-in-terraform?answertab=votes#tab-top "Answers with the highest score first")

1

[](https://stackoverflow.com/posts/67567862/timeline)

in values.yaml if you check the line : 577

[https://github.com/bitnami/charts/blob/master/bitnami/kube-prometheus/values.yaml](https://github.com/bitnami/charts/blob/master/bitnami/kube-prometheus/values.yaml)

there is an option to mount the config map to the deployment.

> ## ConfigMaps that should be mounted into the Prometheus Pods
> 
> configMaps: []

you can set the config map to the deployment and change the time of interval.

if it's not using the config map values it must be using the default config from volume or directly you can go inside the POD and check that once also to verify.

[Share](https://stackoverflow.com/a/67567862/14867493 "Short permalink to this answer")

Edit

Follow

answered May 17 '21 at 10:22

[

![](https://i.stack.imgur.com/JVnxp.jpg?s=64&g=1)

](https://stackoverflow.com/users/5525824/harsh-manvar)

[Harsh Manvar](https://stackoverflow.com/users/5525824/harsh-manvar)

15.1k22 gold badges2929 silver badges6868 bronze badges

-   1
    
    Thank you @Harsh Manvar for your reply. I did check the deployment after and the Mount seems to be empty, as its `None` [PromConfig](https://i.stack.imgur.com/y9sNE.png) this is not what we expect right? However, I do have configMap section if you see my file above, can you suggest me what I missed here? 
    
    – [shuti](https://stackoverflow.com/users/15450599/shuti "111 reputation")
    
     [May 17 '21 at 15:17](https://stackoverflow.com/questions/67567449/prometheus-configuration-and-deployment-in-terraform#comment119436882_67567862)
    
-   @shuti check if confimap getting appiled to the deployment of prom or not, as scrape config is stored inside the configmap so or else you should try adding or updating default scrape config file in volume to verify which file is getting load. 
    
    – [Harsh Manvar](https://stackoverflow.com/users/5525824/harsh-manvar "15,084 reputation")
    
     [May 18 '21 at 10:23](https://stackoverflow.com/questions/67567449/prometheus-configuration-and-deployment-in-terraform#comment119458161_67567862)