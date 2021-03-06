# How to Install Prometheus on Ubuntu 20.04

By [Jamie Arthur](https://linoxide.com/author/arthurj/ "View all posts by Jamie Arthur")

 | Updated November 5, 2021 | [Tutorials](https://linoxide.com/category/tutorials/)

![](https://linoxide.com/wp-content/uploads/2021/11/install-prometheus-ubuntu-featured-image.png)

[Prometheus](https://prometheus.io/docs/introduction/overview/) is a free and open-source monitoring and alerting tool that was initially used for monitoring metrics at SoundCloud back in 2012. It is written in Go programming language.

Prometheus monitors and records real-time events in a time-series database. Since then it has grown in leaps and bounds and had been adopted by many organizations to monitor their infrastructure metrics. Prometheus provides flexible queries and real-time alerting which helps in quick diagnosis and troubleshooting of errors.

Prometheus comprises the following major components:

-   The main Prometheus server for scraping and storing time-series data.
-   Unique exporters for services such as Graphite, HAProxy, StatsD and so much more
-   An alert manager for handling alerts
-   A push-gateway for supporting transient jobs
-   Client libraries for instrumenting application code

In this tutorial, we learn **how to install Prometheus** on **Ubuntu 20.04**.

## What you need:

The following are the minimum requirements that you need to have before getting started out:

-   An instance of an Ubuntu server with a configured sudo user
-   2 GB RAM and 1 vCPU
-   SSH access to the server

## Step 1: Update the system

Start off by updating the package lists as follows:

```
$ sudo apt update
```

Once the package index is refreshed and up to date, head over to the next step.

## Step 2: Download and install Prometheus

Prometheus installation files come in precompiled binaries in compressed tarball or zipped files. To download Prometheus, head over to the [official download page](https://prometheus.io/download/). At the moment of writing this guide, the latest version of Prometheus is 2.31.0.

161.1K

Amazon downplay reports that the Kindle is being discontinued

Next

Stay

But first, we need to create the configuration and data directories for Prometheus.

To create the configuration directory, run the command:

```
$ sudo mkdir -p /etc/prometheus
```

For the data directory, execute:

```
$ sudo mkdir -p /var/lib/prometheus
```

Once the directories are created, grab the compressed installation file:

```
$ wget https://github.com/prometheus/prometheus/releases/download/v2.31.0/prometheus-2.31.0.linux-amd64.tar.gz 
```

Once downloaded, extract the tarball file.

```
$  tar -xvf prometheus-2.31.3.linux-amd64.tar.gz
```

Then navigate to the Prometheus folder.

```
$ cd prometheus-2.31.3.linux-amd64
```

Once in the [directory move](https://linoxide.com/mv-command-in-linux/) the  `prometheus` and `promtool` binary files to `/usr/local/bin/` folder.

```
$ sudo mv prometheus promtool /usr/local/bin/
```

Additionally, move console files in `console` directory and library files in the `console_libraries`  directory to `/etc/prometheus/` directory.

```
$ sudo mv consoles/ console_libraries/ /etc/prometheus/
```

Also, ensure to move the prometheus.yml template configuration file to the  **`/etc/prometheus/`** directory.

```
$ sudo mv prometheus.yml /etc/prometheus/prometheus.yml
```

At this point, Prometheus has been successfully installed. To check the version of Prometheus installed, run the command:

```
$ prometheus --version
```

Output:

```
prometheus, version 2.31.3 (branch: HEAD, revision: f29caccc42557f6a8ec30ea9b3c8c089391bd5df)
build user: root@5cff4265f0e3
build date: 20211005-16:10:52
go version: go1.17.1
platform: linux/amd64
```

```
$ promtool --version
```

Output:

```
promtool, version 2.31.3 (branch: HEAD, revision: f29caccc42557f6a8ec30ea9b3c8c089391bd5df)
build user: root@5cff4265f0e3
build date: 20211005-16:10:52
go version: go1.17.1
platform: linux/amd64
```

If your output resembles what I have, then you are on the right track. In the next step, we will create a system group and user.

## Step 3: Configure System group and user

It's essential that we create a Prometheus group and user before proceeding to the next step which involves creating a system file for Prometheus.

To  create a `prometheus` [group](https://linoxide.com/groupadd-command/) execute the command:

```
$ sudo groupadd --system prometheus
```

Thereafter, Create `prometheus` user and assign it to the just-created `prometheus` group.

```
$ sudo useradd -s /sbin/nologin --system -g prometheus prometheus
```

Next, configure the directory ownership and permissions as follows.

```
$ sudo chown -R prometheus:prometheus /etc/prometheus/ /var/lib/prometheus/$ sudo chmod -R 775 /etc/prometheus/ /var/lib/prometheus/
```

The only part remaining is to make Prometheus a systemd service so that we can easily manage its running status.

## Step 4: Create a systemd file for Prometheus

Using your favorite text editor, create a systemd service file:

```
$ sudo vim /etc/systemd/system/prometheus.service
```

Paste the following lines of code.

```
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Restart=always
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file=/etc/prometheus/prometheus.yml \
    --storage.tsdb.path=/var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries \
    --web.listen-address=0.0.0.0:9090

[Install]
WantedBy=multi-user.target
```

Save the changes and exit the systemd file.

Then proceed and start the Prometheus service.

```
$ sudo systemctl start prometheus
```

Enable the Prometheus service to run at startup. Therefore invoke the command:

```
$ sudo systemctl enable prometheus
```

Then confirm the status of the Prometheus service.

```
$ sudo systemctl status prometheus
```

![Check status of Prometheus services](https://linoxide.com/wp-content/uploads/2021/11/2021-10-1003-Check-status-of-Prometheus-services.png)

## Step 5: Access Prometheus

Finally, to access Prometheus, launch your browser and visit your server's IP address followed by port 9090.

If you have a UFW firewall running, open the 9090 port:

```
$ sudo ufw allow 9090/tcp
```

```
$ sudo ufw reload
```

Back to your browser. Head over to the address indicated.

```
http://server-ip:9090
```

![prometheus dashboard](https://linoxide.com/wp-content/uploads/2021/11/2021-10-1003-Prometheus-dashboard-1024x440.png)

## Conclusion

In this tutorial, we learned how to install Prometheus on Ubuntu 20.04. From here, you can now start monitoring various events and time-based metrics. For additional information, head over to the [official Prometheus documentation](https://prometheus.io/docs/introduction/overview/).