### Enable the stub status module

  
[http://nginx.org/en/docs/http/ngx_http_stub_status_module.html](http://nginx.org/en/docs/http/ngx_http_stub_status_module.html)

Example Configuration below. This will create a HTTP server on port 81 only available from localhost. This goes inside the  **http {}** section

```nginx
server { 
		listen localhost:81;
		location /metrics {
			stub_status on;
		}
}
```

Copy

/etc/nginx/nginx.conf

Make sure to restart the NGINX service

```bash
service nginx restart
```

Copy

This is what the /metrics endpoint will look like

```bash
curl localhost:81/metrics
```

Copy

```
Active connections: 2 
server accepts handled requests
 13 13 25 
Reading: 0 Writing: 1 Waiting: 1
```

### Install the NGINX Prometheus Exporter from [GitHub](https://github.com/nginxinc/nginx-prometheus-exporter)

[

nginxinc/nginx-prometheus-exporter

NGINX Prometheus Exporter for NGINX and NGINX Plus - nginxinc/nginx-prometheus-exporter

![](https://github.githubassets.com/favicon.ico)GitHubnginxinc

![](https://avatars0.githubusercontent.com/u/8629072?s=400&v=4)

](https://github.com/nginxinc/nginx-prometheus-exporter)

```bash
cd /tmp

wget https://github.com/nginxinc/nginx-prometheus-exporter/releases/download/v0.7.0/nginx-prometheus-exporter-0.7.0-linux-amd64.tar.gz

tar -xf nginx-prometheus-exporter-0.7.0-linux-amd64.tar.gz

mv nginx-prometheus-exporter /usr/local/bin

useradd -r nginx_exporter

# Create Systemd Service File

nano /etc/systemd/system/nginx_prometheus_exporter.service

systemctl daemon-reload

service nginx_prometheus_exporter status
service nginx_prometheus_exporter start
```

Copy

```
[Unit]
Description=NGINX Prometheus Exporter
After=network.target

[Service]
Type=simple
User=nginx_exporter
Group=nginx_exporter
ExecStart=/usr/local/bin/nginx-prometheus-exporter \
    -web.listen-address=private_IP_here:9113 \
    -nginx.scrape-uri http://127.0.0.1:81/metrics

SyslogIdentifier=nginx_prometheus_exporter
Restart=always

[Install]
WantedBy=multi-user.target
```

service example

Once the exporter has started you should be able to CURL the exporter and the output should look like this.

```promql
# HELP nginx_connections_accepted Accepted client connections
# TYPE nginx_connections_accepted counter
nginx_connections_accepted 22
# HELP nginx_connections_active Active client connections
# TYPE nginx_connections_active gauge
nginx_connections_active 2
# HELP nginx_connections_handled Handled client connections
# TYPE nginx_connections_handled counter
nginx_connections_handled 22
# HELP nginx_connections_reading Connections where NGINX is reading the request header
# TYPE nginx_connections_reading gauge
nginx_connections_reading 0
# HELP nginx_connections_waiting Idle client connections
# TYPE nginx_connections_waiting gauge
nginx_connections_waiting 1
# HELP nginx_connections_writing Connections where NGINX is writing the response back to the client
# TYPE nginx_connections_writing gauge
nginx_connections_writing 1
# HELP nginx_http_requests_total Total http requests
# TYPE nginx_http_requests_total counter
nginx_http_requests_total 82
# HELP nginx_up Status of the last metric scrape
# TYPE nginx_up gauge
nginx_up 1
# HELP nginxexporter_build_info Exporter build information
# TYPE nginxexporter_build_info gauge
nginxexporter_build_info{gitCommit="c3dd65c",version="0.5.0"} 1
```

Copy

If you have ufw enabled you have to allow tcp port 9113

```bash
ufw allow proto tcp from any to any port 9113
```

Copy

Now just tell prometheus to scrape the exporter!

```yaml
  - job_name: 'nginx'
    static_configs:
    - targets: ['0.0.0.0:9113']
```

Copy

Replace 0.0.0.0 with the IP of the web.listen-address