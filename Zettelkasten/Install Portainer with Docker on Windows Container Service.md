Install Portainer with Docker on Windows Container Service

These instructions are for Portainer Community Edition. For Business Edition, please refer to the [Business Edition documentation](https://docs.portainer.io/v/be-2.7/).

# 

Introduction

Portainer consists of two elements, the _Portainer Server_, and the _Portainer Agent_. Both elements run as lightweight Docker containers on a Docker engine. This document will help you install the Portainer Server container on your Windows server with Windows Containers. To add a new WCS environment to an existing Portainer Server installation, please refer to the [Portainer Agent installation instructions](https://docs.portainer.io/v/ce-2.9/start/install/agent/docker/wcs).

To get started, you will need:

-   The latest version of Docker installed and working
    
-   Administrator access on the machine that will host your Portainer Server instance
    
-   By default, Portainer Server will expose the UI over port `9443` and expose a TCP tunnel server over port `8000`. The latter is optional and is only required if you plan to use the Edge compute features with Edge agents.
    

The installation instructions also make the following assumption about your environment:

-   Your environment meets [our requirements](https://docs.portainer.io/v/ce-2.9/start/requirements-and-prerequisites). While Portainer may work with other configurations, it may require configuration changes or have limited functionality.
    

# 

Preparation

To run Portainer Server in a Windows Server/Desktop Environment you need to create exceptions in the firewall. These can easily be added through PowerShell by running the following commands:

1

netsh advfirewall firewall add rule name="cluster_management" dir=in action=allow protocol=TCP localport=2377

2

netsh advfirewall firewall add rule name="node_communication_tcp" dir=in action=allow protocol=TCP localport=7946

3

netsh advfirewall firewall add rule name="node_communication_udp" dir=in action=allow protocol=UDP localport=7946

4

netsh advfirewall firewall add rule name="overlay_network" dir=in action=allow protocol=UDP localport=4789

5

netsh advfirewall firewall add rule name="swarm_dns_tcp" dir=in action=allow protocol=TCP localport=53

6

netsh advfirewall firewall add rule name="swarm_dns_udp" dir=in action=allow protocol=UDP localport=53

Copied!

You will also need to install the Windows Container Host Service and install Docker:

1

Enable-WindowsOptionalFeature -Online -FeatureName containers -All

2

Install-Module -Name DockerMsftProvider -Repository PSGallery -Force

3

Install-Package -Name docker -ProviderName DockerMsftProvider

Copied!

Once this is complete you will need to restart your Windows server. After the restart completes, you're ready to install Portainer itself.

# 

Deployment

First, create the volume that Portainer Server will use to store its database. Using PowerShell:

1

docker volume create portainer_data

Copied!

Then, download and install the Portainer Server container:

1

docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart always -v \\.\pipe\docker_engine:\\.\pipe\docker_engine -v portainer_data:C:\data cr.portainer.io/portainer/portainer-ce:2.9.3

Copied!

By default, Portainer generates and uses a self-signed SSL certificate to secure port `9443`. Alternatively you can provide your own SSL certificate [during installation](https://docs.portainer.io/v/ce-2.9/advanced/ssl#docker-standalone) or [via the Portainer UI](https://docs.portainer.io/v/ce-2.9/admin/settings#ssl-certificate) after installation is complete.

If you require HTTP port `9000` open for legacy reasons, add the following to your `docker run` command:

`-p 9000:9000`

# 

Logging In

Now that the installation is complete, you can log into your Portainer Server instance by opening a web browser and going to:

1

https://localhost:9443

Copied!

Replace `localhost` with the relevant IP address or FQDN if needed, and adjust the port if you changed it earlier.

You will be presented with the initial setup page for Portainer Server.