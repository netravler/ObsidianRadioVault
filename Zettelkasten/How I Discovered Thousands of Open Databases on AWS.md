[![Avi Lumelsky](https://miro.medium.com/fit/c/96/96/1*_HaNpgL0D5Nfz8d9Pl-AuQ.jpeg)](https://avi-lumelsky.medium.com/?source=post_page-----764729aa7f32-----------------------------------)

[Avi Lumelsky](https://avi-lumelsky.medium.com/?source=post_page-----764729aa7f32-----------------------------------)

# How I Discovered Thousands of Open Databases on AWS

My journey on finding and reporting databases with sensitive data about Fortune-500 companies, Hospitals, Crypto platforms, Startups during due diligence, and more.

**_Table Of Contents_**

-   [_Overview_](https://infosecwriteups.com/how-i-discovered-thousands-of-open-databases-on-aws-764729aa7f32#e9d4)
-   [_Background_](https://infosecwriteups.com/how-i-discovered-thousands-of-open-databases-on-aws-764729aa7f32#e257)
-   [_My Hypothesis_](https://infosecwriteups.com/how-i-discovered-thousands-of-open-databases-on-aws-764729aa7f32#ac13)
-   [_Scanning_](https://infosecwriteups.com/how-i-discovered-thousands-of-open-databases-on-aws-764729aa7f32#9ccc)
-   [_BI & Automation: From thousands to hundreds_](https://infosecwriteups.com/how-i-discovered-thousands-of-open-databases-on-aws-764729aa7f32#bfa8)
-   [_Examples of data I found_](https://infosecwriteups.com/how-i-discovered-thousands-of-open-databases-on-aws-764729aa7f32#836b)
-   [_Conclusion_](https://infosecwriteups.com/how-i-discovered-thousands-of-open-databases-on-aws-764729aa7f32#776f)

# Overview

It is easy to find misconfigured assets on cloud services, by scanning the [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) blocks (IP ranges) of managed services, since they are known and published by them.

![](https://miro.medium.com/max/1400/1*IAnU4gla_lOiy4u_ZfunWQ.png)

An email from one of the companies I reported.

In just 1 day, I found thousands of [ElasticSearch](https://www.elastic.co/) databases and K[ibana](https://www.elastic.co/) dashboards that exposed sensitive information, most probably by mistake:

-   Sensitive information about **customers**: **emails, addresses, current occupation, salaries, private wallets addresses, locations, bank accounts**, and other sensitive information.
-   **Production** **Logs** that are written by Kubernetes cluster — From the applications logs to the kernels and system logs.  
    Logs that are collected from all the nodes, pods, and applications running on top of them, in one place, are open to the world. I just got there first.
-   Some of the databases were already malformed by ransomware.

![](https://miro.medium.com/max/1400/1*vbs3OC7yoKyr6e8TH8kD_Q.png)

A company that I found compromised, is giving services to these companies. The image is taken from their website.

# Background

There must be plenty of assets out there, listening outside their scope, waiting to be discovered.  
published [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) blocks make it easier for attackers to find these assets, to spread all types of malware, or to put their hands on sensitive data of real companies.

DevOps, Developers, and IT practitioners often misconfigure some of the following:

-   Binding the socket on the wrong network interfaces.  
    For example, listening to connections from 0.0.0.0/* — So it is visible to all network interfaces, instead of only the inner-network interface IP address (172.x.x.x)
-   Misconfigured security group for the cluster (allow all TCP and all UDP from broad [CIDR blocks](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)).
-   Sometimes, the security group is changed by others that are not aware of the consequences.
-   The default network or subnet is used, subnet settings are derived, and a public IPv4 address is assigned silently.

# The Hypothesis

I hypothesized that **I can easily find misconfigured assets,** mostly due to human errors **if I will scan specific** [**CIDR**](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) **blocks from within the cloud operator (An Instance / VPS).**

**The key is to scan smartly**, leveraging the pre-known network infrastructure (known [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)-blocks for the software we like to scan) to find live servers, that are within my reach.

If you search a bit, you can find the relevant [CIDR blocks of every cloud provider.](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html)  
Let’s say I’m an IT technician or a security engineer, that needs to allow incoming connections from a specific cloud service, like A[WS’s Elastic Container Service (ECS)](https://aws.amazon.com/ecs/). It can be done by adding the CIDR blocks of the service to the Security Group rules, allowing connections from these [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) blocks.

All Cloud providers publish a list of their services along with their [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) blocks (IP addresses ranges) for each service.

[

## AWS IP address ranges

### Amazon Web Services (AWS) publishes its current IP address ranges in JSON format. To view the current ranges, download…

docs.aws.amazon.com







](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html)

Get the CIDR blocks of ElasticSearch Service (ES) from AWS

Some [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) blocks were [**accessible only from within the cloud provider**](https://en.wikipedia.org/wiki/Private_network). All you have to do is to start a VPS / instance with internet connectivity inside the cloud provider you want to scan.

## What does one need to find them?

-   A basic understanding of networks, the IP stack and routing, and cloud infrastructure.
-   A lightweight tool for Port Scanning (like [**MasScan**](https://github.com/robertdavidgraham/masscan), or [NMap](https://nmap.org/))
-   A list of [CIDR blocks](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) to scan(managed services — like [Kubernetes](https://kubernetes.io/) or [ElasticSearch](https://www.elastic.co/)) along with the ports that are most likely to be open on the instances within these IP ranges.
-   A tool to visualize all the data we collect (like [ElasticSearch](https://www.elastic.co/)+[Kibana](https://www.elastic.co/what-is/kibana))

# Port Scanning — Collecting the data about the assets

I have used [**MasScan**](https://github.com/robertdavidgraham/masscan) to scan for the open ports on the CIDR blocks I selected.  
MasScan is a TCP port scanner, it spews SYN packets asynchronously.  
Under the right circumstances, it can scan the entire Internet in under 5 minutes.

The input is the CIDR blocks (e.g _50.60.0.0/16 or 118.23.1.0/24)_ and the ports we would like to scan (9200, 5600, 80, 443, etc.).

I started an **ELK** stack on my instance using Docker images.  
I started **MasScan** on the same machine, and it started scanning the CIDR blocks. It streamed the output (response logs) of MasScan to ElasticSearch using **LogStash** and visualized everything through **Kibana**.  
During the scan, the TCP responses were logged and indexed in [ElasticSearch](https://www.elastic.co/).

I let it run for a while, and I had 337K**+ IP and Ports combinations** scanned in my hands, in no time.  
Many of them were opened.  
This is how my dashboard looked:

![](https://miro.medium.com/max/1400/1*c_wmJwIe7P-c7fN0fFG7JA.png)

337K open ports in AWS’s ElasticSearch service CIDR blocks (customers clusters), in a few hours.

# Analyzing And Visualizing The Data

> The photos are censored.  
> I have reported the incidents to the involved parties as well as AWS.  
> I got their permission to proceed with this post.
> 
> Most of the parties addressed the issue within a day or two.  
> Some of them ignored the reports to this day.

Thanks to the pipeline I created — I had real-time logs and I could start looking at the services immediately, as it scans for more.

-   I exported the assets that were up from **Kibana** to a **CSV** file using Kibana’s export button. Then I loaded it using pandas (python).

![](https://miro.medium.com/max/552/1*JmgBJHRtCA6aLXObyQ4rNA.png)

-   For each IP, I fired an HTTP HEAD request and got an HTTP response with the fingerprint of the asset.

![](https://miro.medium.com/max/778/1*cw9OUliReATo2rJC8eNr-w.png)

![](https://miro.medium.com/max/874/1*F_1Whdkh0USVh2u7P54FIg.png)

-   I eliminated the responses that required authentication.

![](https://miro.medium.com/max/704/1*fTdby2gUlfDOHC7vFTO9Cw.png)

-   Then I printed their web page’s titles from the HTTP response.

![](https://miro.medium.com/max/588/1*spRQfef0vxfyWkf297TA6w.png)

We can do the same for ElasticSearch port, or any service. The IPs were already scanned by MasScan so I assumed they were up.

![](https://miro.medium.com/max/1400/1*oDBq3BmgJzWyJpEYkVayVg.png)

Printing the title of the HTML documents that came in response from the endpoint I found. Can be also done using nmap’s HTTP script.

![](https://miro.medium.com/max/1400/1*NxOo3DS3ZzyWxGvxSwIqyw.png)

Getting the title of the servers that I found relevant

so, now I have a list of all the assets, along with their web page name.  
I explored the addresses in the browser by entering them in the navigation bar.

Then, I explored the assets carefully, one at a time.  
I searched for the following:

-   **_/_aliases_**
-   **_/<index>/_search_**

This [ElasticSearch](https://www.elastic.co/) REST API is very convenient. It is easy to get the fields metadata, documents count, and everything you wanted to know about the ES cluster.

![](https://miro.medium.com/max/1400/1*ogTOoUE4GwuJyehg6kixag.png)

ElasticSearch Index summary via /_aliases REST call

# Examples Of Data I Found

-   **Kubernetes logs for the entire production clusters** (gathered through log collectors)

![](https://miro.medium.com/max/1400/1*QhPT-j3KK3a5ilY4lhCFlw.png)

K8 cluster logs, streamed via fluentd, with live URLs and logs to the kernel.

![](https://miro.medium.com/max/1400/1*KlSONQQ2YZVVsHTE_8ZbGQ.png)

Successful SSH login using the private key

![](https://miro.medium.com/max/1400/1*sk1DG-cxTd49HPoQZAdspA.png)

SSH Daemon Logs — Cluster’s machines. Realtime logs.

![](https://miro.medium.com/max/1400/1*aIWhmk3KWkXRQN3cuKZriQ.png)

Full cloud visibility — instance type, AMI, account ID, visible to the world, thanks to misconfigured clusters of K8 and ElasticSearch

-   **Private Dashboards**

some companies have streamed Jira tasks and issued them into their ElasticSearch dashboards) — including customer data, code examples, accounts names, etc. This Kibana dashboard contained everything about the R&D of the company.  
The company was in the middle (or peak) of their due-diligence process, reporting they raised quite a lot of money just a few weeks after I reported this incident. That could have been fatal for them if someone else found it.  
I have reported their VP R&D and they have fixed the issue within the same day.

-   **Hospitals / Medical supply — Vaccination information of individuals.**

![](https://miro.medium.com/max/1400/1*UFc5Xt8ntcztYs71lnpKSQ.png)

![](https://miro.medium.com/max/1400/1*Dmw_RcLYZ2k6YbFtp7EMZQ.png)

A vaccination date along with an email address

-   **Crypto Trading Platform**

![](https://miro.medium.com/max/1400/1*S1gUPWADQQgZqmKngxJezQ.png)

Crypto Token swaps

-   **Banking  
    **Bank transfer along with full name and bank account details.

![](https://miro.medium.com/max/1400/1*ODVl6BljY4EGUYu3MrmTHA.png)

Bank transfer along with full name and bank account details.

-   **Live car fleet metrics (about each car):  
    **- IMEI (unique cellular identifier)  
    - Location (coordinates)  
    - gsm_strength  
    - fuel status  
    - error codes  
    - battery status

![](https://miro.medium.com/max/1400/1*bZ7plJYsNtWceGV7ZOdBhg.png)

# Is it real?

sometimes I had trouble understanding whether the database is real or a [honeypot](https://en.wikipedia.org/wiki/Honeypot_(computing)).  
**So,** I googled whatever I found in the database.  
Usually, I found the real website and knew who to report this way.

_The document in ElasticSearch:_

![](https://miro.medium.com/max/1400/1*Vmm5MgfqLJ-i4c-y7GzOEA.png)

_The document in the production website, online:_

![](https://miro.medium.com/max/1400/1*7VkVII4x8b6bN7JgowbOog.png)

## Already-Hacked Servers by RansomWare

If your cluster is open to the world, there is no need for a 0-day to run the ransomware. One can access everything if you have no authentication on top of that.

As you can see, some of the assets I’ve discovered were already “hacked” (not hacked, more but malformed, by RansomWare.  
A tool like [elasticsearch-dump](https://github.com/elasticsearch-dump/elasticsearch-dump) can be used to back up (and restore) the database. After it backed up, they deleted everything from the cluster, leaving this message: “_All your data is_ **_a backed_** _up. You must pay 0.16BTC to …_”

![](https://miro.medium.com/max/1400/1*uvsJCtexXhnERzOBBjWjpA.png)

# Security and Privacy Policies

As you can imagine, many of the organizations I found were GDPR Compliant. This means they are sensitive about data breaches, they have a DPO (Data Protection Officer) who actively searches and handles this kind of incident.

![](https://miro.medium.com/max/1400/1*jWRcbF4FQXK5Hz4YUlby1Q.png)

An example of a privacy policy I read (to find the DPO)

![](https://miro.medium.com/max/1400/1*dEdvrN4OG0QsqgMdHXtAIw.png)

“Contact us if you wish to know how your data is stored”.

Some DPOs did not reply, and some of the DPO’s mailboxes’ have rejected my emails with permissions errors (they are not allowed to receive emails outside the organization).

For some companies, the **DMARC** and **SPF** records did not allow emails to be received in their mail servers from outside the company.  
**What a mess, they cannot be reached by email at all.**

I also sent emails to the twin companies and other subsidiaries.  
I tried to contact them through their website contact forms.  
No response whatsoever. I had to start conversations through CEOs, CTOs, and VPS.

As for the ignoring companies — their assets and data are exposed to this day, and they don’t care.

# Conclusion

Publishing CIDR blocks per service is a logical issue when it comes to port scanning. We need it for many reasons — but it poses such a huge risk for cloud providers at the same time, making their customers scannable with ease.

Misconfiguration happens all the time, and it is here to stay, causing many holes in the company's security unknowingly.  
We often sin and use the default VPC subnet when configuring instances. Hence, many instances are assigned with a public IP address automatically.  
The problem starts with the lack of visibility.  
As far as I know, nobody inside of AWS is currently actively searching for misconfigured databases or managed services. It is outside their scope.

**I have reported the companies that I found**, until a certain limit (there were too many), and **I could not reach them all on my own**.  
In an email conversation with **AWS,** they claimed it was the company’s responsibility to configure and secure their asset, and that they are not actively searching for this misconfiguration. This makes sense, yet I found this surprisingly easy, and they can solve it without much effort.  
I have done it on my own, pretty well, in my spare time.

[Click here to learn more information about AWS’s shared responsibility model](https://aws.amazon.com/compliance/shared-responsibility-model/).

At the moment all we can do is assume that misconfiguration is always possible, no matter the company’s size — and that somebody is always seeing you from the network. If you open a service to the world, at least **use decent authorization and authentication**.

By the way, I am doing this in my spare time.  
I also really love coffee!

[

## Avi is making the internet safer in his spare time

### I'm a business-oriented engineer, who loves security and AI, with deep security insights. I like to pwn cloud…

www.buymeacoffee.com



](https://www.buymeacoffee.com/avilum)

Thank you for reading this far.

If you like my works, or if you offer a bug-bounty program, let me know.  
You are welcome to contact me or comment if you have any questions.

Check out my previous releases:

[

## POC For Google Phishing In 10 Minutes: ɢoogletranslate.com

### Back in 2016, I ran into a post about someone buying ɢoogle.com. It was used for phishing proposes (notice the first…

infosecwriteups.com



](https://infosecwriteups.com/poc-for-google-phishing-in-10-minutes-%C9%A2oogletranslate-com-dcd0d2c32d91)

[

## Facebook Knows What You Eat: Discover The Entire Data Facebook Collects About You, Step By Step.

### A story of how I explored https://facebook.com/dyi programmatically.

medium.com



](https://medium.com/digital-diplomacy/facebook-knows-what-you-eat-how-to-visualize-all-the-data-facebook-knows-about-us-c89fe3cb4f89)

[

## Identify Website Users By Client Port Scanning — Using WebAssembly And Go

### Websites tend to scan the open ports of their users, from the browser, to identify new/returning users better. Can…

infosecwriteups.com



](https://infosecwriteups.com/identify-website-users-by-client-port-scanning-using-webassembly-and-go-e9798b4aa05c)

🔈 🔈**Infosec Writeups is organizing its first-ever virtual conference and networking event. If you’re into Infosec, this is the coolest place to be, with 16 incredible speakers and 10+ hours of power-packed discussion sessions.** [**Check more details and register here.**](https://iwcon.live/)

[

## IWCon2022 - Infosec WriteUps Virtual Conference

### Network With World's Best Infosec Professionals. Find How Cybersecurity Pros Achieved Success. Add New Skills to Your…

iwcon.live



](https://iwcon.live/)

706

2

## Sign up for Infosec Writeups

### By InfoSec Write-ups

Newsletter from Infosec Writeups [Take a look.](https://medium.com/bugbountywriteup/newsletters/infosec-writeups?source=newsletter_v3_promo--------------------------newsletter_v3_promo--------------)

Get this newsletter

Emails will be sent to pheintz2@gmail.com.[Not you?](https://medium.com/m/signin?operation=login&redirect=https%3A%2F%2Finfosecwriteups.com%2Fhow-i-discovered-thousands-of-open-databases-on-aws-764729aa7f32&collection=InfoSec+Write-ups&collectionId=7b722bfd1b8d&newsletterV3=Infosec+Writeups&newsletterV3Id=41c32b933ede&source=newsletter_v3_promo--------------------------newsletter_v3_promo----------41c32b933ede----)

## [More from InfoSec Write-ups](https://infosecwriteups.com/?source=post_page-----764729aa7f32-----------------------------------)

Follow

A collection of write-ups from the best hackers in the world on topics ranging from bug bounties and CTFs to vulnhub machines, hardware challenges and real life encounters. In a nutshell, we are the largest InfoSec publication on Medium.

![Guru HariHaraun](https://miro.medium.com/fit/c/24/24/1*xrwoG74XdFg9QBNVm7gtpQ.jpeg)

[

Guru HariHaraun

](https://thegurusec.medium.com/?source=post_page-----764729aa7f32----0-------------------------------)

[

·Jan 22

](https://infosecwriteups.com/how-i-passed-ceh-practical-in-my-first-attempt-647926a3a0ac?source=post_page-----764729aa7f32----0-------------------------------)

[

## How I passed CEH (Practical) in my first attempt by Guru HariHaraun

Hello guys! I’m Guru HariHaraun, 21 years old. In this blog, I will be sharing with you my secret strategy I followed to pass CEH (Practical) examination within 4 hours. …



](https://infosecwriteups.com/how-i-passed-ceh-practical-in-my-first-attempt-647926a3a0ac?source=post_page-----764729aa7f32----0-------------------------------)

[

Certified Ethical Hacker

](https://medium.com/tag/certified-ethical-hacker?source=post_page-----764729aa7f32---------------certified_ethical_hacker--------------------)

[

8 min read

](https://infosecwriteups.com/how-i-passed-ceh-practical-in-my-first-attempt-647926a3a0ac?source=post_page-----764729aa7f32----0-------------------------------)

[

![How I passed CEH (Practical) in my first attempt by Guru HariHaraun](https://miro.medium.com/fit/c/112/112/1*3W5A-Ld9nCjLjN-3H7Lylg.jpeg)

](https://infosecwriteups.com/how-i-passed-ceh-practical-in-my-first-attempt-647926a3a0ac?source=post_page-----764729aa7f32----0-------------------------------)

---

Share your ideas with millions of readers.

[Write on Medium](https://medium.com/new-story?source=post_page_footer_cta_write----------------------------------------)

---

![Abid Ahmad](https://miro.medium.com/fit/c/24/24/0*mZiUKB4QD9A80ESl.jpg)

[

Abid Ahmad

](https://rootintrud3r.medium.com/?source=post_page-----764729aa7f32----1-------------------------------)

[

·Jan 22

](https://infosecwriteups.com/how-i-was-able-to-find-multiple-vulnerabilities-of-a-symfony-web-framework-web-application-2b82cd5de144?source=post_page-----764729aa7f32----1-------------------------------)

[

## How I was able to find multiple vulnerabilities of a Symfony Web Framework web application

Found high severity vulnerability in 5 minutes just from reconnaissance. Found multiple vulnerabilities on a web application that used the Symfony web framework, enabled Symfony profiler/debug mode. — bi-smi llāhi r-raḥmāni r-raḥīmi (In the name of Allah, most gracious and most merciful) Hello! beautiful people, I’m Abid Ahmad, Cyber Security Student & Ethical Hacker. Today I’ll explain how I found multiple vulnerabilities on a web application that used the Symfony Web Framework where Symfony profiler/debug mode was enabled.



](https://infosecwriteups.com/how-i-was-able-to-find-multiple-vulnerabilities-of-a-symfony-web-framework-web-application-2b82cd5de144?source=post_page-----764729aa7f32----1-------------------------------)

[

Bug Bounty

](https://medium.com/tag/bug-bounty?source=post_page-----764729aa7f32---------------bug_bounty--------------------)

[

4 min read

](https://infosecwriteups.com/how-i-was-able-to-find-multiple-vulnerabilities-of-a-symfony-web-framework-web-application-2b82cd5de144?source=post_page-----764729aa7f32----1-------------------------------)

[

![How I was able to find multiple vulnerabilities of a Symfony Web Framework web application](https://miro.medium.com/fit/c/112/112/1*fUmZVp6k17Wcm6Qov--5eg.png)

](https://infosecwriteups.com/how-i-was-able-to-find-multiple-vulnerabilities-of-a-symfony-web-framework-web-application-2b82cd5de144?source=post_page-----764729aa7f32----1-------------------------------)

---

![Mukilan Baskaran](https://miro.medium.com/fit/c/24/24/1*tif1c7lTx883WhLR92OsxA.jpeg)

[

Mukilan Baskaran

](https://mukibas37.medium.com/?source=post_page-----764729aa7f32----2-------------------------------)

[

·Jan 22

](https://infosecwriteups.com/how-to-solve-brooklyn-nine-nine-tryhackme-walkthrough-b0a6e4e2e534?source=post_page-----764729aa7f32----2-------------------------------)

[

## How to solve Brooklyn nine-nine TryHackme Walkthrough

CTF-Walkthrough — Welcome back amazing hackers I came up with another fun and interesting topic walkthrough on Tryhackme(Brooklyn nine-nine). Let’s get into the topic, after deployment as usual I started to scan the target for any useful information.



](https://infosecwriteups.com/how-to-solve-brooklyn-nine-nine-tryhackme-walkthrough-b0a6e4e2e534?source=post_page-----764729aa7f32----2-------------------------------)

[

Ctf

](https://medium.com/tag/ctf?source=post_page-----764729aa7f32---------------ctf--------------------)

[

3 min read

](https://infosecwriteups.com/how-to-solve-brooklyn-nine-nine-tryhackme-walkthrough-b0a6e4e2e534?source=post_page-----764729aa7f32----2-------------------------------)

[

![How to solve Brooklyn nine-nine TryHackme Walkthrough](https://miro.medium.com/fit/c/112/112/0*5L0XhfJbMVnvuswb)

](https://infosecwriteups.com/how-to-solve-brooklyn-nine-nine-tryhackme-walkthrough-b0a6e4e2e534?source=post_page-----764729aa7f32----2-------------------------------)

---

![Ravi Teja](https://miro.medium.com/fit/c/24/24/1*f-uCIcDfbbnpjZkc7m_log.jpeg)

[

Ravi Teja

](https://rt007.medium.com/?source=post_page-----764729aa7f32----3-------------------------------)

[

·Jan 22

](https://infosecwriteups.com/demystifying-ja3-one-handshake-at-a-time-c80b04ccb393?source=post_page-----764729aa7f32----3-------------------------------)

[

## Demystifying JA3: One Handshake at a Time

Recently, I was browsing a website with BurpSuite and found out that the website was blocking my requests. In the pursuit of unlocking the mystery of how, I have stumbled across an incredible TLS fingerprinting technique called JA3. Backdrop Fingerprinting clients and blocking them based on a particular set of rules…



](https://infosecwriteups.com/demystifying-ja3-one-handshake-at-a-time-c80b04ccb393?source=post_page-----764729aa7f32----3-------------------------------)

[

Bug Bounty

](https://medium.com/tag/bug-bounty?source=post_page-----764729aa7f32---------------bug_bounty--------------------)

[

8 min read

](https://infosecwriteups.com/demystifying-ja3-one-handshake-at-a-time-c80b04ccb393?source=post_page-----764729aa7f32----3-------------------------------)

[

![Demystifying JA3: One Handshake at a Time](https://miro.medium.com/fit/c/112/112/1*UjuGnWDV4l3FPRuvgAma6g.jpeg)

](https://infosecwriteups.com/demystifying-ja3-one-handshake-at-a-time-c80b04ccb393?source=post_page-----764729aa7f32----3-------------------------------)

---

![Ayush Verma](https://miro.medium.com/fit/c/24/24/0*IiRMP9ujfhCQ7KJu)

[

Ayush Verma

](https://3xabyt3.medium.com/?source=post_page-----764729aa7f32----4-------------------------------)

[

·Jan 21

](https://infosecwriteups.com/day-17-web-reconnaissance-or-information-gathering-part-2-100daysofhacking-323ecea7f0a3?source=post_page-----764729aa7f32----4-------------------------------)

[

## Day 17, Web Reconnaissance Or Information Gathering — Part 2#100DaysofHacking

Get all the writeups from Day 1 to 16, Click Here Or Click Here. In our previous blog we learned about google dorking and how should we use target manually as a user, in this blog we’ll learn about some further techniques. Scope Discovery Scope discovery means which of the…



](https://infosecwriteups.com/day-17-web-reconnaissance-or-information-gathering-part-2-100daysofhacking-323ecea7f0a3?source=post_page-----764729aa7f32----4-------------------------------)

[

Pentesting

](https://medium.com/tag/pentesting?source=post_page-----764729aa7f32---------------pentesting--------------------)

[

5 min read

](https://infosecwriteups.com/day-17-web-reconnaissance-or-information-gathering-part-2-100daysofhacking-323ecea7f0a3?source=post_page-----764729aa7f32----4-------------------------------)

[

![Day 17, Web Reconnaissance Or Information Gathering — Part 2#100DaysofHacking](https://miro.medium.com/fit/c/112/112/0*Tw6igJzOoensC0lo)

](https://infosecwriteups.com/day-17-web-reconnaissance-or-information-gathering-part-2-100daysofhacking-323ecea7f0a3?source=post_page-----764729aa7f32----4-------------------------------)

---