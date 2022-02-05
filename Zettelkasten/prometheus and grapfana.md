# Windows Server Monitoring using Prometheus and WMI Exporter

written by [Schkn](https://devconnected.com/author/schkn/)

[![Windows Server Monitoring using Prometheus and WMI Exporter](https://devconnected.com/wp-content/uploads/2019/08/featured-image-10.png "featured image")](https://devconnected.com/wp-content/uploads/2019/08/featured-image-10.png)

If you are a **Windows system administrator**, or a **site reliability engineer**, you spend a lot of time monitoring your Windows servers.

Sometimes, your servers are down, but you can’t know why easily.

Is it because of a **high CPU usage** on one of the processes?

Is the server having some **memory issues**? Is the **RAM** used too much on my Windows server?

Today, after understanding how to do [Linux system monitoring](https://devconnected.com/complete-node-exporter-mastery-with-prometheus/), we are taking a look at how to configure your Windows Server monitoring properly.

For this tutorial, we are going to use **Prometheus**, a modern time series database and monitoring platform.

If you are not familiar with [Prometheus monitoring](https://devconnected.com/the-definitive-guide-to-prometheus-in-2019/), you can have a look at one of the guides we crafted for you.

> Ready to monitor your Windows servers?

Table of Contents

-   [I – What You Will Learn](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#I_What_You_Will_Learn "I – What You Will Learn")
-   [II – Windows Server Monitoring Architecture](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#II_Windows_Server_Monitoring_Architecture "II – Windows Server Monitoring Architecture")
-   [III – Installing Prometheus](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#III_Installing_Prometheus "III – Installing Prometheus")
-   [IV – Installing the WMI Exporter](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#IV_Installing_the_WMI_Exporter "IV – Installing the WMI Exporter")
    -   [a – Downloading the WMI Exporter MSI](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#a_Downloading_the_WMI_Exporter_MSI "a – Downloading the WMI Exporter MSI")
    -   [b – Running the WMI installer](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#b_Running_the_WMI_installer "b – Running the WMI installer")
    -   [c – Observing Windows Server metrics](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#c_Observing_Windows_Server_metrics "c – Observing Windows Server metrics")
    -   [d – Binding Prometheus to the WMI exporter](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#d_Binding_Prometheus_to_the_WMI_exporter "d – Binding Prometheus to the WMI exporter")
-   [V – Building an Awesome Grafana Dashboard](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#V_Building_an_Awesome_Grafana_Dashboard "V – Building an Awesome Grafana Dashboard")
    -   [a – Importing a Grafana dashboard](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#a_Importing_a_Grafana_dashboard "a – Importing a Grafana dashboard")
-   [VI – Raising alerts in Grafana on high CPU usage](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#VI_Raising_alerts_in_Grafana_on_high_CPU_usage "VI – Raising alerts in Grafana on high CPU usage")
    -   [a – Creating a Slack webhook](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#a_Creating_a_Slack_webhook "a – Creating a Slack webhook")
    -   [b – Set Slack as a Grafana notification channel](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#b_Set_Slack_as_a_Grafana_notification_channel "b – Set Slack as a Grafana notification channel")
    -   [c – Building a PromQL query](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#c_Building_a_PromQL_query "c – Building a PromQL query")
    -   [d – Creating a Grafana alert](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#d_Creating_a_Grafana_alert "d – Creating a Grafana alert")
-   [VII – Conclusion](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#VII_Conclusion "VII – Conclusion")

## I – What You Will Learn

If you follow this tutorial until the end, here are the key concepts you are going to learn about.

-   How to **install and configure Prometheus** on your Linux servers;
-   How to **download and install the WMI exporter** for Windows servers;
-   How to bind Prometheus to your WMI exporter;
-   How to **build an awesome Grafana dashboard** to visualize your metrics.

Quite a long program, let’s jump into it.

## II – Windows Server Monitoring Architecture

Before installing the WMI exporter, let’s have a quick look at what our final architecture looks like.

As a reminder, Prometheus is constantly scraping **targets**.

Targets are nodes that are exposing metrics on a given URL, accessible by Prometheus.

Such targets are equipped with “**exporters**” : exporters are binaries running on a target and responsible for getting and aggregating metrics about the host itself.

If you were to monitor a Linux system, you would run a “**Node Exporter**“, that would be responsible for gathering metrics about the CPU usage or the disk I/O currently in use.

For Windows hosts, you are going to use the **WMI exporter**.

The WMI exporter will run as a Windows service and it will be responsible for gathering metrics about your system.

In short, here is the final architecture that you are going to build.

![Windows server monitoring architecture](https://devconnected.com/wp-content/uploads/2019/08/windows-arch.png)

## III – Installing Prometheus

The complete [Prometheus installation for Linux](https://devconnected.com/how-to-setup-grafana-and-prometheus-on-linux/) was already covered in one of our previous article.

Make sure to read it extensively to have your Prometheus instance up and running.

To verify it, head over to [http://localhost:9090](http://localhost:9090/) (9090 being the default Prometheus port).

You should see a Web Interface similar to this one.

If this is the case, it means that your Prometheus installation was successful.

![Prometheus default Web UI](https://devconnected.com/wp-content/uploads/2019/08/prometheus-homepage.png)

**Great!**

Now that your Prometheus is running, let’s install the WMI exporter on your Windows Server.

## IV – Installing the WMI Exporter

The WMI exporter is an awesome exporter for Windows Servers.

It will export metrics such as the **CPU usage, the memory and the disk I/O usage**.

The WMI exporter can also be used to monitor IIS sites and applications, the network interfaces, the services and even the local temperature!

If you want a complete look of everything that the WMI exporter offers, have a look at [all the collectors available](https://github.com/martinlindhe/wmi_exporter#collectors).

In order to install the WMI exporter, head over to the [WMI releases page](https://github.com/martinlindhe/wmi_exporter/releases) on GitHub.

### a – Downloading the WMI Exporter MSI

As of August 2019, the latest version of the WMI exporter is 0.8.1.

![WMI exporter GitHub page](https://devconnected.com/wp-content/uploads/2019/08/wmi-exporter-v0.8.1.png)

On the releases page, download the MSI file corresponding to your CPU architecture.

In my case, I am going to download the [wmi_exporter-0.8.1-amd64.msi](https://github.com/martinlindhe/wmi_exporter/releases/download/v0.8.1/wmi_exporter-0.8.1-amd64.msi) file.

### b – Running the WMI installer

When the download is done, simply click on the MSI file and start running the installer.

This is what you should see on your screen.

![Running the WMI exporter on Windows](https://devconnected.com/wp-content/uploads/2019/08/wmi-exporter-msi.png)

Windows should now start configuring your WMI exporter.

![WMI exporter configuration on Windows](https://devconnected.com/wp-content/uploads/2019/08/configuring-the-wmi-exporter.png)

You should be prompted with a firewall exception. Make sure to accept it for the WMI exporter to run properly.

The MSI installation should exit without any confirmation box. However, the WMI exporter should now run as a **Windows service on your host.**

To verify it, head over to the **Services panel** of Windows (by typing Services in the Windows search menu).

In the Services panel, search for the “**WMI exporter**” entry in the list. Make sure that your service is running properly.

![WMI exporter running as a service](https://devconnected.com/wp-content/uploads/2019/08/wmi-exporter-service.png)

### c – Observing Windows Server metrics

Now that your exporter is running, it should start exposing metrics on [http://localhost:9182/metrics](http://localhost:9182/)

Open your web browser and navigate to the WMI exporter URL. This is what you should see in your web browser.

Some metrics are very general and exported by all the exporters, but some of the metrics are very specific to your Windows host (like the **wmi_cpu_core_frequency_mhz metric** for example)

![Windows server monitoring metrics](https://devconnected.com/wp-content/uploads/2019/08/prom-metrics.png)

Great!

Windows Server monitoring is now active using the WMI exporter.

If you remember correctly, Prometheus scrapes targets.

As a consequence, we have to configure **our Windows Server as a Prometheus target.**

This is done in Prometheus configuration file.

### d – Binding Prometheus to the WMI exporter

As you probably saw from your web browser request, the WMI exporter exports a lot of metrics.

As a consequence, there is a chance that the scrape request times out when trying to get the metrics.

This is why we are going to set a high scrape timeout in our configuration file.

If you want to keep a low scrape timeout, make sure to configure the WMI exporter to export less metrics (by specifying just a few collectors for example).

Head over to your configuration file (mine is located at **/etc/prometheus/prometheus.yml**) and edit the following changes to your file.

```
scrape_configs:
# The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # Careful, the scrape timeout has to be lower than the scrape interval.
    scrape_interval: 6s
    scrape_timeout: 5s
    static_configs:
            - targets: ['localhost:9090', 'localhost:9216']
```

Save your file, and restart your Prometheus service.

```
$ sudo systemctl restart prometheus
$ sudo systemctl status prometheus
```

Head back to the Prometheus UI, and select the “**Targets**” tab to make sure that Prometheus is correctly connected to the WMI exporter.

![Windows server monitoring target in Prometheus](https://devconnected.com/wp-content/uploads/2019/08/wmi-target.png)

If you are getting the following error, “context deadline exceeded”, make sure that the scrape timeout is set in your configuration file.

**Great**! Our Windows Server monitoring is almost ready.

Now it is time for us to start building an awesome Grafana dashboard to monitor our Windows Server.

## V – Building an Awesome Grafana Dashboard

The [Prometheus & Grafana installation](https://devconnected.com/how-to-setup-grafana-and-prometheus-on-linux/) was already covered in our previous guides. Make sure to configure your Grafana properly before moving to the next section.

If you are looking to [install Grafana](https://devconnected.com/how-to-install-grafana-on-windows-8-10/) on Windows, here is another guide for it.

Prometheus should be configured as a Grafana target, and accessible through your reverse proxy.

### a – Importing a Grafana dashboard

In Grafana, you can either create your own dashboards or you can use pre-existing ones that contributors already crafted for you.

In our case, we are going to use the [Windows Node](https://grafana.com/grafana/dashboards/2129) dashboard, accessible via the **2129 ID.**

Head over to the main page of Grafana (located at http://localhost:3000 by default), and click on the **Import option in the left menu.**

![Import option in Grafana](https://devconnected.com/wp-content/uploads/2019/08/import-grafana.png)

In the next window, simply insert the dashboard ID in the corresponding text field.

![Importing a Grafana dashboard with its ID](https://devconnected.com/wp-content/uploads/2019/08/import-dash-1.png)

From there, Grafana should **automatically detect your dashboard as the Windows Node dashboard**. This is what you should see.

![Importing the Windows Node dashboard in Grafana](https://devconnected.com/wp-content/uploads/2019/08/windows-node-dashboard.png)

Select your Prometheus datasource in the “Prometheus” dropdown, and click on “Import” for the dashboard to be imported.

![Final Windows Server monitoring dashboard](https://devconnected.com/wp-content/uploads/2019/08/final-dashboard-2-1.png)

Awesome!

An entire dashboard displaying **Windows metrics** was created for us in just one click.

As you can see, the dashboard is pretty exhaustive.

You can monitor the **current CPU load**, but also the **number of threads** created by the system, and even the number of system exceptions dispatched.

![CPU load monitored in our Windows server](https://devconnected.com/wp-content/uploads/2019/08/cpu-load.png)

On the second line, you have access to metrics related to the network monitoring. You can for example have a look at the number of packets sent versus the number of packets received by your network card.

It can be useful to track anomalies on your network, in case of TCP flood attacks on your servers for example.

![Network monitoring for Windows server](https://devconnected.com/wp-content/uploads/2019/08/network-1.png)

On the third line, you have metrics related to the disk I/O usage on your computer.

Those metrics can be very useful when you are trying to debug applications (for example ASP.NET applications). Using those metrics, you are able to see if your application consume too much memory or too much disk.

![Hard drive monitoring on Windows Server](https://devconnected.com/wp-content/uploads/2019/08/disk-io-1.png)

Finally, one of the greatest panels has to be the memory monitoring. RAM has a very big influence on the overall system performance.

As a consequence, it has to be monitored properly, and this is exactly what the fourth line of the dashboard does.

![Memory usage monitoring on Windows Server](https://devconnected.com/wp-content/uploads/2019/08/memory-1.png)

That’s an awesome dashboard, but what if we want to be alerted whenever the CPU usage is too high for example?

Wouldn’t it be useful for our DevOps teams to know about it in order to see what’s causing the outage on the machine?

This is what we are going to do in the next section.

## VI – Raising alerts in Grafana on high CPU usage

As discussed in the previous section, you want alerts to be raised when the CPU usage is too high.

Grafana is equipped with an alerting system, meaning that whenever a panel raises an alert it will propagate the alert to “**notification channels**“.

Notification channels are Slack, your internal mailing system of PagerDuty for example.

In this case, we are going to use **Slack** as it is a pretty common team productivity tool used in companies.

### a – Creating a Slack webhook

For those who are not familiar with Slack, you can create webhooks which are essentially addresses for external sources to reach Slack.

As a consequence, Grafana is going to post the alert to the Webhook address, and it will be displayed in your Slack channel.

To create a Slack webhook, head over to your [Slack apps page](https://api.slack.com/apps).

![Your Apps page in Slack](https://devconnected.com/wp-content/uploads/2019/08/slack-apps-1.png)

Click on the name of your app (“devconnected” here). On the left menu, click on “Incoming Webhooks”.

![Incoming Webhooks in Slack](https://devconnected.com/wp-content/uploads/2019/08/slack-apps-2.png)

On the next page, simply click on “**Add New Webhook to Workspace**“.

![Adding a webhook to Slack](https://devconnected.com/wp-content/uploads/2019/08/slack-apps-3.png)

On the next screen, choose where you want your alert messages to be sent. In this case, I will choose the main channel of my Slack account.

![Allowing new messages on a Slack channel](https://devconnected.com/wp-content/uploads/2019/08/slack-apps-4.png)

Click on “Allow”. From there, your Slack Webhook URL should be created.

![Webhook created for Grafana](https://devconnected.com/wp-content/uploads/2019/08/webhook-URL.png)

### b – Set Slack as a Grafana notification channel

Copy the Webhook URL and head over to the Notifications Channels window of Grafana. This option is located in the left menu.

![Notification channels windows in Grafana](https://devconnected.com/wp-content/uploads/2019/08/notif-channel.png)

Click on the option, and you should be redirected to the following window.

![Add a notification channel in Grafana](https://devconnected.com/wp-content/uploads/2019/08/add-channel.png)

Click on “Add channel”. You should be redirected to the notification channel configuration page.

Copy the following configuration, and change the webhook URL with the one you were provided with in the last step.

![Creating a new notification channel in Grafana](https://devconnected.com/wp-content/uploads/2019/08/slack-config.png)

When your configuration is done, simply click on “**Send Test**” to send a test notification to your Slack channel.

![Test notification from Grafana on a Slack channel](https://devconnected.com/wp-content/uploads/2019/08/test-notif.png)

**Great! Your notification channel is working properly.**

Let’s create a PromQL query to monitor our CPU usage.

### c – Building a PromQL query

If you are not familiar with PromQL, there is a section dedicated to this language in my [Prometheus monitoring tutorial](https://devconnected.com/the-definitive-guide-to-prometheus-in-2019/).

When talking a look at our **CPU usage panel**, this is the PromQL query used to display the CPU graph.

![PromQL query in Grafana for CPU load](https://devconnected.com/wp-content/uploads/2019/08/promQL-1.png)

First, the query splits the results by the mode (idle, user, interrupt, dpc, privileged). Then, the query computes the average CPU usage for a five minutes period, for every single mode.

In the end, the modes are displayed with aggregated sums.

In my case, my CPU has 8 cores, so the overall usage sums up to 8 in the graph.

![Eight cores on a CPU](https://devconnected.com/wp-content/uploads/2019/08/8-cpus.png)

If I want to be notified when my CPU usage peaks at 50%, you essentially want to trigger an alert when the idle state goes below 4 (as 4 cores are going to be fully used).

To monitor our CPU usage, we are going to use this query

```
sum by (mode) (rate(wmi_cpu_time_total{instance=~"localhost:9182", mode="idle"}[5m]))
```

I am not using a template variable here for the instance as they are not supported by Grafana for the moment.

This query is very similar to the one already implemented in the panel, but it specifies that we specifically want to target the “idle” mode of our CPU.

Replace the existing query with the query we just wrote.

This is what you should now have in your dashboard.

![CPU load monitoring on a Windows server](https://devconnected.com/wp-content/uploads/2019/08/cpu-usage-2.png)

Now that your query is all set, let’s build an alert for it.

### d – Creating a Grafana alert

In order to create a Grafana alert, click on the bell icon located right under the query panel.

![Alert panel in Grafana](https://devconnected.com/wp-content/uploads/2019/08/alert.png)

In the rule panel, you are going to configure the following alert.

Every 10 seconds, Grafana will check if the average CPU usage for the last 10 seconds was below 4 (i.e using more than 50% of our CPU).

If it is the case, an alert will be sent to Slack, otherwise nothing happens.

![Creating a rule in Grafana](https://devconnected.com/wp-content/uploads/2019/08/rule-1.png)

Finally, right below this rule panel, you are going to configure the Slack notification channel.

![Sending the Grafana alerts to a Slack channel](https://devconnected.com/wp-content/uploads/2019/08/notifications-rule.png)

Now let’s try to bump the CPU usage on our instance.

![Grafana alert triggering](https://devconnected.com/wp-content/uploads/2019/08/alertalert.png)

As the CPU usage goes below the 4 threshold, it should set the panel state to “Alerting” (or “Pending” if you specified a “For” option that is too long).

From there, you should receive an alert in Slack.

![Grafana alert in a Slack channel](https://devconnected.com/wp-content/uploads/2019/08/slack-alert.png)

As you can see, there is even an indication of the CPU usage (73% in this case).

Great! Now our DevOps is aware that there is an issue on this server and they can investigate on what’s happening exactly.

## VII – Conclusion

As you can see, monitoring Windows servers can easily be done using Prometheus and Grafana.

With this tutorial, you had a quick overview of what’s possible with the WMI exporter. So what’s next?

From there, you can create your own visualizations, your own dashboards and your own alerts.

Our monitoring section contains a lot of examples of what’s possible and you can definitely take some inspiration from some of the dashboards.

On the same subject, make sure to check our Windows services monitoring using Telegraf and InfluxDB.

Until then, have fun, as always.

[EXPORTER](https://devconnected.com/tag/exporter/)[PROMETHEUS](https://devconnected.com/tag/prometheus/)[PROMETHEUS MONITORING](https://devconnected.com/tag/prometheus-monitoring/)[WINDOWS](https://devconnected.com/tag/windows/)[WINDOWS MONITORING](https://devconnected.com/tag/windows-monitoring/)

13 comments

0 

[](https://www.facebook.com/sharer/sharer.php?u=https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/)[](https://twitter.com/intent/tweet?text=Check%20out%20this%20article:%20Windows%20Server%20Monitoring%20using%20Prometheus%20and%20WMI%20Exporter%20-%20https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/)[](https://reddit.com/submit?url=https%3A%2F%2Fdevconnected.com%2Fwindows-server-monitoring-using-prometheus-and-wmi-exporter%2F&title=Windows%20Server%20Monitoring%20using%20Prometheus%20and%20WMI%20Exporter)

![](https://secure.gravatar.com/avatar/96c5601ef18e54ecc99dbb77e5da7d69?s=100&d=mm&r=g)

##### [SCHKN](https://devconnected.com/author/schkn/ "Posts by schkn")

previous post

[

##### How To Set Up SSH Keys on Debian 10 Buster



](https://devconnected.com/how-to-set-up-ssh-keys-on-debian-10-buster/)

next post

[

##### How To Add and Delete Users on Debian 10 Buster



](https://devconnected.com/how-to-add-and-delete-users-on-debian-10-buster/)

#### YOU MAY ALSO LIKE

[](https://devconnected.com/how-to-install-grafana-on-ubuntu-20-04/ "How To Install Grafana on Ubuntu 20.04")

### [How To Install Grafana on Ubuntu 20.04](https://devconnected.com/how-to-install-grafana-on-ubuntu-20-04/)

[](https://devconnected.com/how-to-install-grafana-on-centos-8/ "How To Install Grafana on CentOS 8")

### [How To Install Grafana on CentOS 8](https://devconnected.com/how-to-install-grafana-on-centos-8/)

[](https://devconnected.com/how-to-install-influxdb-telegraf-and-grafana-on-docker/ "How To Install InfluxDB Telegraf and Grafana on Docker")

### [How To Install InfluxDB Telegraf and Grafana on...](https://devconnected.com/how-to-install-influxdb-telegraf-and-grafana-on-docker/)

#### 13 COMMENTS

![](https://secure.gravatar.com/avatar/9327c83e1ed4e5b5c8afe3831a03c7d0?s=100&d=mm&r=g)

CHUBBSM

All I get is “cannot read property ‘result’ of undefined”, no metrics are showing?

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-7577)

![](https://secure.gravatar.com/avatar/53d4c36173c78ee6d4a986d0161e82ea?s=100&d=mm&r=g)

SHAI

lets not say easily …

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-8259)

![](https://secure.gravatar.com/avatar/f81df70d1f94d22cb5c280003bb68677?s=100&d=mm&r=g)

JUSTIN

why are u using port 9216 for the wmi exporter? doesnt it use 9182? what a bizarre thing to do.

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-9009)

![](https://secure.gravatar.com/avatar/ade5666256063b6ebcf38ef9dd9d75c7?s=100&d=mm&r=g)

VMALB

see my comment below ![😉](https://s.w.org/images/core/emoji/13.0.1/svg/1f609.svg)

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-22306)

![](https://secure.gravatar.com/avatar/b0a87f25c594e3e1e387642cbf3e497b?s=100&d=mm&r=g)

FRANK WELLS

When I import that same Windows Node dashboard the ‘Server’ dropdown does not get populated with the localhost as seen in your image. How does one target the dashboard to the applicable host please?

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-11172)

![](https://secure.gravatar.com/avatar/cff59956e5eb65094cf2a5ba1e9053b5?s=100&d=mm&r=g)

PHILIPP GÖLLNER

2129 is outdated.  
You can either fix the JSON by search-and-replacing every “wmi_” with a “windows_” to reflect the current metric names for the windows exporter.  
or you just import a more recent dashboard (I found 6593 quite cool)

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-14342)

![](https://secure.gravatar.com/avatar/ade5666256063b6ebcf38ef9dd9d75c7?s=100&d=mm&r=g)

VMLAB

see my comment below ![😉](https://s.w.org/images/core/emoji/13.0.1/svg/1f609.svg)

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-22305)

![](https://secure.gravatar.com/avatar/ade5666256063b6ebcf38ef9dd9d75c7?s=100&d=mm&r=g)

VMLAB

Everything is working except step D. binding Prometheus.  
I was stuck there!

took a while to figure it out but here is a better config:

scrape_configs:  
– job_name: ‘prometheus’  
scrape_interval: 6s  
scrape_timeout: 5s  
static_configs:  
– targets: [‘localhost:9090’, ‘:9182’]

–> restart prometheus container

So put the IP address of your Windows machine in config file and NOT localhost!!!  
this worked for me

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-22304)

![](https://secure.gravatar.com/avatar/7b859a0b4fc74718d6ed5d2e60861824?s=100&d=mm&r=g)

NINO

I can’t get my Grafana dashboard showing (or receiving) data. I tried several community dashboards. WMI Exporter is running, Prometheus is getting the data, targets are up. But when I import any dashboard in Grafana, always error 404 is returned.  
Any hints? Thanks in adavance

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-22936)

![](https://secure.gravatar.com/avatar/96c5601ef18e54ecc99dbb77e5da7d69?s=100&d=mm&r=g)

SCHKN

Are you using the Grafana UI in order to import your dashboard?

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-22937)

![](https://secure.gravatar.com/avatar/0778af227b58e28f2cc38b96d4ec9023?s=100&d=mm&r=g)

GERMAN FIDALGO

great Post. please note dashboard “2129” is broken and wont work with the updated Windows_Exporter. i uploaded a new dash that i translated from Chinese that is working perfect after following this article.

[https://grafana.com/grafana/dashboards/14451](https://grafana.com/grafana/dashboards/14451).

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-22992)

![](https://secure.gravatar.com/avatar/3f3906fac39a70eeac9555289ec04bcb?s=100&d=mm&r=g)

THELES SILVEIRA

Hi there!  
Excellent tutorial! I’m not sure if I miss something, but the WMI exporter would leave this data exposed, readable without the need for authentication? So it would be protected by firewall rules only, right?  
Thanks!

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-23122)

![](https://secure.gravatar.com/avatar/3f3906fac39a70eeac9555289ec04bcb?s=100&d=mm&r=g)

THELES SILVEIRA

Hi there!  
Thanks for the excellent tutorial! One question, once the WMI exporter is running, that information would be published on that HTTP port without any registration need, am I right? It would be protected only if a firewall rule would be customized for it.  
Thanks!

[REPLY](https://devconnected.com/windows-server-monitoring-using-prometheus-and-wmi-exporter/#comment-23124)