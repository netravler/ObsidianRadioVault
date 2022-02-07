[![Kubernetes Advocate](https://miro.medium.com/fit/c/96/96/1*coZMFqqEidVO7sr-59vR2w.jpeg)](https://medium.com/@kubernetes-advocate?source=post_page-----378cbf8171d0-----------------------------------)

[Kubernetes Advocate](https://medium.com/@kubernetes-advocate?source=post_page-----378cbf8171d0-----------------------------------)


# Secrets Management in Kubernetes

![](https://miro.medium.com/max/1300/0*ujEUp_nlNhG8YFbe.jpg)

Secrets

Kubernetes Secrets are secure objects which store sensitive data, such as passwords, OAuth tokens, and SSH keys, etc. with encryption in your clusters.

Using Secrets gives you more flexibility in a Pod Life cycle definition and control over how sensitive data is used. It reduces the risk of exposing the data to unauthorized users.

1.  Secrets are namespaced objects.
2.  Secrets can be mounted as data volumes or environment variables to be used by a container in a pod.
3.  Secret data is stored in tmpfs in nodes
4.  API server stores secrets as plain text in etcd
5.  A per-secret size limit of 1MB

Creating a secret:

Create username.txt and password.txt files.

echo -n 'root' > ./username.txt  
echo -n 'Mq2D#(8gf09' > ./password.txt

And

kubectl create secret generic db-cerds \  
  --from-file=./username.txt \  
  --from-file=./password.txt  
secret "db-cerds" created

List secret:

kubectl get secret/db-cerdsNAME       TYPE      DATA      AGE  
db-cerds   Opaque    2         26s

View secret:

kubectl describe secret/db-cerds  
Name:         db-cerds  
Namespace:    default  
Labels:  
Annotations:Type:  OpaqueData  
====  
password.txt:  11 bytes  
username.txt:  4 bytes

Using YAML file:

The Secret contains two maps: data and string data. The data field is used to store arbitrary data, encoded using base64.

echo -n 'root' | base64  
cm9vdA==echo -n 'Mq2D#(8gf09' | base64  
TXEyRCMoOGdmMDk=

Write a Secret yaml file

---   
apiVersion: v1  
data:   
  password: TXEyRCMoOGdmMDk=  
  username: cm9vdA==  
kind: Secret  
metadata:   
  name: database-creds  
type: Opaque

Create the secret using kubectl create

kubectl create -f creds.yaml  
secret "database-creds" createdkubectl get secret/database-creds  
NAME             TYPE      DATA      AGE  
database-creds   Opaque    2         1m

View secret:

**kubectl get secret/database-creds -o yaml**

---   
apiVersion: v1  
data:   
  password: TXEyRCMoOGdmMDk=  
  username: cm9vdA==  
kind: Secret  
metadata:   
  creationTimestamp: 2019-02-25 06:22:37 +00:00  
  name: database-creds  
  namespace: default  
  resourceVersion: "2657"  
  selfLink: /api/v1/namespaces/default/secrets/database-creds  
  uid: bf0cef90-38c5-11e9-8c95-42010a800068  
type: Opaque

Decoding secret values:

echo -n "cm9vdA==" | base64 --decode  
rootecho -n "TXEyRCMoOGdmMDk=" | base64 --decode  
Mq2D#(8gf09

## Usage of Secrets

A Secret can be used with your workloads in two ways:

1.  specify environment variables that reference the Secret’s values
2.  mount a volume containing the Secret.

**Environment variables:**

---   
apiVersion: v1  
kind: Pod  
metadata:   
  name: php-mysql-app  
spec:   
  containers:   
    -   
      env:   
        -   
          name: MYSQL_USER  
          valueFrom:   
            secretKeyRef:   
              key: username  
              name: database-creds  
        -   
          name: MYSQL_PASSWORD  
          valueFrom:   
            secretKeyRef:   
              key: password  
              name: database-creds  
      image: "php:latest"  
      name: php-app

Secret as Volume:

---   
apiVersion: v1  
kind: Pod  
metadata:   
  name: redis-pod  
spec:   
  containers:   
    -   
      image: redis  
      name: redis-pod  
      volumeMounts:   
        -   
          mountPath: /etc/dbcreds  
          name: dbcreds  
          readOnly: true  
  volumes:   
    -   
      name: dbcreds  
      secret:   
        secretName: database-creds

Additional Info :

Secret creation syntax

kubectl create secret [TYPE] [NAME] [DATA]

TYPE can be one of the following:

**generic:** Create a Secret from a local file, directory, or literal value.

**docker-registry:** Creates a dockercfg Secret for use with a Docker registry. Used to authenticate against Docker registries.

**tls:** Create a TLS secret from the given public/private key pair. The public/private key pair must exist beforehand. The public key certificate must be.PEM encoded and match the given private key.

DATA can be one of the following:

— from-file

kubectl create secret generic credentials \  
  --from-file=username=./username.txt \  
  --from-file=password=./password.txt  
--from-env-file

— from-env-file

cat credentials.txt  
username=admin  
password=Ex67Hn*9#(jwkubectl create secret generic credentials \  
--from-env-file ./credentials.txt

— from-literal flags

kubectl create secret generic literal-token \  
--from-literal user=admin \  
--from-literal password="Ex67Hn*9#(jw"

Thanks

## Questions /Answers

## How to make the Kubernetes pods unable to decrypt the Kubernetes secrets without a key?

I am trying to making this as step by step solution.

Encrypt your data using your-key (your encryption-logic, probably, in a script).

./encrypt.sh --key your-key --data your-data

Create a secret of this encrypted data

kubectl create secret generic your-secret-name --from-literal=secretdata=your-encrypted-data

You could add decryption logic like this in your pod ( either as a sidecar or init container)

# decrypt.sh will decode base64 then your decryption logic using your-key  
./decrypt.sh --key your-key --data /var/my-secrets

Also, you need to mount this secret as volume to your container.

spec:  
      containers:  
      - image: "image"  
        name: app  
        ...  
        volumeMounts:  
          - mountPath: "/var/my-secrets"  
            name: my-secret  
      volumes:  
        - name: my-secret  
          secret:  
            secretName: your-secret-name  

## Refreshing PODs automatically when mounted secrets get updated, but it looks not happening

Kubernetes does not support this feature at the moment and there is a feature in the works ([https://github.com/kubernetes/kubernetes/issues/22368](https://github.com/kubernetes/kubernetes/issues/22368)).

You can use custom solution available to achieve the same and one of the popular ones include `[Reloader](https://github.com/stakater/Reloader)`.

The doc you linked describes that the secret values inside the mounted volume will get updated when you update the Kubernetes `Secret` object.

The application running within the pod can get to re-read the key once it’s updated tho’ and a brand new pod isn’t created to change the key itself.

Also, note that there can be some delay between the actual update of the secret and getting those values reflected in the volume.

> As a result, the overall delay from the instant, once the key is updated to the instant once new keys are projected to the Pod, will be as long _as the kubelet sync period + cache propagation delay, where the cache propagation delay depends on the chosen cache type (it equals to watch propagation delay, TTL of cache, or zero correspondingly)_

## Kubernetes deployment mounts secret as a folder instead of a file

`Secrets` let you store and manage sensitive information (e.g. passwords, private keys) and `ConfigMaps` are used for non-sensitive configuration data. As you’ll see within the Secrets associated ConfigMaps documentation:

A Secret is an object that contains a low quantity of sensitive information like a countersign, a token, or a key

> _A ConfigMap allows you to decouple environment-specific configuration from your container images, so that your applications are easily portable._

**Mounting Secret as a file**

It is possible to create `Secret` and pass it as a **file** or multiple **files** to `Pods`.  
I've created a simple example for you to illustrate how it works. Below you can see a sample `Secret` manifest file and `Deployment` that uses this Secret:  
**NOTE:** I used `subPath` with `Secrets` and it works as expected.

---  
apiVersion: v1  
kind: Secret  
metadata:  
  name: my-secret  
data:  
  secret.file1: |  
    c2VjcmV0RmlsZTEK  
  secret.file2: |  
    c2VjcmV0RmlsZTIK  
---  
apiVersion: apps/v1  
kind: Deployment  
metadata:  
...  
    spec:  
      containers:  
      - image: nginx  
        name: nginx  
        volumeMounts:  
        - name: secrets-files  
          mountPath: "/mnt/secret.file1"  # "secret.file1" file will be created in "/mnt" directory  
          subPath: secret.file1  
        - name: secrets-files  
          mountPath: "/mnt/secret.file2"  # "secret.file2" file will be created in "/mnt" directory  
          subPath: secret.file2  
      volumes:  
        - name: secrets-files  
          secret:  
            secretName: my-secret # name of the Secret

**Note:** `Secret` should be created before `Deployment`.

After creating `Secret` and `Deployment`, we can see how it works:

$ kubectl get secret,deploy,pod  
NAME                         TYPE                                  DATA   AGE  
secret/my-secret             Opaque                                2      76sNAME                    READY   UP-TO-DATE   AVAILABLE   AGE  
deployment.apps/nginx   1/1     1            1           76sNAME                         READY   STATUS    RESTARTS   AGE  
pod/nginx-7c67965687-ph7b8   1/1     Running   0          76s$ kubectl exec nginx-7c67965687-ph7b8 -- ls /mnt  
secret.file1  
secret.file2  
$ kubectl exec nginx-7c67965687-ph7b8 -- cat /mnt/secret.file1  
secretFile1  
$ kubectl exec nginx-7c67965687-ph7b8 -- cat /mnt/secret.file2  
secretFile2

**Projected Volume**

I think a better way to achieve your goal is to use [projected volume](https://kubernetes.io/docs/concepts/storage/volumes/#projected).

A projected volume maps many existing volume sources into an equivalent directory.

Within the Projected Volume documentation you’ll be able to notice elaborated explanations however in addition, I created associate degree example that may assist you to perceive however it works. Using projected volume I mounted secret.file1, secret.file2 from Secret and config.file1 from ConfigMap as files into the Pod

---  
apiVersion: v1  
kind: Secret  
metadata:  
  name: my-secret  
data:  
  secret.file1: |  
    c2VjcmV0RmlsZTEK  
  secret.file2: |  
    c2VjcmV0RmlsZTIK  
---  
apiVersion: v1  
kind: ConfigMap  
metadata:  
  name: my-config  
data:  
  config.file1: |  
    configFile1    
---  
apiVersion: v1  
kind: Pod  
metadata:  
  name: nginx  
spec:  
  containers:  
  - name: nginx  
    image: nginx  
    volumeMounts:  
    - name: all-in-one  
      mountPath: "/config-volume"  
      readOnly: true  
  volumes:  
  - name: all-in-one  
    projected:  
      sources:  
      - secret:  
          name: my-secret  
          items:  
            - key: secret.file1  
              path: secret-dir1/secret.file1  
            - key: secret.file2  
              path: secret-dir2/secret.file2  
      - configMap:  
          name: my-config  
          items:  
            - key: config.file1  
              path: config-dir1/config.file1

We can check how it works:

$ kubectl exec nginx -- ls /config-volume  
config-dir1  
secret-dir1  
secret-dir2      
$ kubectl exec nginx -- cat /config-volume/config-dir1/config.file1  
configFile1  
$ kubectl exec nginx -- cat /config-volume/secret-dir1/secret.file1  
secretFile1  
$ kubectl exec nginx -- cat /config-volume/secret-dir2/secret.file2  
secretFile2

If this response doesn’t answer your question, please provide more details about your `Secret` and what exactly you want to achieve.

For More, Recommendations

[https://kubernetes.io/docs/concepts/configuration/secret/](https://kubernetes.io/docs/concepts/configuration/secret/)

[https://platform9.com/blog/kubernetes-secrets-management/](https://platform9.com/blog/kubernetes-secrets-management/)

[https://blogs.oracle.com/developers/5-best-practices-for-kubernetes-security](https://blogs.oracle.com/developers/5-best-practices-for-kubernetes-security)

[https://dev.to/martinpham/secure-your-kubernetes-application-with-https-ng7](https://dev.to/martinpham/secure-your-kubernetes-application-with-https-ng7)

![](https://miro.medium.com/max/1400/0*cRMUa8dBX1dHpj5L)

👋 [**Join us today**](https://avmconsulting.net/) **!!**

️Follow us on [LinkedIn](https://www.linkedin.com/company/avm-consulting-inc/), [Twitter](https://twitter.com/AvmConsulting), [Facebook](https://www.facebook.com/AVM-Consulting-Inc-114225960307418), and [Instagram](https://www.instagram.com/avmconsulting_inc/)

![](https://miro.medium.com/max/1400/1*UOzO1SzC0rQg-0hBwZjFDA.png)

[https://avmconsulting.net/](https://avmconsulting.net/)

If this post was helpful, please click the clap 👏 button below a few times to show your support! ⬇

325

1

## Sign up for AVM Consulting

### By AVM Consulting Blog

We are developing blogging to help community with Cloud/DevOps services [Take a look.](https://medium.com/avmconsulting-blog/newsletters/avm-consulting?source=newsletter_v3_promo--------------------------newsletter_v3_promo--------------)

Get this newsletter

Emails will be sent to pheintz2@gmail.com.[Not you?](https://medium.com/m/signin?operation=login&redirect=https%3A%2F%2Fmedium.com%2Favmconsulting-blog%2Fsecrets-management-in-kubernetes-378cbf8171d0&collection=AVM+Consulting+Blog&collectionId=e5f243f939e8&newsletterV3=AVM+Consulting+&newsletterV3Id=49263c655edb&source=newsletter_v3_promo--------------------------newsletter_v3_promo----------49263c655edb----)

## [More from AVM Consulting Blog](https://medium.com/avmconsulting-blog?source=post_page-----378cbf8171d0-----------------------------------)

Follow

AVM Consulting — Clear strategy for your cloud

![Kubernetes Advocate](https://miro.medium.com/fit/c/24/24/1*coZMFqqEidVO7sr-59vR2w.jpeg)

[

Kubernetes Advocate

](https://medium.com/@kubernetes-advocate?source=post_page-----378cbf8171d0----0-------------------------------)

[

·Feb 15, 2021

](https://medium.com/avmconsulting-blog/how-to-secure-applications-on-kubernetes-ssl-tls-certificates-8f7f5751d788?source=post_page-----378cbf8171d0----0-------------------------------)

[

## How to secure applications running on Kubernetes (SSL/TLS Certificates)?

Securing an application running on Kubernetes (SSL/TLS Certificates) You can secure an application running on Kubernetes by creating a secret that contains a TLS (Transport Layer Security) private key and certificate. Currently, Ingress supports a single TLS port, 443, and assumes TLS termination. The TLS secret must contain keys named tls. crt and tls. …



](https://medium.com/avmconsulting-blog/how-to-secure-applications-on-kubernetes-ssl-tls-certificates-8f7f5751d788?source=post_page-----378cbf8171d0----0-------------------------------)

[

Startup

](https://medium.com/tag/startup?source=post_page-----378cbf8171d0---------------startup--------------------)

[

5 min read

](https://medium.com/avmconsulting-blog/how-to-secure-applications-on-kubernetes-ssl-tls-certificates-8f7f5751d788?source=post_page-----378cbf8171d0----0-------------------------------)

[

![How to secure applications  on Kubernetes (SSL/TLS Certificates)?](https://miro.medium.com/fit/c/112/112/0*EzskUqcWVhPNvqeS.png)

](https://medium.com/avmconsulting-blog/how-to-secure-applications-on-kubernetes-ssl-tls-certificates-8f7f5751d788?source=post_page-----378cbf8171d0----0-------------------------------)

---

Share your ideas with millions of readers.

[Write on Medium](https://medium.com/new-story?source=post_page_footer_cta_write----------------------------------------)

---

![Ross Rhodes](https://miro.medium.com/fit/c/24/24/1*ZaZGOODlWKslR7QQMJaHJg.jpeg)

[

Ross Rhodes

](https://medium.com/@trrhodes?source=post_page-----378cbf8171d0----1-------------------------------)

[

·Feb 11, 2021

](https://medium.com/avmconsulting-blog/maintaining-aws-serverless-platforms-94191fd62f9f?source=post_page-----378cbf8171d0----1-------------------------------)

[

## Maintaining AWS Serverless Platforms

Lessons learned maintaining serverless software in production — With over two years experience developing software on serverless cloud platforms — much of that time spent continuously deploying changes to production — now seems as good a time as any to share some tips and tricks when it comes to maintaining serverless software in production. Working exclusively on Amazon…



](https://medium.com/avmconsulting-blog/maintaining-aws-serverless-platforms-94191fd62f9f?source=post_page-----378cbf8171d0----1-------------------------------)

[

Amazon Web Services

](https://medium.com/tag/amazon-web-services?source=post_page-----378cbf8171d0---------------amazon_web_services--------------------)

[

4 min read

](https://medium.com/avmconsulting-blog/maintaining-aws-serverless-platforms-94191fd62f9f?source=post_page-----378cbf8171d0----1-------------------------------)

[

![Maintaining AWS Serverless Platforms](https://miro.medium.com/fit/c/112/112/1*pCCONHQKY3W2pLUTvnX8FQ.png)

](https://medium.com/avmconsulting-blog/maintaining-aws-serverless-platforms-94191fd62f9f?source=post_page-----378cbf8171d0----1-------------------------------)

---

![Vinayak Pandey](https://miro.medium.com/fit/c/24/24/1*2jV3njNC1CdAUQhIS7lOpg.jpeg)

[

Vinayak Pandey

](https://medium.com/@vinayakpandey-7997?source=post_page-----378cbf8171d0----2-------------------------------)

[

·Feb 10, 2021

](https://medium.com/avmconsulting-blog/playing-with-kms-key-policies-c2c55d5f2d0?source=post_page-----378cbf8171d0----2-------------------------------)

[

## Playing With KMS Key Policies

In this post, we’ll play with some KMS key policies which allow IAM users to access CMKs. Recently I got a Not Authorized message while accessing a CMK, even as an Administrator and that’s when I figured out these intrigued policies. Scenario1: Login as root user and create a CMK…



](https://medium.com/avmconsulting-blog/playing-with-kms-key-policies-c2c55d5f2d0?source=post_page-----378cbf8171d0----2-------------------------------)

[

AWS

](https://medium.com/tag/aws?source=post_page-----378cbf8171d0---------------aws--------------------)

[

2 min read

](https://medium.com/avmconsulting-blog/playing-with-kms-key-policies-c2c55d5f2d0?source=post_page-----378cbf8171d0----2-------------------------------)

[

![Playing With KMS Key Policies](https://miro.medium.com/fit/c/112/112/0*Sy3J0iGoHiOHLysi.png)

](https://medium.com/avmconsulting-blog/playing-with-kms-key-policies-c2c55d5f2d0?source=post_page-----378cbf8171d0----2-------------------------------)

---

![Kubernetes Advocate](https://miro.medium.com/fit/c/24/24/1*coZMFqqEidVO7sr-59vR2w.jpeg)

[

Kubernetes Advocate

](https://medium.com/@kubernetes-advocate?source=post_page-----378cbf8171d0----3-------------------------------)

[

·Jan 29, 2021

](https://medium.com/avmconsulting-blog/common-network-troubleshooting-step-by-step-9cd6186048a7?source=post_page-----378cbf8171d0----3-------------------------------)

[

## Common Network Troubleshooting Step by Step!

This article describes common networking tools that can help you identify network connectivity issues for your server or website. Some of these tools are installed on your system by default and some require installation. Common tools installed by default for Windows, Mac, and Linux The following tools are installed on all Windows, Mac, and Linux operating systems and servers by…



](https://medium.com/avmconsulting-blog/common-network-troubleshooting-step-by-step-9cd6186048a7?source=post_page-----378cbf8171d0----3-------------------------------)

[

AWS

](https://medium.com/tag/aws?source=post_page-----378cbf8171d0---------------aws--------------------)

[

8 min read

](https://medium.com/avmconsulting-blog/common-network-troubleshooting-step-by-step-9cd6186048a7?source=post_page-----378cbf8171d0----3-------------------------------)

[

![Common Network Troubleshooting Step by Step!](https://miro.medium.com/fit/c/112/112/0*uugTU72B-hJ9egfr.png)

](https://medium.com/avmconsulting-blog/common-network-troubleshooting-step-by-step-9cd6186048a7?source=post_page-----378cbf8171d0----3-------------------------------)

---

![Kubernetes Advocate](https://miro.medium.com/fit/c/24/24/1*coZMFqqEidVO7sr-59vR2w.jpeg)

[

Kubernetes Advocate

](https://medium.com/@kubernetes-advocate?source=post_page-----378cbf8171d0----4-------------------------------)

[

·Jan 29, 2021

](https://medium.com/avmconsulting-blog/troubleshooting-an-offline-website-step-by-step-in-cloud-7e521885c49e?source=post_page-----378cbf8171d0----4-------------------------------)

[

## Troubleshooting an offline website step by step in Cloud?

More than half of all deployment issues involve an offline website. The causes of a website being offline are often easy to fix. This article describes how to troubleshoot an offline website. Note: This article assumes knowledge of networking tools such as ping and nmap and traceroute. …



](https://medium.com/avmconsulting-blog/troubleshooting-an-offline-website-step-by-step-in-cloud-7e521885c49e?source=post_page-----378cbf8171d0----4-------------------------------)

[

AWS

](https://medium.com/tag/aws?source=post_page-----378cbf8171d0---------------aws--------------------)

[

8 min read

](https://medium.com/avmconsulting-blog/troubleshooting-an-offline-website-step-by-step-in-cloud-7e521885c49e?source=post_page-----378cbf8171d0----4-------------------------------)

[  
](https://medium.com/avmconsulting-blog/troubleshooting-an-offline-website-step-by-step-in-cloud-7e521885c49e?source=post_page-----378cbf8171d0----4-------------------------------)