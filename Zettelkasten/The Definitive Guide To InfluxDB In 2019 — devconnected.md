[![Antoine Solnichkin](https://miro.medium.com/fit/c/96/96/1*pxYU2qKS1aiCE6qN4-kLGg@2x.jpeg)](https://medium.com/@solnichkin.antoine?source=post_page-----23f5661002c8-----------------------------------)

[Antoine Solnichkin](https://medium.com/@solnichkin.antoine?source=post_page-----23f5661002c8-----------------------------------)


# The Definitive Guide To InfluxDB In 2019 — devconnected

InfluxDB is definitely a fast-growing technology. The time series database, developed by **InfluxData**, is seeing its popularity grow more and more over the past few months. It has become one of the references for developers and engineers willing to bring **live-monitoring** into their own infrastructure.

![](https://miro.medium.com/max/1400/1*uRPKeLMICmzXdMgfusXXwg.png)

_But what exactly is InfluxDB? Why do you need it? What value can you create by incorporating InfluxDB into your own environment?_

This article is the starting point for every **developer**, **engineer** and **IT professional** that wants to learn InfluxDB, its concepts, use-cases and real-world applications.

The goal is to make you proficient with InfluxDB in under one week. We have segmented the InfluxDB learning paths into different modules, each one of them bringing a new level of understanding of time-series databases.

First, you will get **an overall presentation of time-series databases**, what they are, and how they differ from traditional relational databases.

Afterwards, you will be presented with **an in-depth explanation of the concepts that define InfluxDB**. Unfamiliar with measurements, tags or fields? Everything is explained.

Finally, you will see **the use-cases of InfluxDB** and how it can be used in a variety of industries. You will see real-world examples that you can emulate to bring value to your company or project.

# Module 1 — What are Time-Series Databases?

Time Series Databases, as their name state, are **database systems** specifically designed to handle **time-related data**.

All your life, you have dealt with relational databases like MySQL, or SQL Server. You may also have dealt with NoSQL databases like MongoDB or DynamoDB.

Those systems are based on the fact that you have tables. Those tables contain columns and rows, each one of them defining an entry in your table. Often, those tables are specifically designed for a purpose : one may be designed to store users, another one for photos and finally for videos. Such systems are efficient, scalable and used by plenty of giant companies having million of requests on their servers.

Time series databases work differently. Data are still stored in ‘collections’ but those collections share a common denominator : **they are aggregated over time**.

![](https://miro.medium.com/max/1400/1*nCUS7ExQ5aknRKPO-CGDyg.png)

The great difference between relational databases and time series databases

Essentially, it means that for every point that you are able to store, you have a **timestamp** associated with it.

The great difference between relational databases and time series databases

> But.. couldn’t we use a relational database and simply have a column named ‘time’? Oracle for example includes a TIMESTAMP data type that we could use for that purpose.

You could, but that would be **inefficient**.

# a — Why do we need time series databases?

Three words : **fast ingestion rate.**

Time series databases systems are built around the predicate that they need to ingest data in a fast and efficient way.

Indeed, relational databases do have a fast ingestion rate for most of them, from 20k to 100k rows per second.

However, the ingestion is not constant over time. Relational databases have one key aspect that make them slow when data tend to grow : **indexes**.

When you add new entries to your relational database, and if your table contains indexes, your database management system will repeatedly re-index your data for it to be accessed in a fast and efficient way.

As a consequence, **the performance of your DBMS tend to decrease over time**. The load is also increasing over time, resulting in having difficulties to read your data.

Time series database are optimized for a **fast ingestion rate**. It means that such index systems are optimized to index data that are aggregated over time : as a consequence, the ingestion rate does not decrease over time and stays quite stable, around **50k to 100k lines per second on a single node**.

![](https://miro.medium.com/max/1400/1*2Ez-njS9KjCZYKeceRN65Q.png)

This graph is inspired by :  
[https://blog.timescale.com/timescaledb-vs-6a696248104e/](https://blog.timescale.com/timescaledb-vs-6a696248104e/)

# b — Specific concepts about time series databases

On top of the fast ingestion rate, time series databases introduce concepts that are very specific to those technologies.

One of them is **data retention**. In a traditional relational database, data are stored permanently until your decide to drop them yourself.

Given the use-cases of time series databases, you may want not to keep your data for too long : either because it is **too expensive** to do so, or because you are not that interested in old data.

Systems like InfluxDB can take care of dropping data after a certain time, with a concept called **retention policy** (explained in details in part two). You can also decide to run **continuous queries** on live data in order to perform certain operations.

You could find equivalent operations in a relational database, for example ‘jobs’ in SQL that can run on a given schedule.

# c — A Whole Different Ecosystem

Time series databases are very different when it comes to the ecosystem that orbits around them. In general, relational databases are surrounded by applications : web applications, softwares that connect to it to retrieve information or add new entries.

Often, a database is associated with one system. Clients connect to a website, that contacts a database in order to retrieve information. TSDB are built for **client plurality** : you do not have a simple server accessing the database, but a bunch of different sensors (for example) inserting their data at the same time.

As a consequence, tools were designed in order to have efficient ways to **produce data or to consume it**.

## Data consumption

Data consumption is often done via monitoring tools such as **Grafana** or **Chronograf**. Those solutions have built-in solutions to visualize data and even make custom alerts with it.

![](https://miro.medium.com/max/1400/1*NTzl-dLmKuqYJpOS5b1Wbw.png)

The data consumers for TSDB

Those tools are often used to create live dashboards that may be graphs, bar charts, gauges or live world maps.

## Data Production

Data production is done by agents that are responsible for targeting special elements in your infrastructure and extract metrics from them. Such agents are called “ **monitoring agents**”. You can easily configure them to query your tools on a given time span. Examples are **Telegraf** (which is an official monitoring agent), **CollectD** or **StatsD**

![](https://miro.medium.com/max/1400/1*V9uhiErhW2X6U1Evfw_QVQ.png)

The data producers for TSDB

Now that you have a better understanding of what time series databases are and how they differ from relational databases, it is time to dive into the **specific concepts of InfluxDB**.

# Module 2 — InfluxDB Concepts Explained

In this section, we are going to explain the key concepts behind InfluxDB and the key query associated with it. InfluxDB embeds its **own query language** and I think that this point deserves a small explanation.

# a — InfluxDB Query Language

Before starting, it is important for you to know which version of InfluxDB you are currently using. As of April 2019, InfluxDB comes in two versions : **v1.7+ and v2.0**.

v2.0 is currently in alpha version and puts the **Flux** language as a centric element of the platform. v1.7 is equipped with **InfluxQL** language (and Flux if you activate it).

![](https://miro.medium.com/max/1400/1*99tYLSPRwxWoB8ST3fuhNg.png)

> _Right now, I do recommend to keep on using InfluxQL as Flux is not completely established in the platform._

**InfluxQL** is a query language that is very similar to SQL and that allows any user to query its data and filter it. Here’s an example of an InfluxQL query :

In the following sections, we are going to explore **InfluxDB key concepts**, provided with the associated IQL (short for InfluxQL) queries.

# b — InfluxDB Key Concepts Explained

![](https://miro.medium.com/max/1238/1*OjhJB1fGSwI1rHSCgA6CCQ.png)

In this section, we will go through the list of essential terms to know to deal with InfluxDB in 2019.

## Database

A **database** is a fairly simple concept to understand on its own because you are used to use this term with relational databases. In a SQL environment, a database would host a collection of tables, and even schemas and would represent one instance on its own.

In InfluxDB, **a database host a collection of measurements**. However, a single InfluxDB instance can host multiple databases. This is where it differs from traditional database systems. This logic is detailed in the graph below :

![](https://miro.medium.com/max/1400/1*0rbtDPwhP_i0qgocwji5Lw.png)

The most common ways to interact with databases are either **creating a database** or by **navigating into a database** in order to see collections (you have to be “in a database” in order to query collections, otherwise it won’t work).

![](https://miro.medium.com/max/1008/1*mhsmkW-vTBywM_BzcjTGLQ.png)

Most used Influx database queries

## Measurement

As shown in the graph above, a database stores multiple measurements. You could think of a measurement as a SQL table. **It stores data, and even meta data, over time**. Data that are meant to coexist together should be stored in the same measurement.

![](https://miro.medium.com/max/1324/1*cd2-nr81mva8RdaL3HlXkQ.png)

Measurement example

![](https://miro.medium.com/max/1400/1*y_B0ZrkEoOb7eqdLPvkBxg.png)

Measurement IFQL example

In a SQL world, data are stored in columns, but in InfluxDB we have two other terms : **tags & fields.**

## Tags & Fields

**Warning**!

This is a very important chapter as it explains the subtle difference between tags & fields.

When I first started with InfluxDB, I had a hard time grasping exactly why are tags & fields different. For me, they represented ‘columns’ where you could store exactly the same data.

When defining a new ‘column’ in InfluxDB, you have the choice to either declare it **as a tag or as a value** and it makes a very big difference.

In fact, the biggest difference between the two is **that tags are indexed and values are not**. Tags can be seen as **metadata** defining our data in the measurement. They are hints giving additional information about data, but not data itself.

Fields, on the other side, is literally data. In our past example, the temperature ‘column’ would be a field.

Back to our cpu_metrics example, let’s say that we wanted to add a column named ‘location’ as its name states, defines where the sensor is.

_Should we add it as a tag or a field?_

![](https://miro.medium.com/max/1400/1*KyD1Czqssg2fnhfj0jC8JQ.png)

In our case, it would be added as a.. **tag**! We definitely want the location ‘column’ to be **indexed** and taken into account when performing a query over the location.

In general, I would advise to keep your measurements relatively small when it comes to the number of fields. More and more fields often rhymes with lower performance. You could create other measurements to store another field and index it properly.

Now that we’ve added the location tag to our measurement, let’s go a bit deeper into the taxonomy.

A set of tags is called a **“tag-set”**. The ‘column name’ of a tag is called a **“tag key”**. Values of a tag are called **“tag values”**. The same taxonomy repeats for fields. Back to our drawings.

![](https://miro.medium.com/max/906/1*6YOCMW7GzPAO3cqEKl4_3A.png)

Measurement taxonomy

## Timestamp

Probably the simplest keyword to define. A timestamp in InfluxDB is **a date and a time defined in RFC3339 format**. When using InfluxDB, it is very common to define your time column as a timestamp in Unix time expressed in nano seconds.

> _Tip : you can choose a nanosecond format for the time column and reduce the precision later by adding trailing zeros to your time value for it to fit the nanosecond format._

## Retention policy

This feature of InfluxDB is for me one of the best features there is.

**A retention policy defines how long you are going to keep your data**. Retention policies are defined per database and of course you can have multiple of them. By default, the retention policy is ‘ **autogen**’ and will basically keep your data forever. In general, databases have multiple retention policies that are used for **different purposes**.

![](https://miro.medium.com/max/1400/1*rIPpDxkbTR0grBG_RR0Y0A.png)

How retention policies work

> _What are the typical use-cases of retention policies?_

Let’s pretend that you are using InfluxDB for live monitoring of an entire infrastructure.

You want to be able to detect when a server goes off for example. In this case, you are interested in data coming from that server in the present or short moments before. You are not interested in keeping the data for several months, as a consequence you want to define **a small retention policy** : one or two hours for example.

Now if you are using InfluxDB for IoT, capturing data coming from a water tank for example. Later, you want to be able to share your data with the data science team for them to analyze it. In this case, you might want to **keep data for a longer time** : five years for example.

## Point

Finally, an easy one to end this chapter about InfluxDB terms. **A point is simply a set of fields that has the same timestamp**. In a SQL world, it would be seen as a row or as a unique entry in a table. Nothing special here.

> _Congratulations on making it so far! In the next chapter, we are going to see the different use-cases of InfluxDB and how it can be used to take your company to the next level._

# Module 3 — InfluxDB Use-Cases

# DevOps Monitoring

**DevOps Monitoring** is a very big subject nowadays. More and more teams are investing in building fast and reliable architectures that revolve around monitoring. From services to clusters of servers, it is very common for engineers to build a monitoring stack that provides smart alerts.

If you are interested in learning more about DevOps Monitoring, I wrote a couple of guides on the subject, you might find them relevant to your needs.

[

## Monitoring systemd services in realtime with Chronograf - devconnected

### Every system administrator knows systemd. Developed by Lennart Poettering and freedesktop.org, systemd comes as a very…

devconnected.com



](http://devconnected.com/monitoring-systemd-services-in-realtime-with-chronograf/)

From the tools defined in section 1, you could build your own monitoring infrastructure and bring direct value to your company or start-up.

# IoT World

The IoT is probably the next big revolution that is coming in the next few years. By 2020, it is estimated that over **30 billion devices will be considered IoT devices**. Whether you are monitoring a single device or a giant network of IoT devices, you want to have accurate and instant metrics for you to take the best decisions regarding the goal you are trying to achieve.

Real companies are already working with InfluxDB for IoT. One example would be **WorldSensing**, a company that aims at expanding smart cities via individual concepts such as smart parking or traffic monitoring system. Their website is available here : [https://www.worldsensing.com/](https://www.worldsensing.com/)

# Industrial & Smart Plants

Plants are becoming more and more connected. Tasks are more automated than ever : as a consequence it brings an obvious need to be able to monitor every piece of the production chain to ensure a maximal throughput. But even when machines are not doing all the work and humans are involved, **time-series monitoring a unique opportunity to bring relevant metrics to managers.**

Besides reinforcing productivity, they can contribute to building **safer workplaces** as they are able to detect issues quicker. **Value for managers as well as for workers**.

# Your Own Imagination!

The examples detailed above are just examples and your imagination is the only limit to the applications that you can find for time series databases. I have shown it via some articles that I wrote, but time-series can be even used in [**cybersecurity**!](http://devconnected.com/geolocating-ssh-hackers-in-real-time/)

If you have cool applications of InfluxDB or time-series database, post them as comment below, it is interesting to see what idea people can come up with.

# Module X — Going Further

In this article, you learned many different concepts : **what are time-series databases** and how they are used in the real world. We have gone through a complete list of all the **technical terms behind InfluxDB**, and I am confident now to say that you are to go on your own adventure.

My advice to you right now would be **to build something on your own**. [Install it](https://docs.influxdata.com/influxdb/v1.7/introduction/installation/), play with it, and start bringing value to your company or start-up today.

**Create a dashboard, play with queries, setup some alerts** : there are many things that you will have to do in order to complete your InfluxDB journey.

If you need some inspiration to go further, you can check the other articles that we wrote on the subject : they provide clear step by step guides on how to setup everything.

Something you want me to write about in the next articles?

Leave a comment below with your idea. 💡

Until next time, have fun, as always.

_Originally published at_ [_http://devconnected.com_](http://devconnected.com/the-definitive-guide-to-influxdb-in-2019/) _on April 18, 2019._

334

3

## [More from devconnected — DevOps, Sysadmins & Engineering](https://medium.com/schkn?source=post_page-----23f5661002c8-----------------------------------)

Follow

Tutorials & Guides for DevOps, sysadmins and software engineers.

![Antoine Solnichkin](https://miro.medium.com/fit/c/24/24/1*pxYU2qKS1aiCE6qN4-kLGg@2x.jpeg)

[

Antoine Solnichkin

](https://medium.com/@solnichkin.antoine?source=post_page-----23f5661002c8----0-------------------------------)

[

·Apr 8, 2019

](https://medium.com/schkn/monitoring-a-server-cluster-using-grafana-and-influxdb-d5ff5f7151b2?source=post_page-----23f5661002c8----0-------------------------------)

[

## Monitoring a server cluster using Grafana and InfluxDB

Six billion requests per month. Does this number sound large to you? This number is the total number of requests made in a month on a very popular website that every developer knows : StackOverflow. Two HAProxy servers and nine Web Servers are the building blocks of StackOverflow, implementing a…



](https://medium.com/schkn/monitoring-a-server-cluster-using-grafana-and-influxdb-d5ff5f7151b2?source=post_page-----23f5661002c8----0-------------------------------)

[

Dev Ops

](https://medium.com/tag/devops?source=post_page-----23f5661002c8---------------devops--------------------)

[

9 min read

](https://medium.com/schkn/monitoring-a-server-cluster-using-grafana-and-influxdb-d5ff5f7151b2?source=post_page-----23f5661002c8----0-------------------------------)

[

![Monitoring a server cluster using Grafana and InfluxDB](https://miro.medium.com/fit/c/112/112/1*EGrGVCnU907tcT_Yj8H7UA.png)

](https://medium.com/schkn/monitoring-a-server-cluster-using-grafana-and-influxdb-d5ff5f7151b2?source=post_page-----23f5661002c8----0-------------------------------)

---

Share your ideas with millions of readers.

[Write on Medium](https://medium.com/new-story?source=post_page_footer_cta_write----------------------------------------)

---

![Antoine Solnichkin](https://miro.medium.com/fit/c/24/24/1*pxYU2qKS1aiCE6qN4-kLGg@2x.jpeg)

[

Antoine Solnichkin

](https://medium.com/@solnichkin.antoine?source=post_page-----23f5661002c8----1-------------------------------)

[

·Mar 21, 2019

](https://medium.com/schkn/building-an-audio-visualizer-for-razer-chroma-keyboards-7814cab950ff?source=post_page-----23f5661002c8----1-------------------------------)

[

## Building an Audio Visualizer for Razer Chroma Keyboards

The sound is a really fascinating phenomenon. As a human, without even knowing it, you are able to decode and interpret thousands of different sounds from thousands different sources. …



](https://medium.com/schkn/building-an-audio-visualizer-for-razer-chroma-keyboards-7814cab950ff?source=post_page-----23f5661002c8----1-------------------------------)

[

Programming

](https://medium.com/tag/programming?source=post_page-----23f5661002c8---------------programming--------------------)

[

11 min read

](https://medium.com/schkn/building-an-audio-visualizer-for-razer-chroma-keyboards-7814cab950ff?source=post_page-----23f5661002c8----1-------------------------------)

[

![Building an Audio Visualizer for Razer Chroma Keyboards](https://miro.medium.com/freeze/fit/c/112/112/1*IHXMsRjbaE0EClCLnmMgQw.gif)

](https://medium.com/schkn/building-an-audio-visualizer-for-razer-chroma-keyboards-7814cab950ff?source=post_page-----23f5661002c8----1-------------------------------)

---

![Antoine Solnichkin](https://miro.medium.com/fit/c/24/24/1*pxYU2qKS1aiCE6qN4-kLGg@2x.jpeg)

[

Antoine Solnichkin

](https://medium.com/@solnichkin.antoine?source=post_page-----23f5661002c8----2-------------------------------)

[

·Feb 11, 2019

](https://medium.com/schkn/monitoring-systemd-services-in-realtime-with-chronograf-285c650c1a73?source=post_page-----23f5661002c8----2-------------------------------)

[

## Monitoring systemd services in realtime with Chronograf

Every system administrator knows systemd. Developed by Lennart Poettering and freedesktop.org, systemd comes as a very handy initialization tool for administrating Linux services and handling dependencies. Most of the modern tools are shipped as systemd services and managed from there. But what happens when one of the services fails? Most…



](https://medium.com/schkn/monitoring-systemd-services-in-realtime-with-chronograf-285c650c1a73?source=post_page-----23f5661002c8----2-------------------------------)

[

Dev Ops

](https://medium.com/tag/devops?source=post_page-----23f5661002c8---------------devops--------------------)

[

8 min read

](https://medium.com/schkn/monitoring-systemd-services-in-realtime-with-chronograf-285c650c1a73?source=post_page-----23f5661002c8----2-------------------------------)

[

![Monitoring systemd services in realtime with Chronograf](https://miro.medium.com/fit/c/112/112/1*nDF6QTBVL5KQMYr9Ahcu0A.png)

](https://medium.com/schkn/monitoring-systemd-services-in-realtime-with-chronograf-285c650c1a73?source=post_page-----23f5661002c8----2-------------------------------)

---

![Antoine Solnichkin](https://miro.medium.com/fit/c/24/24/1*pxYU2qKS1aiCE6qN4-kLGg@2x.jpeg)

[

Antoine Solnichkin

](https://medium.com/@solnichkin.antoine?source=post_page-----23f5661002c8----3-------------------------------)

[

·Jan 26, 2019

](https://medium.com/schkn/geolocating-ssh-hackers-in-real-time-108cbc3b5665?source=post_page-----23f5661002c8----3-------------------------------)

[

## Geolocating SSH Hackers In Real-Time

The rise of the machines has arrived. While you’re reading this article, thousands if not hundreds of thousands of cyberattacks are performed. Some of them are more sophisticated than others : from trojans, phishing attempts, malware infections to botnets attacks (also known as DDoS), cyberattacks are literally everywhere. Today we…



](https://medium.com/schkn/geolocating-ssh-hackers-in-real-time-108cbc3b5665?source=post_page-----23f5661002c8----3-------------------------------)

[

Security

](https://medium.com/tag/security?source=post_page-----23f5661002c8---------------security--------------------)

[

8 min read

](https://medium.com/schkn/geolocating-ssh-hackers-in-real-time-108cbc3b5665?source=post_page-----23f5661002c8----3-------------------------------)

[

![Geolocating SSH Hackers In Real-Time](https://miro.medium.com/fit/c/112/112/1*EqVRIV9ktx1u9t3fpsyktw.png)

](https://medium.com/schkn/geolocating-ssh-hackers-in-real-time-108cbc3b5665?source=post_page-----23f5661002c8----3-------------------------------)

---

![Antoine Solnichkin](https://miro.medium.com/fit/c/24/24/1*pxYU2qKS1aiCE6qN4-kLGg@2x.jpeg)

[

Antoine Solnichkin

](https://medium.com/@solnichkin.antoine?source=post_page-----23f5661002c8----4-------------------------------)

[

·Dec 28, 2018

](https://medium.com/schkn/4-best-time-series-databases-to-watch-in-2019-ef1e89a72377?source=post_page-----23f5661002c8----4-------------------------------)

[

## 4 Best Time Series Databases To Watch in 2019

When developing IoT, financial or industrial applications, the choice of a good time series database is most of the time a headache, choosing between the 30+ (and growing) list of time series vendors in the industry. When choosing a time series database, it is best to know what they have…



](https://medium.com/schkn/4-best-time-series-databases-to-watch-in-2019-ef1e89a72377?source=post_page-----23f5661002c8----4-------------------------------)

[

Big Data

](https://medium.com/tag/big-data?source=post_page-----23f5661002c8---------------big_data--------------------)

[

7 min read

](https://medium.com/schkn/4-best-time-series-databases-to-watch-in-2019-ef1e89a72377?source=post_page-----23f5661002c8----4-------------------------------)