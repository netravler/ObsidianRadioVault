# This is for setting up on a Windows Server

Install the following, order generally not of importance.

- Prometheus https://prometheus.io/docs/introduction/first_steps/ 
- Grafana https://grafana.com/grafana/download/6.0.0-beta1?platform=windows
- Nginx http://nginx.org/en/download.html
- Nginx-exporter https://github.com/nginxinc/nginx-prometheus-exporter/releases
- windows-exporter https://community.chocolatey.org/packages/prometheus-windows-exporter.install
- Jenkins https://www.jenkins.io/doc/book/installing/windows/
- any other applications that are to be assessed via Prometheus/Grafana

### Prometheus command:

```

c:\<file location>\prometheus.exe

```

### Example Prometheus YML:

```

Global:

  scrape_interval:     5s # By default, scrape targets every 15 seconds.

  

  # Attach these labels to any time series or alerts when communicating with

  # external systems (federation, remote storage, Alertmanager).

  external_labels:

    monitor: 'codelab-monitor'

  

# A scrape configuration containing exactly one endpoint to scrape:

# Here it's Prometheus itself.

scrape_configs:

  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.

  - job_name: 'prometheus'

  

    metrics_path: /metrics

  

    # Override the global default and scrape targets from this job every 5 seconds.

    scrape_interval: 5s

  

    static_configs:

      - targets: ['localhost:9090', 'localhost:9182']

  

  - job_name: windows_exporter

  

    scrape_interval: 5s

  

    static_configs:

      - targets: ['localhost:9182']

  

  - job_name: 'jenkins'

  

    scrape_interval: 5s

  

    metrics_path: /prometheus

  

    static_configs:

      - targets: ['localhost:8888']

  

  - job_name: 'nginx'

  

    scrape_interval: 5s

    metrics_path: /metrics

  

    static_configs:

      - targets: ['20.98.59.194:9113']

```

### NGINX command:

```

c:\<file location>\nginx.exe

```


### NGINX_Exporter command:

```
c:\<file location>\nginx-prometheus-exporter -nginx.scrape-url=http://20.98.59.194:8088/metrics

```

### WINDOWS_Exporter command:

```

c:\<file location>\windows_exporter>msiexec /i windows_exporter.msi --collectors.enable "cpu,cs,logical_disk,os,system,net"

```

Once the above has been begun or completed you may need to adjust firewalls and/or the NSG if running from Azure.

Please note you will need to assign/verify ports.  Pay particular attention to existing service ports as to not create conflicts.

This is a live document and as such should be reviewed on a periodic basis.

Kind regards,

Paul Heintz