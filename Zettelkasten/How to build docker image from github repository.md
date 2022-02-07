[![Dave Sugden](https://miro.medium.com/fit/c/96/96/0*g6e7vddVgpVJvQRf)](https://davelms.medium.com/?source=post_page-----394f4c1679cc-----------------------------------)

[Dave Sugden](https://davelms.medium.com/?source=post_page-----394f4c1679cc-----------------------------------)

![](https://miro.medium.com/max/700/1*uB6UypHfUDHAZySPtOYSEw.png)

# Build your Docker images automatically when pushing new code to GitHub

## In this tutorial, we will explore two techniques to automatically build and publish Docker images to Docker Hub every time you push new code into your GitHub source code repository.

# Overview

This article will outline two techniques for automating the build of Docker images after every push or merge to the master branch in **GitHub**. The built images will be pushed to a **Docker Hub** container repository.

The two approaches being demonstrated are;

1.  using Docker Hub – use GitHub webhooks to notify Docker Hub about code changes and trigger the build of a new Docker image within Docker Hub itself.
2.  using GitHub Actions – using GitHub’s service for running Continuous Integration pipelines, we will build the new Docker image within GitHub machines and push the image to Docker Hub.

# Prerequisites

To begin this tutorial, you will need to set yourself up with accounts on GitHub and Docker Hub.

1. **GitHub** account ([register here](https://github.com/))

Within your GitHub account, create a repository with a _Dockerfile_ and any additional source code. I’m using a simple Hello World example for the purposes of this article.

![](https://miro.medium.com/max/586/1*9OxFVOa7zDEJ7WMKfydW4Q.png)

“Hello World” Dockerfile

2. **Docker Hub** account ([register here](https://hub.docker.com/))

# Approach #1 — using Docker Hub to build image

In the first approach, we will configure Docker Hub to receive notifications from GitHub whenever there are any changes to our source repository. This is achieved using GitHub webhooks. On receipt of the notification, Docker Hub will build the new Docker image and publish it for consumption.

![](https://miro.medium.com/max/700/1*dEGEmjaZhzxpDvfoUNEUig.png)

## Step 1. Associate your GitHub and Docker Hub accounts.

Within Docker Hub visit [Account Settings > Linked Accounts](https://hub.docker.com/settings/linked-accounts) and click “Connect” to allow access to your source repositories.

![](https://miro.medium.com/max/700/1*ftKLE06pz8D0hhjTqjLYYg.png)

## Step 2. Create container repository in Docker Hub

Within Docker Hub [create a new repository](https://hub.docker.com/repository/create) and under “Build Settings” click on the GitHub icon to associate your source code repository.

![](https://miro.medium.com/max/700/1*8uaFAkWDCxcETg-qNBr14g.png)

From the drop-down select your GitHub _organisation_ (this will default to your username) and the _source code repository_.

## Step 3. Configure Build Rules

There are many options available, and several examples are displayed in the help. For the purpose of this demo, we’ll keep the default settings and set up the trigger to be on pushes to the _master_ branch and use the _latest_ docker tag.

![](https://miro.medium.com/max/700/1*Ti4keGwnPv8wKijcGvOSHg.png)

Other options exist to create Docker images from tags or release branches, and the pattern matching allows you to create dynamic image tags.

![](https://miro.medium.com/max/700/1*GqvzBnZ7QG4i5ZwQqUH1nA.png)

Example Build Rule options

Click “Create & Build” to set up your new container repository and build your first Docker image — the first build will trigger automatically.

## Step 5. Viewing builds.

Within Docker Hub you will find the build status in the “General” tab of your repository. Viewing the “Builds” tab will show more information about each build. From here you will see the status of jobs and view build logs.

![](https://miro.medium.com/max/700/1*b-6wPWld7fh1R4DfyaOC8A.png)

Once built we can pull and run the newly built image.

**$ docker run davelms/hello-world:latest**  
Hello world!

## Step 6. Push new source code changes

Final step is to simulate a code change — I’ll do this by editing the “Hello world!” message — and push the new commit up to GitHub.

GitHub will send a notification to Docker Hub about the code change; Docker Hub will initiate a new build; and we can run that new image once completed.

![](https://miro.medium.com/max/700/1*0W5ftoBz20IlgVYE2anAHw.png)

GitHub commit log

Immediately after pushing the code, we see a new build initiated in Docker Hub. You can see that the commit sha is referenced — tip: you can use this in tags should you wish instead of “latest”.

![](https://miro.medium.com/max/700/1*eU9ncv-vCLZWNp0r6W-How.png)

Docker Hub build history

When the build has finished, update locally with `docker pull` and re-run your container. This time we see the output from our new source code.

**$ docker run davelms/hello-world  **       
Hello Dave!

# Approach #2 — using GitHub Actions to build Docker images

This time we will use GitHub’s custom CI/CD platform — GitHub Actions — to build the Docker image after every push to the source code repository. The workflow will define a single job that builds the image and pushes the new image to Docker Hub.

![](https://miro.medium.com/max/700/1*-rtCRt_wgq4Uy00ar92e2g.png)

## Step 1. Create a Docker Hub security access token

First of all, within Docker Hub create yourself an access token by visiting [Settings > Security](https://hub.docker.com/settings/security). Give it a name you recognise it later.

![](https://miro.medium.com/max/700/1*owl44toezrvA6__rqOZHEw.png)

Once created, head over to GitHub and create secrets within your source code repository for **DOCKER_HUB_USERNAME** and **DOCKER_HUB_TOKEN**, along with **DOCKER_HUB_REPOSITORY**.

![](https://miro.medium.com/max/700/1*DjcwigVU61qOdRBgWKCCOQ.png)

## Step 2. Create a GitHub Action to build and push images

Keeping within GitHub, head into the “Actions” tab of your source code repository. It’s likely that it will have detected the Dockerfile and will recommend you Docker-related workflow examples to get you started.

Since I will share an example, skip these helpers for now and select “set up a workflow yourself”. Use the workflow definition below and commit the file.

name: Docker Image CI  
  
on:  
  push:  
    branches: [ master ]  
  
jobs:  
  
  build:  
  
    runs-on: ubuntu-latest  
  
    steps:  
    - uses: actions/checkout@v2  
    - name: Build the Docker image  
      run: |  
        echo "${{ secrets.DOCKER_HUB_TOKEN }}" | docker login -u "${{ secrets.DOCKER_HUB_USERNAME }}" --password-stdin docker.io  
        docker build . --file Dockerfile --tag docker.io/${{ secrets.DOCKER_HUB_USERNAME }}/${{ secrets.DOCKER_HUB_REPOSITORY }}:$GITHUB_SHA  
        docker push docker.io/${{ secrets.DOCKER_HUB_USERNAME }}/${{ secrets.DOCKER_HUB_REPOSITORY }}:$GITHUB_SHA

**What does this CI pipeline do?**

1.  Define that we will trigger a job after all pushes to master branch.
2.  Configures a single job called “build” to run on an Ubuntu machine.
3.  The source code repository will be checked out and will run three inline commands: 1.`docker login` 2. `docker build` 3. `docker push`. These will use our secrets we saved off earlier.
4.  Note that the example uses the commit sha as the image tag — GitHub Actions makes the sha available in an environment variable called `GITHUB_SHA`. Of course, you could stick with “latest” like we did in the first example should you wish to do so.

## Step 3. View the build

When the build has finished, the image will appear over on Docker Hub.

![](https://miro.medium.com/max/700/1*Ds1R5C2NSlseKbcql801Vg.png)

Now verify you can pull the new image down successfully.

**$ docker run davelms/hello-world:8517bf9c40f4cf198ea3313bd5ec3cc43176bd85**       
Unable to find image 'davelms/hello-world:8517bf9c40f4cf198ea3313bd5ec3cc43176bd85' locally  
8517bf9c40f4cf198ea3313bd5ec3cc43176bd85: Pulling from davelms/hello-world  
d9cbbca60e5f: Already exists  
Digest: sha256:f8808c8b2ae19f6f3700e51a127e04d8366a1285bdfc6e4006092807f0eced1b  
Status: Downloaded newer image for davelms/hello-world:8517bf9c40f4cf198ea3313bd5ec3cc43176bd85  
**Hello Dave!**

## Step 4. Push new source code changes

Final step is to simulate a code change — I’ll do this by editing the message back to “Hello world!” — and push the new commit up to GitHub.

![](https://miro.medium.com/max/700/1*5AB7GPML-9mZjzQHUYkjfw.png)

Commit log

After the push, a new CI/CD workflow containing our “build” job is run.

![](https://miro.medium.com/max/700/1*rTM2p-8on6CqYISK5G9Sww.png)

CI Pipeline history

The new image will have been pushed to Docker Hub successfully.

![](https://miro.medium.com/max/700/1*EKQUg0WkIWF-sIUcTC6Qhw.png)

This time we see the output from our new source code — the message is back to “Hello World!”. All is working as expected.

**$ docker run davelms/hello-world:a26c53183fac84a9c7ce128ce6ae6250fae26c7d **      
Unable to find image 'davelms/hello-world:a26c53183fac84a9c7ce128ce6ae6250fae26c7d' locally  
a26c53183fac84a9c7ce128ce6ae6250fae26c7d: Pulling from davelms/hello-world  
d9cbbca60e5f: Already exists  
Digest: sha256:3cfa80d4fa8c51271928f0294d89293a7a7fc7022a416a1758fc37394bc12808  
Status: Downloaded newer image for davelms/hello-world:a26c53183fac84a9c7ce128ce6ae6250fae26c7d  
**Hello World!**

# Conclusion

In this article we walked through two techniques for automating the build of Docker images after every source code change in GitHub. In both examples, we pushed the images into Docker Hub.

Firstly, we used webhooks and built the image directly within **Docker Hub** where the “free plan” offers the ability to create images (the limitation with this plan is that it limits to building one image at a time).

Next, we used **GitHub Actions** to build a Continuous Integration pipeline and push the built image to Docker Hub — we could extend this pipeline later to include some unit and integration tests. GitHub Actions provides 2,000 minutes of machine time per month under its “free plan”.

Thank you for reading this article — I hope you found it useful — you can follow me on [Twitter](https://twitter.com/davelms) and connect on [LinkedIn](https://www.linkedin.com/in/davelms/).

[

## Dave Sugden

### The latest Tweets from Dave Sugden (@davelms). DevOps & Football. #LUFC & #Scarborough Write tech to…

twitter.com



](https://twitter.com/davelms)

3

## Get an email whenever Dave Sugden publishes.

Subscribe

Emails will be sent to pheintz2@gmail.com.[Not you?](https://medium.com/m/signin?operation=login&redirect=https%3A%2F%2Fdavelms.medium.com%2Fbuild-your-docker-images-automatically-when-pushing-new-code-to-github-394f4c1679cc&newsletterV3=89619766745b&newsletterV3Id=56b4e8ba4935&source=newsletter_v3_promo--------------------------subscribe_user----------56b4e8ba4935----)

## [More from Dave Sugden](https://davelms.medium.com/?source=post_page-----394f4c1679cc-----------------------------------)

Follow

DevOps | SRE | AWS | GCP https://twitter.com/davelms

[

May 10, 2020

](https://davelms.medium.com/five-minutes-create-a-free-static-website-using-google-sites-58573a927fa2?source=post_page-----394f4c1679cc----0-------------------------------)

[

## Create a free website using Google Sites

Quick start guide to using Google Sites for building a static site for your school, sports club, business, or personal profile, and hosting it on your own custom domain. — Setting up a basic yet professional looking static website could not be easier with Google Sites. Let’s explore how in this quick introduction, where we will create a free website and host it off our own custom domain. Your site could be up and running inside 5 minutes.



](https://davelms.medium.com/five-minutes-create-a-free-static-website-using-google-sites-58573a927fa2?source=post_page-----394f4c1679cc----0-------------------------------)

[

Website

](https://medium.com/tag/website?source=post_page-----394f4c1679cc---------------website--------------------)

[

6 min read

](https://davelms.medium.com/five-minutes-create-a-free-static-website-using-google-sites-58573a927fa2?source=post_page-----394f4c1679cc----0-------------------------------)

[

![Create a free website in 5 minutes using Google Sites](https://miro.medium.com/fit/c/112/112/1*ARD7P6jAuZRaA8yumt4ahQ.jpeg)

](https://davelms.medium.com/five-minutes-create-a-free-static-website-using-google-sites-58573a927fa2?source=post_page-----394f4c1679cc----0-------------------------------)

---

Share your ideas with millions of readers.

[Write on Medium](https://medium.com/new-story?source=post_page_footer_cta_write----------------------------------------)

---

[

Published in **JavaScript in Plain English**

](https://javascript.plainenglish.io/?source=post_page-----394f4c1679cc----1-------------------------------)

[

·May 2, 2020

](https://davelms.medium.com/solve-a-sudoku-using-javascript-de456e8c34a5?source=post_page-----394f4c1679cc----1-------------------------------)

[

## How to Solve a Sudoku with JavaScript

A walk through of an algorithm to solve popular Sudoku number challenges, with examples written in JavaScript. — Rules of the game A brief summary for those that are not familiar with the rules of Sudoku. A completed Sudoku is a 9 x 9 grid, where each row, column, and each 3 x 3 square must contain each of the numbers 1–9 (and once only). The grid below depicts a single row…



](https://davelms.medium.com/solve-a-sudoku-using-javascript-de456e8c34a5?source=post_page-----394f4c1679cc----1-------------------------------)

[

Java Script

](https://medium.com/tag/javascript?source=post_page-----394f4c1679cc---------------javascript--------------------)

[

9 min read

](https://davelms.medium.com/solve-a-sudoku-using-javascript-de456e8c34a5?source=post_page-----394f4c1679cc----1-------------------------------)

[

![How to Solve a Sudoku with JavaScript](https://miro.medium.com/fit/c/112/112/1*Msv0P6fIngQxn1PN_SVxbg.jpeg)

](https://davelms.medium.com/solve-a-sudoku-using-javascript-de456e8c34a5?source=post_page-----394f4c1679cc----1-------------------------------)

---

[

Apr 26, 2020

](https://davelms.medium.com/use-ansible-to-create-and-configure-ec2-instances-on-aws-cfbb0ed019bf?source=post_page-----394f4c1679cc----2-------------------------------)

[

## Use Ansible to create and configure EC2 instances on AWS

Quick how to tutorial to using Ansible to stand up EC2 instances on AWS. Over the next 5 minutes, we’ll create an initial jumpbox in our default VPC and install some basic developer tools onto it. — Inspiration I’ve been using the awesome A Cloud Guru “Cloud Playground” sandboxes to progress through some of their training content. These environments are fully-functional AWS accounts and allow the user to follow along the Cloud certification tutorials and training — a great new feature of A Cloud Guru.



](https://davelms.medium.com/use-ansible-to-create-and-configure-ec2-instances-on-aws-cfbb0ed019bf?source=post_page-----394f4c1679cc----2-------------------------------)

[

Ansible

](https://medium.com/tag/ansible?source=post_page-----394f4c1679cc---------------ansible--------------------)

[

7 min read

](https://davelms.medium.com/use-ansible-to-create-and-configure-ec2-instances-on-aws-cfbb0ed019bf?source=post_page-----394f4c1679cc----2-------------------------------)

[

![Use Ansible to create and configure EC2 instances on AWS](https://miro.medium.com/fit/c/112/112/1*b9G9WieT210RxxSQrX-LMw.jpeg)

](https://davelms.medium.com/use-ansible-to-create-and-configure-ec2-instances-on-aws-cfbb0ed019bf?source=post_page-----394f4c1679cc----2-------------------------------)

---

[

Apr 19, 2020

](https://davelms.medium.com/convert-an-old-php-application-to-docker-containers-812fa6ae2309?source=post_page-----394f4c1679cc----3-------------------------------)

[

## Convert an old PHP 5 web application to Docker

Take an old PHP 5 web application and convert it to Docker containers, using the latest PHP 7, Composer, Node.js, Grunt, and Bower. — Backstory PHP was always the go-to language for me. I started exploring PHP in 2000, around the time of PHP 4 launch, in an effort to convert an increasingly popular website that was written as lots of individual, static HTML pages. I began work on writing a Content Management System from…



](https://davelms.medium.com/convert-an-old-php-application-to-docker-containers-812fa6ae2309?source=post_page-----394f4c1679cc----3-------------------------------)

[

Docker

](https://medium.com/tag/docker?source=post_page-----394f4c1679cc---------------docker--------------------)

[

8 min read

](https://davelms.medium.com/convert-an-old-php-application-to-docker-containers-812fa6ae2309?source=post_page-----394f4c1679cc----3-------------------------------)

[

![Convert an old PHP application to Docker containers](https://miro.medium.com/fit/c/112/112/1*aE5KJkbslMZO6Zid9JSLiw.png)

](https://davelms.medium.com/convert-an-old-php-application-to-docker-containers-812fa6ae2309?source=post_page-----394f4c1679cc----3-------------------------------)

---

[

Apr 10, 2020

](https://davelms.medium.com/schedule-aws-lambda-to-download-a-file-from-the-internet-and-save-it-to-s3-3cd3f1a43a2b?source=post_page-----394f4c1679cc----4-------------------------------)

[

## Schedule AWS Lambda to download a file from the internet and save it to S3

How to write an AWS Lambda (Node.js) function and schedule it daily to download a file from the internet and save it into an S3 bucket. — This post will explain how to use AWS Lambda to download a file each day and save the file into an S3 bucket. Why did I pick Lambda? The task runs for ~1 second every day and I don’t really need a virtual machine for that — or any infrastructure…



](https://davelms.medium.com/schedule-aws-lambda-to-download-a-file-from-the-internet-and-save-it-to-s3-3cd3f1a43a2b?source=post_page-----394f4c1679cc----4-------------------------------)

[

AWS

](https://medium.com/tag/aws?source=post_page-----394f4c1679cc---------------aws--------------------)

[

11 min read

](https://davelms.medium.com/schedule-aws-lambda-to-download-a-file-from-the-internet-and-save-it-to-s3-3cd3f1a43a2b?source=post_page-----394f4c1679cc----4-------------------------------)

[

![Schedule cron in Lambda to download a file and save it to S3](https://miro.medium.com/fit/c/112/112/1*RhGZIe05Pj_kjGPDbBOmqQ.jpeg)

](https://davelms.medium.com/schedule-aws-lambda-to-download-a-file-from-the-internet-and-save-it-to-s3-3cd3f1a43a2b?source=post_page-----394f4c1679cc----4-------------------------------)

---