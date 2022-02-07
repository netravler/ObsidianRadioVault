# Redis

-   Overview
-   Installation
-   Change log
-   Related content

#### Installing Redis on Grafana Cloud:

Installing plugins on a Grafana Cloud instance is a one-click install; same with updates. Cool, right?

Note that it could take up to 1 minute to see the plugin show up in your Grafana.

[Sign up for Grafana Cloud to install Redis.](https://grafana.com/auth/sign-up/create-user?plcmt=redis-datasource&pg=plugins)

For more information, visit the docs on [plugin installation](https://grafana.com/docs/grafana/latest/plugins/installation/).

#### Installing on a local Grafana:

For local instances, plugins are installed and updated via a simple CLI command. Plugins are not updated automatically, however you will be notified when updates are available right within your Grafana.

##### 1. Install the Data Source

Use the grafana-cli tool to install Redis from the commandline:

```
grafana-cli plugins install redis-datasource
```

The plugin will be installed into your grafana plugins directory; the default is /var/lib/grafana/plugins. [More information on the cli tool](https://grafana.com/docs/grafana/latest/administration/cli/#plugins-commands).

Alternatively, you can manually download the .zip file and unpack it into your grafana plugins directory.

 [Download](https://grafana.com/api/plugins/redis-datasource/versions/2.1.1/download)

##### 2. Configure the Data Source

Accessed from the Grafana main menu, newly installed data sources can be added immediately within the Data Sources section.

Next, click the Add data source button in the upper right. The data source will be available for selection in the **Type** select box.

To see a list of installed data sources, click the **Plugins** item in the main menu. Both core data sources and installed data sources will appear.