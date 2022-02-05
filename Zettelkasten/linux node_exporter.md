[[wget https://github.com/prometheus/node_exporter/releases/download/v0.17.0/node_exporter-0.17.0.linux-amd64.tar.gz]]

Extract the downloaded package.

tar -xvzf node_exporter-0.17.0.linux-amd64.tar.gz

Create a user for the node exporter.

useradd -rs /bin/false nodeusr

Move binary to “/usr/local/bin” from the downloaded extracted package.

mv node_exporter-0.17.0.linux-amd64/node_exporter /usr/local/bin/

Create a service file for the node exporter.

vim /etc/systemd/system/node_exporter.service

Add the following content to the file.

[Unit]
Description=Node Exporter
After=network.target

[Service]
User=nodeusr
Group=nodeusr
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target

Save and exit the file.

Reload the system daemon.

systemctl daemon-reload

Start node exporter service.

systemctl start node_exporter

Add a firewall rule to allow node exporter.

 firewall-cmd --zone=public --add-port=9100/tcp --permanent

Reload firewall service.

systemctl restart firewalld

Enable node exporter on system boot.

systemctl enable node_exporter

View the metrics browsing node exporter URL.

http://IP-Address:9100/metrics

![Node Exporter Metrics](https://786647.smushcdn.com/1490832/wp-content/uploads/2019/04/Node_Exporter_Metrics.png?lossy=1&strip=1&webp=1)

Node Exporter Metrics

Add configured node exporter Target On Prometheus Server.

Login to Prometheus server and modify the prometheus.yml file

Edit the file:

vim /etc/prometheus/prometheus.yml

Add the following configurations under the scrape config.

 - job_name: 'node_exporter_centos'
    scrape_interval: 5s
    static_configs:
      - targets: ['10.94.10.209:9100']

The file should look like as follows.

![Modified File](https://786647.smushcdn.com/1490832/wp-content/uploads/2019/04/Modified_File.png?lossy=1&strip=1&webp=1)

Modified File

Restart Prometheus service.

systemctl restart prometheus

Login to Prometheus server web interface, and check targets.

http://Prometheus-Server-IP:9090/targets

