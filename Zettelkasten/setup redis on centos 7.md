-   [Prerequisites](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#prerequisites)

-   [Step 1 — Installing Redis](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#step-1-%E2%80%94-installing-redis)

-   [Step 2 — Binding Redis and Securing it with a Firewall](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#step-2-%E2%80%94-binding-redis-and-securing-it-with-a-firewall)

-   [Step 3 — Configuring a Redis Password](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#step-3-%E2%80%94-configuring-a-redis-password)

-   [Step 4 — Renaming Dangerous Commands](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#step-4-%E2%80%94-renaming-dangerous-commands)

-   [Step 5 — Setting Data Directory Ownership and File Permissions](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#step-5-%E2%80%94-setting-data-directory-ownership-and-file-permissions)

-   [Conclusion](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#conclusion)

-   [NEW NEW Introducing DigitalOcean Managed MongoDB — a fully managed, database as a service for modern apps](https://www.digitalocean.com/blog/introducing-digitalocean-managed-mongodb "NEW Introducing DigitalOcean Managed MongoDB — a fully managed, database as a service for modern apps")
-   [Products](https://www.digitalocean.com/products/)
-   [Pricing](https://www.digitalocean.com/pricing/)
-   Docs
    
    -   [](https://www.digitalocean.com/docs/)
    -   [](https://developers.digitalocean.com/documentation/)
    
-   Sign in
    
    -   [](https://www.digitalocean.com/community/auth/digitalocean)
    -   [](https://cloud.digitalocean.com/registrations/new)
    

-   [](https://www.digitalocean.com/)
-   [](https://www.digitalocean.com/community "DigitalOcean Community Home")
-   [Tutorials](https://www.digitalocean.com/community/tutorials)
-   [Questions](https://www.digitalocean.com/community/questions)
-   [Tech Talks](https://www.digitalocean.com/community/tags/tech-talks)
-   Get Involved
    
    -   [
        
        ](https://www.digitalocean.com/community/pages/hub-for-good)
    -   [
        
        ](https://www.digitalocean.com/community/pages/write-for-digitalocean)
    -   [
        
        ](https://hacktoberfest.digitalocean.com/)
    
    -   [](https://www.digitalocean.com/community/tools "Community-built tools and integrations that use the DigitalOcean API")
    -   [](https://www.digitalocean.com/hatch/ "Build your startup on DigitalOcean.")
    -   [](https://marketplace.digitalocean.com/vendors/ "List your open source 1-Click Application in the DigitalOcean Marketplace")
    -   [](https://www.digitalocean.com/partners/solutions-partners/ "Easily deploy & modernize your clients’ infrastructures with the Solutions Partner Program")
    -   [](https://www.digitalocean.com/droplets-for-demos/ "DigitalOcean credits to fund research for conference and meetup presentations")
    -   [](https://github.com/digitalocean "View all the open-source projects that DigitalOcean have on GitHub")
    
-    Search Community /
-   [Sign Up](https://www.digitalocean.com/community/auth/digitalocean?display=sessionless+register+button)

### RELATED

Join the DigitalOcean Community![Community](https://digitalocean.cdn.prismic.io/digitalocean/8d8bc1f0-2a4b-473b-9793-2a240ea8ccb4_digitalocean-logo-mark-copy.svg)

Join 1M+ other developers and:

-   Get help and share knowledge in Q&A
-   Subscribe to topics of interest
-   Get courses & tools that help you grow as a developer or small business owner

[Join Now](https://www.digitalocean.com/community/auth/digitalocean?display=sessionless+register+button)

[

How To Build A Security Information and Event Management (SIEM) System with Suricata and the Elastic Stack on Rocky Linux 8![Tutorial](https://www.digitalocean.com/assets/community/tutorials/featured-item-tutorial-icon-bce4d5cc06536a907c48f6663a93392cdf6c5f30677b48439d1164c89fb71ad1.png)Tutorial

](https://www.digitalocean.com/community/tutorials/how-to-build-a-security-information-and-event-management-siem-system-with-suricata-and-the-elastic-stack-on-rocky-linux-8)[

How To Build A Security Information and Event Management (SIEM) System with Suricata and the Elastic Stack on Debian 11![Tutorial](https://www.digitalocean.com/assets/community/tutorials/featured-item-tutorial-icon-bce4d5cc06536a907c48f6663a93392cdf6c5f30677b48439d1164c89fb71ad1.png)Tutorial

](https://www.digitalocean.com/community/tutorials/how-to-build-a-security-information-and-event-management-siem-system-with-suricata-and-the-elastic-stack-on-debian-11)

#### TUTORIAL

# How To Install and Secure Redis on CentOS 7

[CentOS](https://www.digitalocean.com/community/tags/centos)[Security](https://www.digitalocean.com/community/tags/security)[Firewall](https://www.digitalocean.com/community/tags/firewall)[Redis](https://www.digitalocean.com/community/tags/redis)

-   [
    
    ![](https://community-cdn-digitalocean-com.global.ssl.fastly.net/variants/YGwXUT7FDLtcTBhwTEeQy9Sz/6a540d0de9b44df9962f62205489ba6e2a5beab748fa47e816b60597a256ba9b)
    
    ](https://www.digitalocean.com/community/users/mdrake)
-   By [Mark Drake](https://www.digitalocean.com/community/users/mdrake)
    
    Last Validated onAugust 26, 2020 Originally Published onMarch 2, 2018 124.3kviews

##### Not using CentOS 7?  
Choose a different version or distribution.

CentOS 7CentOS 8Debian 9Debian 10Ubuntu 20.04Ubuntu 18.04Ubuntu 16.04Ubuntu 14.04

CentOS 7

[See More](https://www.digitalocean.com/community/tutorial_collections/how-to-install-and-secure-redis)

### Introduction

[Redis](https://redis.io/) is an open-source, in-memory data structure store which excels at caching. A non-relational database, Redis is known for its flexibility, performance, scalability, and wide language support.

Redis was designed for use by trusted clients in a trusted environment, and has no robust security features of its own. Redis does, however, have [a few security features](https://redis.io/topics/security) that include a basic unencrypted password and command renaming and disabling. This tutorial provides instructions on how to configure these security features, and also covers a few other settings that can boost the security of a standalone Redis installation on CentOS 7.

Note that this guide does not address situations where the Redis server and the client applications are on different hosts or in different data centers. Installations where Redis traffic has to traverse an insecure or untrusted network will require a different set of configurations, such as setting up an SSL proxy or a [VPN](https://www.digitalocean.com/community/tutorials/how-to-setup-and-configure-an-openvpn-server-on-centos-7) between the Redis machines.

## Prerequisites

To follow along with this tutorial, you will need:

-   One CentOS 7 Droplet configured using our [Initial Server Setup for CentOS 7](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-7).
-   Firewalld installed and configured using [this guide](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-7), up to and including the “Turning on the Firewall” step.

With those prerequisites in place, we are ready to install Redis and perform some initial configuration tasks.

## Step 1 — Installing Redis

Before we can install Redis, we must first add [Extra Packages for Enterprise Linux (EPEL) repository](https://fedoraproject.org/wiki/EPEL) to the server’s package lists. EPEL is a package repository containing a number of open-source add-on software packages, most of which are maintained by the Fedora Project.

We can install EPEL using `yum`:

```bash
sudo yum install epel-release
```

 

Copy

Once the EPEL installation has finished you can install Redis, again using `yum`:

```bash
sudo yum install redis -y
```

 

Copy

This may take a few minutes to complete. After the installation finishes, start the Redis service:

```bash
sudo systemctl start redis.service
```

 

Copy

If you’d like Redis to start on boot, you can enable it with the `enable` command:

```bash
sudo systemctl enable redis
```

 

Copy

You can check Redis’s status by running the following:

```bash
sudo systemctl status redis.service
```

 

Copy

```
Output● redis.service - Redis persistent key-value database
   Loaded: loaded (/usr/lib/systemd/system/redis.service; disabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/redis.service.d
           └─limit.conf
   Active: active (running) since Thu 2018-03-01 15:50:38 UTC; 7s ago
 Main PID: 3962 (redis-server)
   CGroup: /system.slice/redis.service
           └─3962 /usr/bin/redis-server 127.0.0.1:6379
```

Once you’ve confirmed that Redis is indeed running, test the setup with this command:

```bash
redis-cli ping
```

 

Copy

This should print `PONG` as the response. If this is the case, it means you now have Redis running on your server and we can begin configuring it to enhance its security.

## Step 2 — Binding Redis and Securing it with a Firewall

An effective way to safeguard Redis is to secure the server it’s running on. You can do this by ensuring that Redis is bound only to either localhost or to a private IP address and that the server has a firewall up and running.

However, if you chose to set up a Redis cluster using [this tutorial](https://www.digitalocean.com/community/tutorials/how-to-configure-a-redis-cluster-on-centos-7), then you will have updated the configuration file to allow connections from anywhere, which is not as secure as binding to localhost or a private IP.

To remedy this, open the Redis configuration file for editing:

```bash
sudo vi /etc/redis.conf
```

 

Copy

Locate the line beginning with `bind` and make sure it’s uncommented:

/etc/redis.conf

```bash
bind 127.0.0.1
```

 

Copy

If you need to bind Redis to another IP address (as in cases where you will be accessing Redis from a separate host) we **strongly** encourage you to bind it to a private IP address. Binding to a public IP address increases the exposure of your Redis interface to outside parties.

/etc/redis.conf

```bash
bind your_private_ip
```

 

Copy

If you’ve followed the prerequisites and installed firewalld on your server and you do not plan to connect to Redis from another host, then you do not need to add any extra firewall rules for Redis. After all, any incoming traffic will be dropped by default unless explicitly allowed by the firewall rules. Since a default standalone installation of Redis server is listening only on the loopback interface (`127.0.0.1` or localhost), there should be no concern for incoming traffic on its default port.

If, however, you do plan to access Redis from another host, you will need to make some changes to your firewalld configuration using the `firewall-cmd` command. Again, you should only allow access to your Redis server from your hosts by using their private IP addresses in order to limit the number of hosts your service is exposed to.

To begin, add a dedicated Redis zone to your firewalld policy:

```bash
sudo firewall-cmd --permanent --new-zone=redis
```

 

Copy

Then, specify which port you’d like to have open. Redis uses port `6379` by default:

```bash
sudo firewall-cmd --permanent --zone=redis --add-port=6379/tcp
```

 

Copy

Next, specify any private IP addresses which should be allowed to pass through the firewall and access Redis:

```bash
sudo firewall-cmd --permanent --zone=redis --add-source=client_server_private_IP
```

 

Copy

After running those commands, reload the firewall to implement the new rules:

```bash
sudo firewall-cmd --reload
```

 

Copy

Under this configuration, when the firewall sees a packet from your client’s IP address, it will apply the rules in the dedicated Redis zone to that connection. All other connections will be processed by the default `public` zone. The services in the default zone apply to every connection, not just those that don’t match explicitly, so you don’t need to add other services (e.g. SSH) to the Redis zone because those rules will be applied to that connection automatically.

If you chose to [set up a firewall using Iptables](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-basic-iptables-firewall-on-centos-6), you will need to grant your secondary hosts access to the port Redis is using with the following commands:

```bash
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A INPUT -p tcp -s client_servers_private_IP/32 --dport 6379 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo iptables -P INPUT DROP
```

 

Copy

Make sure to save your Iptables firewall rules using the mechanism provided by your distribution. You can learn more about Iptables by taking a look at our [Iptables essentials guide](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands).

Keep in mind that using either firewall tool will work. What’s important is that the firewall is up and running so that unknown individuals cannot access your server. In the next step, we will configure Redis to only be accessible with a strong password.

## Step 3 — Configuring a Redis Password

If you installed Redis using the [How To Configure a Redis Cluster on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-configure-a-redis-cluster-on-centos-7) tutorial, you should have configured a password for it. At your discretion, you can make a more secure password now by following this section. If you haven’t set up a password yet, instructions in this section show how to set the database server password.

Configuring a Redis password enables one of its built-in security features — the `auth` command — which requires clients to authenticate before being allowed access to the database. Like the `bind` setting, the password is configured directly in Redis’s configuration file, `/etc/redis.conf`. Reopen that file:

```bash
sudo vi /etc/redis.conf
```

 

Copy

Scroll to the `SECURITY` section and look for a commented directive that reads:

/etc/redis.conf

```bash
# requirepass foobared
```

 

Copy

Uncomment it by removing the `#`, and change `foobared` to a very strong password of your choosing. Rather than make up a password yourself, you may use a tool like `apg` or `pwgen` to generate one. If you don’t want to install an application just to generate a password, though, you may use the command below.

Note that entering this command as written will generate the same password every time. To create a password different from the one that this would generate, change the word in quotes to any other word or phrase.

```bash
echo "digital-ocean" | sha256sum
```

 

Copy

Though the generated password will not be pronounceable, it is a very strong and very long one, which is exactly the type of password required for Redis. After copying and pasting the output of that command as the new value for `requirepass`, it should read:

/etc/redis.conf

```bash
requirepass password_copied_from_output
```

 

Copy

If you prefer a shorter password, use the output of the command below instead. Again, change the word in quotes so it will not generate the same password as this one:

```bash
echo "digital-ocean" | sha1sum
```

 

Copy

After setting the password, save and close the file then restart Redis:

```bash
sudo systemctl restart redis.service
```

 

Copy

To test that the password works, access the Redis command line:

```bash
redis-cli
```

 

Copy

The following is a sequence of commands used to test whether the Redis password works. The first command tries to set a key to a value before authentication.

```bash
set key1 10
```

 

Copy

That won’t work as we have not yet been authenticated, so Redis returns an error.

```
Output(error) NOAUTH Authentication required.
```

The following command authenticates with the password specified in the Redis configuration file.

```bash
auth your_redis_password
```

 

Copy

Redis will acknowledge that we have been authenticated:

```
OutputOK
```

After that, running the previous command again should be successful:

```bash
set key1 10
```

 

Copy

```
OutputOK
```

The `get key1` command queries Redis for the value of the new key.

```bash
get key1
```

 

Copy

```
Output"10"
```

This last command exits `redis-cli`. You may also use `exit`:

```bash
quit
```

 

Copy

It should now be very difficult for unauthorized users to access your Redis installation. Please note, though, that without SSL or a VPN the unencrypted password will still be visible to outside parties if you’re connecting to Redis remotely.

Next, we’ll look at renaming Redis commands to further protect Redis from malicious actors.

## Step 4 — Renaming Dangerous Commands

The other security feature built into Redis allows you to rename or completely disable certain commands that are considered dangerous. When run by unauthorized users, such commands can be used to reconfigure, destroy, or otherwise wipe your data. Some of the commands that are known to be dangerous include:

-   `FLUSHDB`
-   `FLUSHALL`
-   `KEYS`
-   `PEXPIRE`
-   `DEL`
-   `CONFIG`
-   `SHUTDOWN`
-   `BGREWRITEAOF`
-   `BGSAVE`
-   `SAVE`
-   `SPOP`
-   `SREM` `RENAME` `DEBUG`

This is not a comprehensive list, but renaming or disabling all of the commands in that list is a good starting point.

Whether you disable or rename a command is site-specific. If you know you will never use a command that can be abused, then you may disable it. Otherwise, you should rename it instead.

Like the authentication password, renaming or disabling commands is configured in the `SECURITY` section of the `/etc/redis.conf` file. To enable or disable Redis commands, open the configuration file for editing one more time:

```bash
sudo vi  /etc/redis.conf
```

 

Copy

**NOTE**: These are examples. You should choose to disable or rename the commands that make sense for you. You can check the commands for yourself and determine how they might be misused at [redis.io/commands](http://redis.io/commands).  

To disable or kill a command, simply rename it to an empty string, as shown below:

/etc/redis.conf

```bash
# It is also possible to completely kill a command by renaming it into
# an empty string:
#
rename-command FLUSHDB ""
rename-command FLUSHALL ""
rename-command DEBUG ""
```

 

Copy

To rename a command, give it another name like in the examples below. Renamed commands should be difficult for others to guess, but easy for you to remember:

/etc/redis.conf

```bash
rename-command CONFIG ""
rename-command SHUTDOWN SHUTDOWN_MENOT
rename-command CONFIG ASC12_CONFIG
```

 

Copy

Save your changes and close the file, and then apply the change by restarting Redis:

```bash
sudo systemctl restart redis.service
```

 

Copy

To test the new command, enter the Redis command line:

```bash
redis-cli
```

 

Copy

Authenticate yourself using the password you defined earlier:

```bash
auth your_redis_password
```

 

Copy

```
OutputOK
```

Assuming that you renamed the **CONFIG** command to **ASC12_CONFIG**, attempting to use the `config` command should fail.

```bash
config get requirepass
```

 

Copy

```
Output(error) ERR unknown command 'config'
```

Calling the renamed command should be successful (it’s case-insensitive):

```bash
asc12_config get requirepass
```

 

Copy

```
Output1) "requirepass"
2) "your_redis_password"
```

Finally, you can exit from `redis-cli`:

```bash
exit
```

 

Copy

Note that if you’re already using the Redis command line and then restart Redis, you’ll need to re-authenticate. Otherwise, you’ll get this error if you type a command:

```
OutputNOAUTH Authentication required.
```

Regarding renaming commands, there’s a cautionary statement at the end of the `SECURITY` section in the `/etc/redis.conf` file, which reads:

/etc/redis.conf

```bash
. . .

# Please note that changing the name of commands that are logged into the
# AOF file or transmitted to slaves may cause problems.

. . .
```

 

Copy

That means if the renamed command is not in the AOF file, or if it is but the AOF file has not been transmitted to slaves, then there should be no problem. Keep that in mind as you’re renaming commands. The best time to rename a command is when you’re not using AOF persistence or right after installation (that is, before your Redis-using application has been deployed).

When you’re using AOF and dealing with a master-slave installation, consider this answer from the project’s GitHub issue page. The following is a reply to the author’s question:

> The commands are logged to the AOF and replicated to the slave the same way they are sent, so if you try to replay the AOF on an instance that doesn’t have the same renaming, you may face inconsistencies as the command cannot be executed (same for slaves).

The best way to handle renaming in cases like that is to make sure that renamed commands are applied to all instances of master-slave installations.  

## Step 5 — Setting Data Directory Ownership and File Permissions

In this step, we’ll consider a couple of ownership and permissions changes you can make to improve the security profile of your Redis installation. This involves making sure that only the user that needs to access Redis has permission to read its data. That user is, by default, the **redis** user.

You can verify this by `grep`-ing for the Redis data directory in a long listing of its parent directory. The command and its output are given below.

```bash
ls -l /var/lib | grep redis
```

 

Copy

```
Outputdrwxr-xr-x 2 redis   redis   4096 Aug  6 09:32 redis
```

You can see that the Redis data directory is owned by the **redis** user, with secondary access granted to the **redis** group. This ownership setting is secure, but the folder’s permissions (which are set to 755) are not. To ensure that only the Redis user has access to the folder and its contents, change the permissions setting to 770:

```bash
sudo chmod 770 /var/lib/redis
```

 

Copy

The other permission you should change is that of the Redis configuration file. By default, it has a file permission of 644 and is owned by **root**, with secondary ownership by the **root** group:

```bash
ls -l /etc/redis.conf
```

 

Copy

```
Output-rw-r--r-- 1 root root 30176 Jan 14  2014 /etc/redis.conf
```

That permission (644) is world-readable. This presents a security issue as the configuration file contains the unencrypted password you configured in Step 4, meaning we need to change the configuration file’s ownership and permissions. Ideally, it should be owned by the **redis** user, with secondary ownership by the **redis** group. To do that, run the following command:

```bash
sudo chown redis:redis /etc/redis.conf
```

 

Copy

Then change the permissions so that only the owner of the file can read and/or write to it:

```bash
sudo chmod 600 /etc/redis.conf
```

 

Copy

You may verify the new ownership and permissions using:

```bash
ls -l /etc/redis.conf
```

 

Copy

```
Outputtotal 40
-rw------- 1 redis redis 29716 Sep 22 18:32 /etc/redis.conf
```

Finally, restart Redis:

```bash
sudo systemctl restart redis.service
```

 

Copy

Congratulations, your Redis installation should now be more secure!

## Conclusion

Keep in mind that once someone is logged in to your server, it’s very easy to circumvent the Redis-specific security features we’ve put in place. This is why the most important security feature covered in this tutorial is the firewall, as that prevents unknown users from logging into your server in the first place.

If you’re attempting to secure Redis communication across an untrusted network you’ll have to employ an SSL proxy, as recommended by Redis developers in the [official Redis security guide](http://redis.io/topics/security).

Was this helpful?YesNo

 [](http://twitter.com/share?text=How%20To%20Install%20and%20Secure%20Redis%20on%20CentOS%207&url=https%3A%2F%2Fwww.digitalocean.com%2Fcommunity%2Ftutorials%2Fhow-to-install-secure-redis-centos-7%2Fhelpfulness_and_share_box%3Futm_content%3Dhow-to-install-secure-redis-centos-7%26utm_medium%3Dcommunity%26utm_source%3Dtwshare)  [](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fwww.digitalocean.com%2Fcommunity%2Ftutorials%2Fhow-to-install-secure-redis-centos-7%2Fhelpfulness_and_share_box%3Futm_content%3Dhow-to-install-secure-redis-centos-7%26utm_medium%3Dcommunity%26utm_source%3Dfbshare) [](https://news.ycombinator.com/submitlink?u=https%3A%2F%2Fwww.digitalocean.com%2Fcommunity%2Ftutorials%2Fhow-to-install-secure-redis-centos-7%2Fhelpfulness_and_share_box%3Futm_content%3Dhow-to-install-secure-redis-centos-7%26utm_medium%3Dcommunity%26utm_source%3Dhnshare&t=How%20To%20Install%20and%20Secure%20Redis%20on%20CentOS%207)

**4**

[Report an issue](javascript:void(0);)

About the authors

[

![](https://community-cdn-digitalocean-com.global.ssl.fastly.net/variants/YGwXUT7FDLtcTBhwTEeQy9Sz/6a540d0de9b44df9962f62205489ba6e2a5beab748fa47e816b60597a256ba9b)



](https://www.digitalocean.com/community/users/mdrake)

[Mark Drake](https://www.digitalocean.com/community/users/mdrake)

Technical Writer @ DigitalOcean

#### Still looking for an answer?

[Ask a question](https://www.digitalocean.com/community/questions/new?tags=CentOS%2CSecurity%2CFirewall%2CRedis)[Search for more help](javascript:void(0))

-   Comments

#### 4 Comments

-   B
-   I
-   UL
-   OL
-   Link
-   Code
-   Highlight
-   Table

[Sign In to Comment](https://www.digitalocean.com/community/auth/digitalocean)

-   0
    
    [dungankhanh112](https://www.digitalocean.com/community/users/dungankhanh112) [March 13, 2018](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7?comment=68885)
    
    HelpMe  
    Too hard and I can not install VPS CentOS 6, expect admin to help me with, thanks
    
    [**Reply**](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#) [Report](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#)
    
     
    
-   0
    
    [matapala](https://www.digitalocean.com/community/users/matapala) [August 24, 2020](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7?comment=90764)
    
    Small typo i think :  
    `sudo chmod 660 /etc/redis.conf` should be `sudo chmod 600 /etc/redis.conf` to produce the output expected:  
    `total 40  
    -rw------- 1 redis redis 29716 Sep 22 18:32 /etc/redis.conf`
    
    [**Reply**](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#) [Report](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#)
    
     
    
    -   0
        
        [mdrake](https://www.digitalocean.com/community/users/mdrake)  [August 26, 2020](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7?comment=90815)
        
        Hello [@matapala](https://www.digitalocean.com/community/users/matapala),
        
        Indeed! I’ve updated the command to include the correct octal notation. Great catch!
        
        [**Reply**](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#) [Report](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#)
        
         
        
    
-   0
    
    [endilinuxmint](https://www.digitalocean.com/community/users/endilinuxmint) [July 25, 2021](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7?comment=99985)
    
    Thank you. This is very helpful for people like me
    
    [**Reply**](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#) [Report](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#)
    
     
    

[![Creative Commons License](https://www.digitalocean.com/assets/community/creativecommons-027bb7f065acf05ba3c0f84a040d2da641648afc81daa6ff5570301d4998bbb6.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

This work is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/).

[

![](https://images.prismic.io/digitalocean/ca12c951cc76f33037f3384bba9942545d160d82_iaan_illo.jpg?auto=compress,format)

GET OUR BIWEEKLY NEWSLETTER

Sign up for Infrastructure as a Newsletter.

](https://www.digitalocean.com/community/newsletter)[

![](https://images.prismic.io/digitalocean/8a56a463-cf2c-42a2-9d2f-9096c4218f30_HH4G-reference.jpg?auto=compress,format&rect=0,0,370,140&w=370&h=140)

HOLLIE'S HUB FOR GOOD

Working on improving health and education, reducing inequality, and spurring economic growth? We'd like to help.

](https://www.digitalocean.com/community/pages/hollies-hub-for-good)[

![](https://images.prismic.io/digitalocean/70910e9fdeb57be46aaa209ce8d9b4dc8e117fab_w4do2.jpg?auto=compress,format)

BECOME A CONTRIBUTOR

You get paid; we donate to tech nonprofits.

](https://www.digitalocean.com/community/pages/write-for-digitalocean)

Featured on Community[Kubernetes Course](https://www.digitalocean.com/community/curriculums/kubernetes-for-full-stack-developers)[Learn Python 3](https://www.digitalocean.com/community/tutorial_series/how-to-code-in-python-3)[Machine Learning in Python](https://www.digitalocean.com/community/tutorials/machine-learning-projects-python-a-digitalocean-ebook)[Getting started with Go](https://www.digitalocean.com/community/tutorials/how-to-write-your-first-program-in-go)[Intro to Kubernetes](https://www.digitalocean.com/community/tutorials/an-introduction-to-kubernetes)

---

DigitalOcean Products[Virtual Machines](https://www.digitalocean.com/products/droplets/)[Managed Databases](https://www.digitalocean.com/products/managed-databases/)[Managed Kubernetes](https://www.digitalocean.com/products/kubernetes/)[Block Storage](https://www.digitalocean.com/products/block-storage/)[Object Storage](https://www.digitalocean.com/products/spaces/)[Marketplace](https://marketplace.digitalocean.com/)[VPC](https://www.digitalocean.com/products/vpc/)[Load Balancers](https://www.digitalocean.com/products/load-balancer/)

[

### Welcome to the developer cloud

DigitalOcean makes it simple to launch in the cloud and scale up as you grow – whether you’re running one virtual machine or ten thousand.

Learn More](https://www.digitalocean.com/products)

[![DigitalOcean Cloud Control Panel](https://images.prismic.io/digitalocean/95c1215227aa7f39f2bd23076de28feb969741c7_cloud.digitalocean.com_projects_63f9252f-652b-4645-9c0c-bee96f2bc503_resources_ic0ce81-2.png?auto=compress,format)](https://www.digitalocean.com/products)

[](https://www.digitalocean.com/)  
  
© 2022 DigitalOcean, LLC. All rights reserved.

Company

-   [About](https://www.digitalocean.com/about/)
-   [Leadership](https://www.digitalocean.com/about/leadership/)
-   [Blog](https://www.digitalocean.com/blog/)
-   [Careers](https://www.digitalocean.com/careers/)
-   [Partners](https://www.digitalocean.com/partners/)
-   [Referral Program](https://www.digitalocean.com/referral-program/)
-   [Press](https://www.digitalocean.com/press/)
-   [Legal](https://www.digitalocean.com/legal/)
-   [Security & Trust Center](https://www.digitalocean.com/trust/)

Products

-   [Pricing](https://www.digitalocean.com/pricing/)
-   [Products Overview](https://www.digitalocean.com/products/)
-   [Droplets](https://www.digitalocean.com/products/droplets/)
-   [Kubernetes](https://www.digitalocean.com/products/kubernetes/)
-   [Managed Databases](https://www.digitalocean.com/products/managed-databases/)
-   [Spaces](https://www.digitalocean.com/products/spaces/)
-   [Marketplace](https://www.digitalocean.com/products/marketplace/)
-   [Load Balancers](https://www.digitalocean.com/products/load-balancer/)
-   [Block Storage](https://www.digitalocean.com/products/block-storage/)
-   [API Documentation](https://developers.digitalocean.com/documentation/)
-   [Documentation](https://www.digitalocean.com/docs)
-   [Release Notes](https://www.digitalocean.com/docs/release-notes/)

Community

-   [Tutorials](https://www.digitalocean.com/community/tutorials)
-   [Q&A](https://www.digitalocean.com/community/questions)
-   [Tools and Integrations](https://www.digitalocean.com/community/tools)
-   [Tags](https://www.digitalocean.com/community/tags)
-   [Write for DigitalOcean](https://www.digitalocean.com/write-for-donations/)
-   [Presentation Grants](https://www.digitalocean.com/droplets-for-demos/)
-   [Hatch Startup Program](https://www.digitalocean.com/hatch/)
-   [Shop Swag](http://store.digitalocean.com/)
-   [Research Program](https://www.digitalocean.com/research/)
-   [Open Source](https://www.digitalocean.com/open-source/)
-   [Code of Conduct](https://www.digitalocean.com/community/pages/code-of-conduct)

Contact

-   [Get Support](https://www.digitalocean.com/support/)
-   [Trouble Signing In?](https://www.digitalocean.com/docs/getting-started/faq/)
-   [Sales](https://www.digitalocean.com/company/contact/sales/)
-   [Report Abuse](https://www.digitalocean.com/company/contact/#abuse)
-   [System Status](https://status.digitalocean.com/)
-   [Share your ideas](https://ideas.digitalocean.com/)

[SCROLL TO TOP](https://www.digitalocean.com/community/tutorials/how-to-install-secure-redis-centos-7#top)