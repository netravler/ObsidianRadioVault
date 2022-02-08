# Build and configuration AKS on Azure using Terraform

-   Published on September 9, 2020

![](https://media-exp1.licdn.com/dms/image/C4D12AQGNrvQMpLAb-w/article-cover_image-shrink_720_1280/0/1599702057715?e=1649894400&v=beta&t=IAi24CcrrJxAtWzd9fZK7ldZOn4i5vahxJYywb_rL-o)

[

![Guilherme Sesterheim](https://media-exp1.licdn.com/dms/image/C4D03AQFJmGKPIYjHDw/profile-displayphoto-shrink_100_100/0/1623343256603?e=1649894400&v=beta&t=YXHpuj1Z0OJEwQ3yu2WImQFnw4oeLgLPb14AjWgILKQ)



](https://www.linkedin.com/in/tetris/gsesterheim/)

[

## Guilherme Sesterheim

](https://www.linkedin.com/in/tetris/gsesterheim/)

SAP DevOps SRE Engineer at Amazon Web Services (AWS)

[28 articles](https://www.linkedin.com/in/gsesterheim/recent-activity/posts/) Follow

**TL;DR:** 3 resources will be added to your Azure account. 1 – Configure Terraform to save state lock files on Azure Blob Storage. 2 – Use Terraform to create and keep track of your AKS. 3 – How to configure kubectl locally to set up your Kubernetes.

This article follows best practices and benefits of infrastructure automation described [here](http://sesterheim.com.br/app-scaling-with-operational-excellence/). **Infrastructure as code**, **immutable infrastructure**, **more speed**, **reliability**, **auditing** and **documentation** are the concepts you will be helped to achieve after following this article.

**You can find all the source code for this project on this GitHub repo:** [**https://github.com/guisesterheim/TerraformAKS**](https://github.com/guisesterheim/TerraformAKS)

## Creating a user for your Azure account

Terraform has a good how to for you to authenticate. In [this](https://www.terraform.io/docs/providers/azurerm/guides/service_principal_client_secret.html) link you’ll find how to retrieve the following needed authentication data:

subscription_id, tenant_id, client_id, and client_secret.

To find the remaining container_name, storage_account_name, key and resource_group_name, create your own Blob Storage container in Azure. And use the names as the suggestion below:

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQGYdSn_1je1dQ/article-inline_image-shrink_1000_1488/0/1599702083928?e=1649894400&v=beta&t=wUtKIiQZZS7if7niNsAl8XiFu_ohQcbukmpvQKCFKuA)

-   The top red mark is your storage_account_name
-   In the middle you have your container_name
-   The last one you have your key (file name)

## Starting Terraform locally

To keep track of your Infrastructure with Terraform, you will have to let Terraform store your tfstate file in a safe place. The command below will start Terraform and store your tfstate in Azure Blob Storage. So **navigate to folder tf_infrastructure** and use the following command to start your Terraform repo:

terraform init \
    -backend-config "container_name=<your folder inside Azure Blob Storage>" \
    -backend-config "storage_account_name=<your Azure Storage Name>" \
    -backend-config "key=<file name to be stored>" \
    -backend-config "subscription_id=<subscription ID of your account>" \
    -backend-config "client_id=<your username>" \
    -backend-config "client_secret=<your password>" \
    -backend-config "tenant_id=<tenant id>" \
    -backend-config "resource_group_name=<resource group name to find your Blob Storage>"

Should everything goes well you should a screen similar to the one below and we are ready to plan our infrastructure deployment!

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQHeq8k9L52-2Q/article-inline_image-shrink_1000_1488/0/1599702099959?e=1649894400&v=beta&t=XLSQmtzjhV3dSdvMnDguHU3Ip0BJJ-nVIzfNqYAO8o0)

## Planning your deploy – Terraform plan

The next step is to plan your deploy. Use the following command so Terraform can prepare to deploy your resources:

terraform plan \
    -var 'client_id=<client_id>' \
    -var 'client_secret=<secret_id>' \
    -var 'subscription_id=<subscription_id>' \
    -var 'tenant_id=<tenant_id>' \
    -var 'timestamp=<timestamp>' \
    -var 'acr_reader_user_client_id=<User client ID to read ACR>' \
    -var 'acr_reader_user_secret_key=<User secret to read ACR>' \
    -var-file="<your additional vars file name. Suggestion: rootVars-dev.tfvars>" \
    -out tfout.log

Some of the information above are the some as we used in Terraform init. So go ahead and copy them. The rest of them are:

-   **TIMESTAMP** – this is the timestamp of when you are running this terraform plan. It is intended to help with the blue/green deployment strategy. The timestamp is a simple string that will be added to the end of your resource group name. The resource group name will have the following format: “fixedRadical-environment-timestamp”. You can check how it’s built on file tf_infrastructure/modules/common/variables.tf
-   **ACR_READER_USER_CLIENT_ID** – This is the client_id used by your Kubernetes to go and read the ACR (Azure Container Registry) to retrieve your docker images for deployment. You should use a new one with fewer privileges than the main client_id we’re using.
-   **ACR_READER_USER_SECRET_KEY** – This is the client secret (password) of the above client_id.
-   **-VAR-FILE** – Terraform allows us to add variables in a file instead of on the command line like we’ve been using. Do not store sensitive information inside this file. You have an example on tf_infrastructure/rootVars-dev.tfvars file
-   **TFOUT.LOG** – This is the name of the file to which Terraform will store the plan to achieve your Terraform configuration

Should everything goes well you’ll have a screen close to the one below and we’ll be ready to finally create your AKS!

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQH2HZKkflLqHw/article-inline_image-shrink_1500_2232/0/1599702134533?e=1649894400&v=beta&t=tzJsbik7nMQngoOmxsiINUddgFSneJAgKWR4YyUNObo)

Take a look at the “node_labels” tag on AKS and also on the additional node pool. We will use this in the Kubernetes config file below to tell Kubernetes in which node pool to deploy our Pods.

## Deploying the infrastructure – Terraform apply

All the hard work is done. Just run the command below and wait for about 10 minutes and your AKS will be running

terraform apply tfout.log

Once the deployment is done you should see a screen like this:

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQFUoKpXBNbXbQ/article-inline_image-shrink_1000_1488/0/1599702146568?e=1649894400&v=beta&t=0GW0JkP9G9Sl2tWhd7wcKTgb8alr-t1Cn5uDRTYzNtA)

## Configuring kubectl to work connected to AKS

Azure CLI does the heavy lifting on this part. So run the command below to make your Kubectl command-line tool to easily point to the newly deployed AKS:

az aks get-credentials --name $(terraform output aks_name) --resource-group $(terraform output resource_group_name)

If you don’t have the Azure CLI configured yet, follow the [instructions here](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## Applying our configuration to Kubernetes

Now navigate back on your terminal to the folder kubernetes_deployment. Let’s apply the commands and then run through the files to understand what’s going on:

1. PROFILE=dev
2. kubectl apply -f k8s_deployment-dev.yaml
3. kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.34.1/deploy/static/provider/cloud/deploy.yaml

### PROFILE=DEV

**PROFILE=dev** – it is setting an environment variable on your terminal to be read by kubectl and applied to the docker containers. I used a spring application, so you can see it being used on k8s_deployment-dev.yaml here:

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQH7kneTgfFwAw/article-inline_image-shrink_1000_1488/0/1599702174503?e=1649894400&v=beta&t=FB5qId5DorGnVQomYV3_YuwsAAOKVX4HPw_z1vB2Koc)

1.  **Kubernetes will grab our PROFILE=dev** environment variable and pass on to Spring Boot.
2.  **The path** where Kubernetes will pull our images from using ACR credentials.
3.  **Liveness probe** teaches Kubernetes how to understand if that container is running or not.
4.  **NodeSelector** tells Kubernetes in which node pool (using the node_labels we highlighted above) where the Pods should be run.

### Configure K8S

kubectl apply -f k8s_deployment-dev.yaml

Kubernetes allows us to store all our configuration in a single file. This is the file. You will see two deployments (pods instructions): company and customer. Also, you will see one service that exposes each of them: company-service and customer-service.

-   The **services** (example below) use the ClusterIP strategy. It will tell Kubernetes to create an internal Load Balancer to balance requests to your pods. The port tells which port receives requests and the targetPort tells which port in the service will handle requests. More info [here](https://kubernetes.io/docs/concepts/services-networking/service/).

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQGkjT1p7UVgBQ/article-inline_image-shrink_1000_1488/0/1599702236215?e=1649894400&v=beta&t=Avu7BUAuSBPS03-X_fu0yX3aJzRvxUwWs8mGrVf4ZF0)

-   **Ingress strategy** is the most important part:

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQEuI48l59iPGw/article-inline_image-shrink_1000_1488/0/1599702187277?e=1649894400&v=beta&t=GDXSD1jhZgewg4CQ02tygELw5ATOF6rV_fkKWWow0VM)

1.  **nginx** is the class for your ingress strategy. It uses nginx implementation to load balance requests internally.
2.  **/$1$2$3** is what Kubernetes should forward as the request URL to our pods. $1 means (api/company) highlighted in item 5. $2 means (/|$) and $3 means (.*)
3.  **/$1/swagger-ui.html** this is the default app root for our Pods
4.  **Redirect from www** – true – self-explanatory
5.  **Path** is the URL structure to pass on as variables to item 2

-   To add **TLS** yo our Kubernetes you have to generate your certificate and past key and crt on the highlighted areas below on base64 format. An example on Linux is like first image below. When adding the info to the file remember to past it as a single row without spaces, line breaks or others. Second image shows where to put the crt and key respectivelly.

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQFvuI1Oacle1g/article-inline_image-shrink_1000_1488/0/1599702292860?e=1649894400&v=beta&t=qdERSU7lKpdwNmYnkGjgiRDha3jc52CLMJdtZHM4IKE)

### **Apply nginx Load Balancer**

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.34.1/deploy/static/provider/cloud/deploy.yaml 

This will apply nginx to handle our ingress instrategy.

## Testing our Kubernetes deployment

After all this configuration run the command below to wait for Kubernetes to assign an IP to our ingress strategy:

kubectl get ingress --watch

You will get an output like this:

![No alt text provided for this image](https://media-exp1.licdn.com/dms/image/C4D12AQEnV21M7RlGCQ/article-inline_image-shrink_1000_1488/0/1599702308578?e=1649894400&v=beta&t=hXlVclxWCIgW5PlnMGNtmTu7NqRpBtxz0hqYHhNNmXI)

Once you have the IP, you can paste it to Chrome, add the path to your specific service and you will get your application output.

Report this

### Published by

[

![Guilherme Sesterheim](https://media-exp1.licdn.com/dms/image/C4D03AQFJmGKPIYjHDw/profile-displayphoto-shrink_100_100/0/1623343256603?e=1649894400&v=beta&t=YXHpuj1Z0OJEwQ3yu2WImQFnw4oeLgLPb14AjWgILKQ)





](https://www.linkedin.com/in/tetris/gsesterheim/)

[Guilherme Sesterheim](https://www.linkedin.com/in/tetris/gsesterheim/)

SAP DevOps SRE Engineer at Amazon Web Services (AWS)

Published • 1y

[28 articles](https://www.linkedin.com/in/gsesterheim/recent-activity/posts/)Follow

An example of using Terraform to spin up AKS and the configuring it using kubectl