[![Promcat logo](https://promcat.io/_nuxt/606d7b49bdaedf29a70794c509c85d92.svg)](https://promcat.io/)

Search

FILTER

A PROJECT BY

[SYSDIG](https://sysdig.com/)

![Windows](https://upload.wikimedia.org/wikipedia/commons/8/8d/Windows_darkblue_2012.svg)

## SUPPORTED VERSIONS

Version 2019

-   [DESCRIPTION](https://promcat.io/apps/windows#)
-   [SETUP GUIDE](https://promcat.io/apps/windows#)
-   [DASHBOARDS](https://promcat.io/apps/windows#)
-   [ALERTS](https://promcat.io/apps/windows#)

## INSTALLING THE EXPORTER

You can use the installer as a standalone application or install it in your system.

To install the Windows exporter download the `msi` binary file from the [release page](https://github.com/prometheus-community/windows_exporter/releases) of the Windows exporter repository. Run the installer to install the exporter. This will configure the Windows Exporter as a Windows service and create an exception in the Windows firewall.

To execute it as a standalone application download the exe file from the [release page](https://github.com/prometheus-community/windows_exporter/releases) of the Windows exporter and execute it.

To disable the process collector to prevent high cardinality problems, you can install or execute the exporter with the following options.

```
# Installer:
msiexec /i windows_exporter.msi --collectors.enabled "cpu,cs,logical_disk,os,system,net"

# Stand alone:
.\windows_exporter.exe --collectors.enabled "cpu,cs,logical_disk,os,system,net"
```

## CONFIGURING SYSDIG AGENT

To allow the Sysdig agent to scrape a remote endpoint from one of the agents, you have to annotate the node that will host the scraping agent:

```
kubectl annotate nodes NODEID sysdig.com/scraper=true
```

A Sysdig agent configuration is given below. Ensure that you change the line with the IP and PORT of your Windows instance:

> url: "http://IP:PORT/metrics"

To apply, save the file as `dragent.yaml` and apply:

```
kubectl apply -f dragent.yaml
```

## INSTALLING DASHBOARDS AND ALERTS IN SYSDIG MONITOR:

_Note: This instructions are only for on-prem customers and legacy environments. All these dashboards and alerts are available out-of-the-box in Sysdig monitor platform. Here is the list of [all the dashboards included in the platform](https://docs.sysdig.com/en/docs/sysdig-monitor/dashboards/dashboard-templates/)._

Run this command using your API-TOKEN (-u argument is optional):

```bash
docker  run -it --rm \
    sysdiglabs/promcat-connect:0.1 \
    install \
    windows:2019 \
    -t YOUR-API-TOKEN \
    -u https://app.sysdigcloud.com/ 
```

## SETUP FILES

[dragent.yaml](data:text/plain;charset=utf-8,apiVersion%3A%20v1%0Adata%3A%0A%20%20dragent.yaml%3A%20%7C%0A%20%20%20%20new_k8s%3A%20true%0A%20%20%20%20k8s_cluster_name%3A%20cluster_name%0A%20%20%20%20jmx%3A%0A%20%20%20%20%20%20enabled%3A%20false%0A%20%20%20%20metrics_excess_log%3A%20true%0A%20%20%20%20app_checks_enabled%3A%20false%0A%20%20%20%2010s_flush_enable%3A%20true%0A%20%20%20%20use_promscrape%3A%20true%0A%20%20%20%20prometheus%3A%0A%20%20%20%20%20%20enabled%3A%20true%0A%20%20%20%20%20%20histograms%3A%20true%0A%20%20%20%20%20%20ingest_raw%3A%20true%0A%20%20%20%20%20%20ingest_calculated%3A%20false%0A%20%20%20%20%20%20interval%3A%2060%0A%20%20%20%20%20%20timeout%3A%2030%0A%20%20%20%20%20%20log_errors%3A%20true%0A%20%20%20%20%20%20max_metrics%3A%203000%0A%20%20%20%20%20%20max_metrics_per_process%3A%2020000%0A%20%20%20%20%20%20remote_services%3A%0A%20%20%20%20%20%20%20%20-%20wmi%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20kubernetes.node.annotation.sysdig.com%2Fscraper%3A%20true%0A%20%20%20%20%20%20%20%20%20%20%20%20conf%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20url%3A%20%22http%3A%2F%2FIP%3APORT%2Fmetrics%22%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20tags%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20scraping_node%3A%20%22%7Bkubernetes.node.name%7D%22%0A%20%20%20%20%20%20process_filter%3A%0A%20%20%20%20%20%20-%20include%3A%0A%20%20%20%20%20%20%20%20%20%20kubernetes.pod.annotation.prometheus.io%2Fscrape%3A%20true%0A%20%20%20%20%20%20%20%20%20%20conf%3A%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20path%3A%20%22%7Bkubernetes.pod.annotation.prometheus.io%2Fpath%7D%22%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20port%3A%20%22%7Bkubernetes.pod.annotation.prometheus.io%2Fport%7D%22%0A%20%20%20%20snaplen%3A%20512%0A%20%20%20%20tags%3A%20role%3Acluster%0Akind%3A%20ConfigMap%0Ametadata%3A%0A%20%20%20%20labels%3A%0A%20%20%20%20%20%20app%3A%20sysdig-agent%0A%20%20%20%20name%3A%20sysdig-agent%0A%20%20%20%20namespace%3A%20sysdig-agent)

## SETUP DETAILS

-   [dragent.yaml](https://promcat.io/apps/windows#)

Copy Code`apiVersion: v1
data:
  dragent.yaml: | new_k8s: true
    k8s_cluster_name: cluster_name
    jmx:
      enabled: false
    metrics_excess_log: true
    app_checks_enabled: false
    10s_flush_enable: true
    use_promscrape: true
    prometheus:
      enabled: true
      histograms: true
      ingest_raw: true
      ingest_calculated: false
      interval: 60
      timeout: 30
      log_errors: true
      max_metrics: 3000
      max_metrics_per_process: 20000
      remote_services:
        - wmi:
            kubernetes.node.annotation.sysdig.com/scraper: true
            conf:
              url: "http://IP:PORT/metrics"
              tags:
                scraping_node: "{kubernetes.node.name}"
      process_filter:
      - include:
          kubernetes.pod.annotation.prometheus.io/scrape: true
          conf:
              path: "{kubernetes.pod.annotation.prometheus.io/path}"
              port: "{kubernetes.pod.annotation.prometheus.io/port}"
    snaplen: 512
    tags: role:cluster
kind: ConfigMap
metadata:
    labels:
      app: sysdig-agent
    name: sysdig-agent
    namespace: sysdig-agent`

[](https://promcat.io/)

[What is PromCat?](https://promcat.io/about)

-    [GitHub](https://github.com/sysdiglabs/promcat-resources)
 -    [Join us on Slack](https://slack.sysdig.com/)
 -    [File an issue](https://github.com/sysdiglabs/promcat-resources/issues)

© Copyright 2022 Sysdig, Inc. All Rights Reserved.

A PROJECT BY [SYSDIG](https://sysdig.com/)