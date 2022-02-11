[

![Kristijan Mitevski](https://learnk8s.io/a/d7ed57767a646eaa3d38d2c540389fc5.jpg)

Kristijan Mitevski](https://www.linkedin.com/in/kristijan-mitevski/ "Kristijan Mitevski")

# Getting started with Terraform and Kubernetes on Azure AKS

 UPDATED IN MARCH 2021

---

![Getting started with Terraform and Kubernetes on Azure AKS](https://learnk8s.io/a/1c54b14d7eb99b184eac1837c1cf54f3.svg)

---

**TL;DR:** _In this article, you will learn how to create Kubernetes clusters on [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/services/kubernetes-service/) with the Azure CLI and Terraform. By the end of the tutorial, you will automate creating two clusters (dev and prod) complete with an Ingress controller in a single command._

Azure offers a managed Kubernetes service where you can request a cluster, connect to it, and use it to deploy applications.

Azure Kubernetes Service (AKS) is a managed Kubernetes service, which means that the Azure platform is fully responsible for managing the cluster control plane.

In particular, AKS:

-   Manages Kubernetes API servers and the etcd database.
-   Runs the Kubernetes control-plane single or in multiple availability zones.
-   Scales the control-plane as you add more nodes to your cluster.
-   Provides a mechanism to upgrade your control plane and nodes to a newer - version.
-   Rotates certificates and keys.

However, when you use AKS, you outsource managing the control plane to Azure at no cost.

Yes, that is correct.

On Azure running the AKS incurs no cost for the control plane — you only pay for what you use by the worker nodes.

However, if you want a guaranteed Service Level Agreements of 99.95% uptime or higher, the additional cost is [USD0.10 per hour per cluster.](https://azure.microsoft.com/en-us/pricing/details/kubernetes-service/)

Microsoft Azure has 12-month [free tier promotion](https://azure.microsoft.com/en-us/free/) for its popular services as well as free USD200 credit to freely spend on any service in the next 30 days after your registration.

If you use the free tier offer, you will not incur any additional charges when following this tutorial.

The rest of the guide assumes that you have an account on Microsoft Azure.

If you don't, [you can sign up here.](https://azure.microsoft.com/en-us/free/)

Lastly, if you prefer to look at the code, [you can do so here](https://github.com/k-mitevski/terraform-aks).

## Table of contents

-   [Three popular options to provision an AKS cluster](https://learnk8s.io/terraform-aks#three-popular-options-to-provision-an-aks-cluster)
-   [Setting up the Azure account](https://learnk8s.io/terraform-aks#setting-up-the-azure-account)
-   [Azure CLI: the quickest way to provision an AKS cluster](https://learnk8s.io/terraform-aks#azure-cli-the-quickest-way-to-provision-an-aks-cluster)
-   [Provisioning an AKS cluster with Terraform](https://learnk8s.io/terraform-aks#provisioning-an-aks-cluster-with-terraform)
-   [Creating a resource group with Terraform](https://learnk8s.io/terraform-aks#creating-a-resource-group-with-terraform)
-   [Terraform step by step](https://learnk8s.io/terraform-aks#terraform-step-by-step)
-   [Testing the cluster by deploying a simple Hello World app](https://learnk8s.io/terraform-aks#testing-the-cluster-by-deploying-a-simple-hello-world-app)
-   [Routing traffic into the cluster with an Ingress](https://learnk8s.io/terraform-aks#routing-traffic-into-the-cluster-with-an-ingress)
-   [Fully automated Dev & Production environments with Terraform modules](https://learnk8s.io/terraform-aks#fully-automated-dev-production-environments-with-terraform-modules)

## Three popular options to provision an AKS cluster

There are three popular options to run and deploy an AKS cluster:

1.  You can create a cluster from the [AKS web interface.](https://portal.azure.com/#create/microsoft.aks)
2.  You can use the [Azure command-line utility.](https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough?toc=%2fcli%2fazure%2ftoc.json)
3.  You can define the cluster using code with a tool such as [Terraform.](https://www.terraform.io/)

Even if it is listed as the first option, **creating a cluster through the Azure portal is discouraged.**

There are plenty of configuration options and screens that you have to complete before using the cluster.

When you create the cluster manually, can you be sure that:

-   _You did not forget to change one of the parameters?_
-   _You can repeat precisely the same steps while creating a cluster for other environments?_
-   _When there is a change, you can apply the same modifications in sequence to all clusters without any mistake?_

The process is error-prone and doesn't scale well if you have more than a single cluster.

**A better option is defining a file containing all the configuration flags and using it as a blueprint to create the cluster.**

And that's precisely what you can do with the Azure CLI and infrastructure as code tools such as Terraform.

## Setting up the Azure account

Before you start creating clusters and utilizing Terraform, you have to install the Azure CLI.

You can find the official documentation on installing the [Azure CLI here.](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)

After you install the Azure CLI, you should run:

bash

```
az version
{
  "azure-cli": "2.18.0",
  "azure-cli-core": "2.18.0",
  "azure-cli-telemetry": "1.0.6",
  "extensions": {}
}
```

If you can see the above output, that means the installation is successful.

Next, you need to link your account to the Azure CLI, and you can do this with:

bash

```
az login
```

This will open a login page where you can authenticate with your credentials.

Once completed, you should see the "You have logged in. Now let us find all the subscriptions to which you have access…" and your subscription output in JSON format.

Now before continuing, you can find all of the [available regions that AKS supports here.](https://docs.microsoft.com/en-us/azure/aks/availability-zones#limitations-and-region-availability)

You can now try listing all your AKS clusters with:

bash

```
az aks list
[]
```

_An empty list._

That makes sense since you haven't created any clusters yet.

## Azure CLI: the quickest way to provision an AKS cluster

Azure provides a featured all-in-one CLI tool, meaning you won't need to install any additional software or tools to manage and create clusters.

Let's explore the tool with:

bash

```
az aks create --help

Command
    az aks create : Create a new managed Kubernetes cluster.

Arguments
    --name -n                      [Required] : Name of the managed cluster.
    --resource-group -g            [Required] : Name of resource group. You can configure the

# [output truncated]
```

Notice the required arguments for creating a cluster: the name and resource group.

Scrolling down further in the output, you will see the sheer number of examples provided with the `--help` argument.

bash

```
Create a Kubernetes cluster with a specific version.
        az aks create -g MyResourceGroup -n MyManagedCluster --kubernetes-version 1.16.9

    Create a Kubernetes cluster with a larger node pool.
        az aks create -g MyResourceGroup -n MyManagedCluster --node-count 7
```

You've noticed that apart from the cluster requiring a name, you will also need to provide a resource group in the arguments.

_What are resource groups, and why do I need them?_

Not to worry, I will explain this in the next section.

### Resource Groups in Azure

[Resource Group(s) in Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-cli) are containers that logically hold multiple resources.

You can use Resource Groups to bundle all the resources such as Load Balancers, NICs, Subnets, etc., in the same group — giving you a more accessible option to manage everything in separate environments.

You can list all the resource groups with:

bash

```
az group list
```

But since you haven't created any, you will get an empty response.

Let's fix that and create a resource group where the cluster will be made.

A Resource Group will need a name and location where to be created:

bash

```
az group create --name learnk8sResourceGroup --location northeurope
```

**Note:** To easily list all the available locations in a table format, you can do so with:

bash

```
az account list-locations -o table

DisplayName               Name         RegionalDisplayName
------------------------  -----------  -------------------
East US                   eastus       (US) East US
East US 2                 eastus2      (US) East US 2
[output truncated]
```

After issuing the `az group create` command, you should now see in the output **"provisioningState": "Succeeded"**.

Entering the `az group list` command will provide you with the same output.

**Before moving forward, you will need to register a [resource provider](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/resource-providers-and-types). Otherwise, if you try and create the cluster without first defining it, the command will fail.**

bash

```
az provider register -n Microsoft.ContainerService
```

You can now create the AKS cluster with:

bash

```
az aks create -g learnk8sResourceGroup -n learnk8s-cluster --generate-ssh-keys --node-count 2
```

Let's have a look at the flags:

1.  The `--generate--ssh--keys` argument is required if you are not supplying your SSH keys.
2.  The `--node-count` is required to stay under the [quota limit](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits). If needed, you can send a request to Azure for increasing the limits.

Be patient; the cluster can take up to 15 minutes to be created.

> While you are waiting for the cluster to be provisioned, you should go ahead and download kubectl — the command-line tool to connect and manage the Kubernetes cluster.

[Kubectl can be downloaded from here.](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

You can check that the binary is installed successfully with:

bash

```
kubectl version --client
Client Version: version.Info{Major:"1", Minor:"20", GitVersion:"v1.20.2"
```

Once the cluster is created, you will get a JSON output with its specs.

The cluster will be created with the following values:

-   Location: **North Europe**.
-   Node count: **2 nodes**.
-   Autoscaling: **off**.
-   Instance type: **Standard_DS2_v2 (CPU: 2; RAM: 7GB)**.
-   Disk per node: **128GB**.

[You can always choose different settings](https://docs.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest#az_aks_create) if the above isn't what you had in mind.

You can now fetch the credentials with:

bash

```
az aks get-credentials --resource-group learnk8sResourceGroup --name learnk8s-cluster
Merged "learnk8s-cluster" as current context in /home/ubuntu/.kube/config
```

And verify that you can access the AKS cluster with kubectl:

bash

```
kubectl get nodes
NAME                                STATUS   ROLES   AGE   VERSION
aks-nodepool1-12768183-vmss000000   Ready    agent   13m   v1.18.14
aks-nodepool1-12768183-vmss000001   Ready    agent   13m   v1.18.14
```

If needed, you can modify the cluster with the `az aks update` command.

> The complete list of `az aks <command>` commands is available [on the official Azure CLI documentation.](https://docs.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest)

As an example, you can enable autoscaling and set the minimum and a maximum number of nodes with:

bash

```
az aks update \
  --resource-group learnk8sResourceGroup \
  --name learnk8s-cluster \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 2
```

Be patient and wait for the update to finish.

Once it's done, you can find in the output that **enableAutoScaling** is set to **true**.

bash

```
"agentPoolProfiles": [
    {
      "availabilityZones": null,
      "count": 2,
      "enableAutoScaling": true,
      "enableNodePublicIp": false,
[output truncated]
```

To verify and get more detailed info, you can use `az aks show` with the `-o yaml` for easier reading:

bash

```
az aks show --name learnk8s-cluster --resource-group learnk8sResourceGroup -o yaml
```

Voila! With this, you have successfully created and updated an AKS cluster through the Azure CLI!

You can delete the cluster and resource group now, as you will learn another way to deploy and manage them.

bash

```
az aks delete --name learnk8s-cluster --resource-group learnk8sResourceGroup
```

You can also delete the resource group with:

bash

```
az group delete --resource-group learnk8sResourceGroup
```

On the prompts, just confirm with `y` and wait for the operations to finish.

All the resources in that resource group will be deleted as well.

## Provisioning an AKS cluster with Terraform

[Terraform](https://www.terraform.io/) is an open-source Infrastructure as a Code tool.

**Instead of writing the code to create the infrastructure, you define a plan of what you want to be executed, and you let Terraform create the resources on your behalf.**

The plan isn't written in YAML, though.

Instead Terraform uses [a language called HCL](https://github.com/hashicorp/hcl) - HashiCorp Configuration Language.

_In other words, you use HCL to declare the infrastructure you want to be deployed, and Terraform executes the instructions._

Terraform uses plugins called providers to interface with the resources in the cloud provider.

This further expands with modules as a group of resources and are the building blocks you will use to create a cluster.

But let's take a break from the theory and see those concepts in practice.

Before you can create a cluster with Terraform, you should install the binary.

You can find the [instructions on how to install the Terraform CLI from the official documentation.](https://learn.hashicorp.com/tutorials/terraform/install-cli)

Verify that the Terraform tool has been installed correctly with:

bash

```
terraform version
Terraform v0.14.6
```

Before diving into the code, there are few prerequisites needed to be done.

Terraform uses a different set of credentials to provision the infrastructure, so you should create those first.

You will first need to get your subscription ID.

bash

```
az account list
# OR, more advanced way to get it:
az account list |  grep -oP '(?<="id": ")[^"]*'
```

**Make a note now of your subscription id.**

> If you have more than one subscription, you can set your active subscription with `az account set --subscription="SUBSCRIPTION_ID"`. You still need to make a note of your subscription id.

Terraform needs a Service Principal to create resources on your behalf.

You can think of a Service Principal as a user identity (login and password) with a specific role and tightly controlled permissions to access your resources.

It could have fine-grained permissions such as only to create virtual machines or read from particular blob storage.

In your case, you need a Contributor Service Principal — enough permissions to create and delete resources.

You can create the Service Principal with:

bash

```
az ad sp create-for-rbac \
  --role="Contributor" \
  --scopes="/subscriptions/SUBSCRIPTION_ID"
```

The previous command should print a JSON payload like this:

bash

```
{
  "appId": "00000000-0000-0000-0000-000000000000",
  "displayName": "azure-cli-2021-02-13-20-01-37",
  "name": "http://azure-cli-2021-02-13-20-01-37",
  "password": "0000-0000-0000-0000-000000000000",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

Make a note of the `appId`, `password`, and `tenant`. You need those to set up Terraform.

Export the following environment variables:

bash

```
export ARM_CLIENT_ID=<insert the appId from above>
export ARM_SUBSCRIPTION_ID=<insert your subscription id>
export ARM_TENANT_ID=<insert the tenant from above>
export ARM_CLIENT_SECRET=<insert the password from above>
```

### Creating a resource group with Terraform

Let's create the most straightforward Terraform file.

Don't worry if you are not familiar with the Terraform code; I will explain everything in a minute.

Now create a file named `main.tf` with the following content:

main.tf

```
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=2.48.0"
    }
  }
}

provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "rg" {
  name     = "learnk8sResourceGroup"
  location = "northeurope"
}
```

You will notice something familiar. This time you will utilize Terraform code to create a resource group.

### Terraform commands

In the same directory run:

bash

```
terraform init
```

The command will initialize Terraform and execute a couple of crucial tasks.

1.  It downloads the Azure provider that is necessary to translate the Terraform instructions into API calls.
2.  It will create two more folders as well as a state file. The state file is used to keep track of the resources that have been created already.

Consider the files as a checkpoint; without them, Terraform won't know what has been already created or updated.

> If you further want to validate if the configuration is correct, you can do so with the `terraform validate` command.

You're now ready to create your resource group using Terraform.

Two commands are frequently used in succession.

The first is:

bash

```
terraform plan
[output truncated]
Plan: 1 to add, 0 to change, 0 to destroy.
```

Terraform will perform a dry-run and will prompt you with a detailed summary of what resources are about to create.

It's always a good idea to double-check what happens to your infrastructure before you commit the changes.

You don't want to accidentally destroy a database because you forgot to add or remove a resource.

Once you are happy with the changes, you can create the resources for **real** with:

bash

```
terraform apply
[output truncated]
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

On the prompt, confirm with `yes`, and Terraform will create the Resource Group.

Congratulations, you just used Terraform to provision a resource!

You can imagine that by adding more block resources, you can create more components in your infrastructure.

You can have a look at all the resources that you could create in the [left column of the official provider page for Azure.](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs)

> Please note that you should have sufficient knowledge of Azure and its resources to understand how components can be plugged in together. The documentation provides excellent examples, though.

Before you provision a cluster, let's clean up the existing resources.

You can delete the resource group with:

bash

```
terraform destroy
[output truncated]
Apply complete! Resources: 0 added, 0 changed, 1 destroyed.
```

Terraform prints a list of resources that are ready to be deleted, and as soon as you confirm, it destroys all the resources.

## Terraform step by step

Create a new folder with the following files:

-   `main.tf` - to store the actual code for the cluster.
-   `outputs.tf` - to define the outputs.

In the `main.tf` file, copy and paste the following code:

main.tf

```
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=2.48.0"
    }
  }
}
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "rg" {
  name     = "learnk8sResourceGroup"
  location = "northeurope"
}

resource "azurerm_kubernetes_cluster" "cluster" {
  name                = "learnk8scluster"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  dns_prefix          = "learnk8scluster"

  default_node_pool {
    name       = "default"
    node_count = "2"
    vm_size    = "standard_d2_v2"
  }

  identity {
    type = "SystemAssigned"
  }
}
```

And in the `outputs.tf` add the following:

outputs.tf

```
resource "local_file" "kubeconfig" {
  depends_on   = [azurerm_kubernetes_cluster.cluster]
  filename     = "kubeconfig"
  content      = azurerm_kubernetes_cluster.cluster.kube_config_raw
}
```

> Since there aren't many variables to define, creating a separate `variables.tf` file will be skipped for now.

That's a lot of code!

But don't worry, I will explain everything as soon as you create the cluster.

Continue and from the same folder run the commands as before:

To initialize Terraform, use:

bash

```
terraform init
```

To perform a dry-run and inspect what Terraform will create.

bash

```
terraform plan
# output truncated
Plan: 3 to add, 0 to change, 0 to destroy.
```

Finally, to apply everything and create the resources:

bash

```
terraform apply
# output truncated
Apply complete! Resources: 3 added, 0 changed, 0 destroyed.
```

After issuing the apply command, you will be prompted to confirm, and same as before, just type `yes`.

_It's time for a cup of coffee._

Provisioning a cluster on AKS takes, on average, about fifteen minutes.

When it's complete, if you inspect the current folder, you should notice a few new files:

bash

```
tree .
.
├── kubeconfig
├── main.tf
├── outputs.tf
├── terraform.tfstate
└── terraform.tfstate.backup
```

Terraform uses the `terraform.tfstate` to keep track of what resources were created.

The `kubeconfig` is the kube configuration file for the newly created cluster.

Inspect the cluster pods using the generated kubeconfig file:

bash

```
kubectl get node --kubeconfig kubeconfig
NAME                              STATUS   ROLES   AGE     VERSION
aks-default-75184889-vmss000000   Ready    agent   21m     v1.18.14
aks-default-75184889-vmss000001   Ready    agent   21m     v1.18.14
```

If you prefer to not prefix the `--kubeconfig` environment variable to every command, you can export the `KUBECONFIG` variable as:

bash

```
export KUBECONFIG="${PWD}/kubeconfig"
```

_The export is valid only for the current terminal session._

Now that you've created the cluster, it's time to go back and discuss the Terraform file.

The Terraform file that you just executed is divided into several blocks, so let's look at each one of them.

main.tf

```
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=2.48.0"
    }
  }
}
provider "azurerm" {
  features {}
}
```

The first two blocks of code are the [required providers(Terraform v0.13+) and provider.](https://www.terraform.io/docs/language/providers/requirements.html#requiring-providers)

This is where you define your Terraform configuration with which provider (AWS, GCP, Azure) you will work with, and that must be installed.

The source and versions are self-explanatory.

You define the URL where to download the provider, usually `hashicorp/provider` and which version from that provider.

> If you want to learn more about version constraints, you can take a look [here](https://www.terraform.io/docs/language/expressions/version-constraints.html).

main.tf

```
resource "azurerm_resource_group" "rg" {
  name     = "learnk8sResourceGroup"
  location = "northeurope"
}
```

In the above [resource](https://www.terraform.io/docs/language/resources/syntax.html) block, you define which resource you want to be created.

In this case, a Resource Group along with its required parameters.

Finally, there is one more resource definition needed:

main.tf

```
resource "azurerm_kubernetes_cluster" "cluster" {
  name                = "learnk8scluster"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  dns_prefix          = "learnk8scluster"

  default_node_pool {
    name       = "default"
    node_count = "2"
    vm_size    = "standard_d2_v2"
  }

  identity {
    type = "SystemAssigned"
  }
}
```

Let's explain in detail what is defined in the code here.

In the first part, the `azurerm_kubernetes_cluster` is the [actual resource](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/kubernetes_cluster), that manages an Azure Kubernetes cluster.

The `cluster` is the **locally given name** for that resource that is only to be used as a reference **inside the scope** of the module.

The `name` and `dns_prefix` are used to define the cluster's name and DNS name.

Notice the lengthy values for `location` and `resource_group_name`.

Those values have already been defined inside the Resource Group block, but you can retrieve them as attributes.

In the snippet above, the attribute `azurerm_resource_group.rg.location` resolves to `northeurope` and `resource_group_name` to `learnk8sResourceGroup`.

Referencing attributes is convenient, so you can tweak the value in a single place instead of copying and pasting it everywhere.

In the `default_node_pool` you are defining the specs for the worker nodes.

In short, Terraform will create a pool named `default`, consisting of `2` nodes, with an instance type of `standard_d2_v2`.

Finally, the last block is used to define the type of the `identity`, which is `SystemAssigned`.

This means that Azure will automatically create the required roles and permissions, and you won't need to manage any credentials.

These credentials are tied to the lifecycle of the service instance.

> You can read more about the types [here](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview#managed-identity-types).

The `outputs.tf`, as its name suggests, will output some value that you define in it.

outputs.tf

```
resource "local_file" "kubeconfig" {
  depends_on   = [azurerm_kubernetes_cluster.cluster]
  filename     = "kubeconfig"
  content      = azurerm_kubernetes_cluster.cluster.kube_config_raw
}
```

The resource here will create a local file populated with the kube configuration to generate access for the cluster.

The required parameters are `filename` and the `content`, which again use local value the `kube_config_raw`.

The `depends_on` is not required, but it's best to set it as a precaution.

This is a [meta-argument](https://www.terraform.io/docs/language/meta-arguments/depends_on.html) that sets a dependency on _something_ either a resource or module before another code block gets executed.

Here you want the cluster to be created **first** before fetching the kubeconfig value. Otherwise, Terraform may create an empty file.

### The Azure CLI vs Terraform — pros and cons

You can already tell the main differences between the Azure CLI and Terraform:

-   Both create an AKS cluster.
-   Both export a valid `kubeconfig` file.
-   The configuration with the Azure CLI is more straightforward and more concise.
-   Terraform goes into great detail and is more granular. You have to craft every single resource carefully.

_So which one should you use?_

**For smaller experiments, when you need to spin a cluster quickly, you should consider using the Azure CLI.**

With a short command, you can easily create it.

**For production-grade infrastructure where you want to configure and tune every single detail of your cluster, you should consider using Terraform.**

But there's another crucial reason why you should prefer Terraform - incremental updates.

Let's imagine that you want to add a second pool to your cluster.

Perhaps you want to add another - more memory-optimized node pool to your cluster for your memory-hungry applications.

> **NOTE:** To proceed with the changes, you will have to reduce the node count to one from the **default pool.**

As mentioned before, there are resource quotas that limit the CPU cores to 4.

You can edit the file and add the new node pool at the bottom of the config as follows:

main.tf

```
# ...
resource "azurerm_kubernetes_cluster_node_pool" "mem" {
 kubernetes_cluster_id = azurerm_kubernetes_cluster.cluster.id
 name                  = "mem"
 node_count            = "1"
 vm_size               = "standard_d11_v2"
}
```

Proceed with the previous commands to plan and apply the changes:

bash

```
terraform plan
Plan: 1 to add, 1 to change, 0 to destroy.
```

bash

```
terraform apply
azurerm_kubernetes_cluster.cluster: Modifying...
[output truncated]
Apply complete! Resources: 1 added, 1 changed, 0 destroyed.
```

Be patient for the two operations to finish.

After the operation is done, you can verify that the new node pool has been added with:

bash

```
kubectl get nodes --kubeconfig kubeconfig
NAME                              STATUS   ROLES   AGE     VERSION
aks-default-75184889-vmss000000   Ready    agent   32m     v1.18.14
aks-mem-75184889-vmss000000       Ready    agent   2m15s   v1.18.14
```

Success!! You not only have created a production-ready cluster but also modified it to have an additional node pool.

Now it's time to deploy an application.

_Let's do that next._

## Testing the cluster by deploying a simple Hello World app

You can create a Deployment with the following YAML definition:

deployment.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-kubernetes
spec:
  selector:
    matchLabels:
      name: hello-kubernetes
  template:
    metadata:
      labels:
        name: hello-kubernetes
    spec:
      containers:
        - name: app
          image: paulbouwer/hello-kubernetes:1.8
          ports:
            - containerPort: 8080
```

You can also find all the files for the demo app [here](https://github.com/k-mitevski/terraform-aks/tree/master/kubernetes).

> **NOTE:** To make it easier to run commands to the cluster without specifying the `--kubeconfig` parameter constantly. You can either export or move the generated `kubeconfig` to `~/.kube/config`.

If you have other configs located there, it's a good idea to back up that file!

bash

```
mv kubeconfig ~/.kube/config
```

You can submit the definition to the cluster with:

bash

```
kubectl apply -f deployment.yaml
deployment.apps/hello-kubernetes created
```

To see if your application runs correctly, you can connect to it with `kubectl port-forward`.

First, retrieve the name of the Pod:

bash

```
kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
hello-kubernetes-7f65c7597f-8dn2l   1/1     Running   0          92s
```

You can connect to the Pod with:

bash

```
kubectl port-forward hello-kubernetes-7f65c7597f-8dn2l 8080:8080
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080
```

Or you can use the trickier option that will automatically get the pod's name:

bash

```
kubectl port-forward $(kubectl get pod -l name=hello-kubernetes --no-headers | awk '{print $1}') 8080:8080
```

The `kubectl port-forward` command connects to the Pod with the name `hello-kubernetes-7f65c7597f-8dn2l`.

And forwards all the traffic from port 8080 on the Pod to port 8080 on your computer.

If you visit [http://localhost:8080](http://localhost:8080/) on your computer, you should be greeted by the application's web page.

_Great!_

Exposing the application with `kubectl port-forward` is an excellent way to test the app quickly, but it isn't a long-term solution.

**If you wish to route live traffic to the Pod, you should have a more permanent solution.**

In Kubernetes, you can use a Service of `type: LoadBalancer` to start up a load balancer to expose your Pods.

You can use the following code:

service-loadbalancer.yaml

```
apiVersion: v1
kind: Service
metadata:
  name: hello-kubernetes
spec:
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: 8080
  selector:
    name: hello-kubernetes
```

And submit the YAML with:

bash

```
kubectl apply -f service-loadbalancer.yaml
service/hello-kubernetes created
```

As soon as you submit the command, AKS provisions an L4 Load Balancer and connects it to your pod.

Eventually, you should be able to describe the Service and retrieve the load balancer's IP address.

Running:

bash

```
kubectl get svc
NAME               TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)        AGE
hello-kubernetes   LoadBalancer   10.0.221.11   20.82.170.76   80:31326/TCP   44s
```

If you visit the `external IP` address in your browser, you should see the application.

_Excellent!_

There's only an issue, though.

**The load balancer that you created earlier serves one service at a time.**

Also, it has no option to provide intelligent routing based on paths.

So if you have multiple services that need to be exposed, you will need to create the same number of load balancers.

Imagine having ten applications that have to be exposed.

If you use a Service of `type: LoadBalancer` for each of them, you might end up with ten different L4 Load Balancers.

**That wouldn't be a problem if those load balancers weren't so expensive.**

How can you get around this issue?

## Routing traffic into the cluster with an Ingress

In Kubernetes, another resource is designed to solve that problem: [the Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/).

The Ingress has two parts:

1.  The first is the Ingress object which is the same as Deployment or Service in Kubernetes. This is defined by the kind part of the YAML manifest.
2.  The second part is the Ingress controller. This is the actual part that controls the load balancers, so they know how to serve the requests and forward the data to the Pods.

In other words, the Ingress controller acts as a reverse proxy that routes the traffic to your Pods.

![The Ingress in Kubernetes](https://learnk8s.io/a/955d2faae8baafd42e0ab4c5fbb3f463.svg)

The Ingress routes the traffic based on paths, domains, headers, etc., which consolidates multiple endpoints in a single resource that runs inside Kubernetes.

With this, you can serve multiple services simultaneously from one exposed endpoint - the load balancer.

There're lots of Ingress controllers that you can choose from:

1.  [Nginx Ingress](https://kubernetes.github.io/ingress-nginx/)
2.  [Ambassador](https://github.com/datawire/ambassador)
3.  [Traefik](https://traefik.io/traefik/)
4.  [And more.](https://docs.google.com/spreadsheets/u/1/d/191WWNpjJ2za6-nbG4ZoUMXMpUK8KlCIosvQB0f-oq3k)

However, in this part, the AKS has its own [add-on](https://docs.microsoft.com/en-us/azure/aks/http-application-routing) that enables the use of Ingress controller.

Azure provides two ways to enable the Ingress in the cluster.

Either through using the Azure CLI or by defining it in Terraform code.

In the next section, you will learn how to use Terraform to enable Azure Ingress.

First, you will be amending the `main.tf` file to add the required add-on setting to enable the Azure Ingress controller.

You can either get the new `main.tf` file from [here](https://github.com/k-mitevski/terraform-aks/tree/master/04_aks_ingress), or copy and paste it through here:

main.tf

```
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=2.48.0"
    }
  }
}
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "rg" {
  name     = "learnk8sResourceGroup"
  location = "northeurope"
}

resource "azurerm_kubernetes_cluster" "cluster" {
  name                = "learnk8scluster"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  dns_prefix          = "learnk8scluster"

  default_node_pool {
    name       = "default"
    node_count = "2"
    vm_size    = "standard_d2_v2"
  }
  identity {
    type = "SystemAssigned"
  }
  addon_profile {
    http_application_routing {
      enabled = true
    }
  }
}
```

Notice the last part - `addon_profile`; with this option, you can enable the Ingress controller for the AKS cluster.

Of course, there are [other cluster-specific add-ons available as well.](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/kubernetes_cluster#aci_connector_linux)

The add-on can be enabled even if the cluster is already created.

After enabling it, you should (plan and) apply the changes.

You will notice a few new Pods in the cluster:

bash

```
kubectl get pods -n kube-system | grep addon
addon-http-application-routing-default-http-backend-554cd5gsc9x   1/1     Running   0          17m
addon-http-application-routing-external-dns-cd8f8b797-zw2z8       1/1     Running   0          17m
addon-http-application-routing-nginx-ingress-controller-5c8z4v4   1/1     Running   0          17m
```

In Azure, the Ingress add-on installs [Ingress Nginx.](https://kubernetes.github.io/ingress-nginx/)

On top of that, enabling the add-on also installs the [ExternalDNS](https://github.com/kubernetes-sigs/external-dns) that can be used to manage DNS entries automatically.

As soon as you install the add-on, Azure creates an L4 Load Balancer and configures it to route traffic to the Ingress Nginx.

When you submit an Ingress manifest to Kubernetes, the Ingress controller reconfigures itself to route traffic to that Service (and Pods).

Let's have a look at an example:

ingress.yaml

```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: hello-kubernetes
  annotations:
    kubernetes.io/ingress.class: addon-http-application-routing
spec:
  backend:
    serviceName: hello-kubernetes
    servicePort: 80
  rules:
    - http:
        paths:
          - path: /
            backend:
              serviceName: hello-kubernetes
              servicePort: 80
```

The following Kubernetes Ingress manifest routes all the traffic from path `/` to the Pods targeted by the `hello-kubernetes` Service.

As soon as you submit the resource to the cluster, the Ingress controller is notified of the new resource.

-   ![Consider the following cluster with three Nodes and a single pod with a web application.](https://learnk8s.io/a/9b5855bd3b25cf7dbeb54d471fbf4027.svg)
    
    1/4
    
    Consider the following cluster with three Nodes and a single pod with a web application.
    
    NEXT 
    
-   
-   
-   

As with every Ingress controller, it provides convenience since you can control your infrastructure uniquely from Kubernetes — there's no need to fiddle with AKS anymore.

Now that you know about the Ingress, you can give it a try and deploy it.

```
kubectl apply -f ingress.yaml
```

Create the Ingress resource by applying the `ingress.yaml` manifest from above.

> **Note**: You should delete the previous Load Balancer Service and instead deploy the [`service.yaml`](https://github.com/k-mitevski/terraform-aks/blob/master/kubernetes/service.yaml), so you don't end up with duplicate load balancers.

Pay attention to the annotation field, though.

`kubernetes.io/ingress.class: addon-http-application-routing` is used to select the right Ingress controller.

It may take a few minutes (the first time) for the Ingress to be created, but eventually, you will see the following output if you describe it:

bash

```
kubectl describe ingress hello-kubernetes

Name:             hello-kubernetes
Namespace:        default
Address:          20.54.107.12
Default backend:  hello-kubernetes:80 (10.244.1.2:8080)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /   hello-kubernetes:80 (10.244.1.2:8080)
Annotations:  kubernetes.io/ingress.class: addon-http-application-routing
Events:
  Type    Reason  Age   From                      Message
  ----    ------  ----  ----                      -------
  Normal  CREATE  98s   nginx-ingress-controller  Ingress default/hello-kubernetes
  Normal  UPDATE  50s   nginx-ingress-controller  Ingress default/hello-kubernetes
```

Excellent, you can use the IP from the `Address` field to visit your application in the browser.

The Ingress add-on is meant as a quick way to install an Ingress and route traffic in the cluster.

However, if you want a more robust solution that can also serve HTTPS traffic, [take a look at the following section](https://docs.microsoft.com/en-us/azure/aks/ingress-tls) on how you can deploy Nginx Ingress controller using Helm.

Congratulations!

You've managed to deploy a fully working cluster that can route live traffic!

## Fully automated Dev & Production environments with Terraform modules

One of the most common tasks when provisioning infrastructure is to create separate environments.

1.  A development environment where you can test your changes and integrate them with other colleagues.
2.  A production environment.

Since you want your apps to progress through the environments, you might want to provision multiple clusters, one for each environment.

When you don't have infrastructure as code, you are forced to click on the user interface and repeat the same choice.

But since you've mastered Terraform, you can refactor your code and create multiple environments with a single command!

**The beauty of Terraform is that you can use the same code to generate several clusters with different names.**

You can parametrize the name of your resources and create clusters that are exact copies.

You can reuse the existing Terraform code and provision two clusters simultaneously using [Terraform modules](https://www.terraform.io/docs/modules/index.html) and [expressions](https://www.terraform.io/docs/configuration/expressions.html).

> Before you execute the script, it's a good idea to destroy any cluster that you created previously with `terraform destroy`.

You saw the mention of [Terraform variables](https://www.terraform.io/docs/configuration/variables.html) earlier, but let's revisit them in more detail.

The expression syntax is straightforward.

You define variables like this:

variables.tf

```
variable "cluster_name" {
  description = "The name for the AKS cluster"
  default     = "learnk8scluster"
}
variable "env_name" {
  description = "The environment for the AKS cluster"
  default     = "dev"
}
```

You can create and add the definitions in a `variables.tf` file.

Later, you can reference and chain the variables in the AKS resource like this:

variables.tf

```
# output truncated for clarity
resource "azurerm_kubernetes_cluster" "cluster" {
  name       = "${var.cluster_name}-${var.env_name}"
```

Terraform will interpolate the string to "learnk8scluster-dev".

When you execute the usual `terraform apply` command, you can override the variable with a different name.

For example:

bash

```
terraform apply -var="cluster_name=my-cluster" -var="env_name=staging"
```

This will provision a cluster with the name of `my-cluster-staging`.

In isolation, expressions are not particularly useful.

Let's have a look at an example.

If you execute the following commands, what do you expect?

Is Terraform creating two clusters or updates the dev cluster to a staging cluster?

bash

```
terraform apply -var="env_name=dev"
# and later
terraform apply -var="env_name=staging"
```

The code updates the dev cluster to a staging cluster.

It's overwriting the same cluster!

But what if you wish to create a copy?

You can use the Terraform modules to your advantage.

Terraform modules use variables and expressions to encapsulate resources.

Move your `main.tf`, `variables.tf`, and `outputs.tf` in a subfolder, such as another folder named `main`, and create an empty `main.tf`.

bash

```
mkdir -p main
mv main.tf variables.tf outputs.tf main
tree .
.
├── main
│   ├── variables.tf
│   └── main.tf
│   └── outputs.tf
└── main.tf
```

From now on, you can use the code that you've created as a reusable module.

In the subfolder, where the `main.tf` file is located, append the `env_name` variable to the Resource Group.

In this way way you can differentiate and create different clusters in separate resource groups:

main.tf

```
# output truncated for clarity
resource "azurerm_resource_group" "rg" {
  name     = "learnk8sResourceGroup-${var.env_name}"
```

Now you can reference all the code from the `main.tf` resource like this:

main.tf

```
module "prod_cluster" {
    source       = "./main"
    env_name     = "prod"
    cluster_name = "learnk8scluster"
}
```

And since the module is reusable, you can create more than a single cluster:

main.tf

```
module "dev_cluster" {
    source       = "./main"
    env_name     = "dev"
    cluster_name = "learnk8scluster"
}

module "prod_cluster" {
    source       = "./main"
    env_name     = "prod"
    cluster_name = "learnk8scluster"
}
```

You can preview the changes with:

bash

```
terraform plan
# output truncated
Plan: 6 to add, 0 to change, 0 to destroy.
```

> **Warning!** Before applying, you should be aware that there are quota limits on the free tier account for AKS, as mentioned before. The most critical is tied to the CPU core quota. To circumvent that for this tutorial purposes, the Terraform code for running multiple clusters is changed to deploy two clusters with single node pools instead of the usual three.

You can apply the changes and create two clusters that are exact copies with:

```
terraform apply
Apply complete! Resources: 6 added, 0 changed, 0 destroyed.
```

The two clusters have the AKS Ingress add-on enabled automatically, so they can handle external traffic.

What happens when you update the cluster module?

When you modify a property, Terraform will update all clusters with the same property.

If you wish to customize the properties on a per-environment basis, you should extract the parameters in variables and change them from the root `main.tf`.

**Let's have a look at an example.**

You might want to run the same instance type such as `standard_d2_v2` in the dev environment but change to `standard_d11_v2` instance type for production.

As an example you can refactor the code and extract the instance type as a variable:

```
variable "instance_type" {
  default = "standard_d2_v2"
}
```

And add the corresponding change in the Azure resource like:

```
#...
default_node_pool {
  name       = "default"
  node_count = "1"
  vm_size    = var.instance_type
}
#...
```

> Notice the variable definition; since we aren't chaining two or more variables, there is no need to declare it with `${}`. Instead, you can define it directly with `var.variable_name`.

Later, you can modify the root `main.tf` file with the instance type:

```
module "dev_cluster" {
    source       = "./main"
    env_name     = "dev"
    cluster_name = "learnk8scluster"
    instance_type= "standard_d2_v2"
}

module "prod_cluster" {
    source       = "./main"
    env_name     = "prod"
    cluster_name = "learnk8scluster"
    instance_type= "standard_d11_v2"
}
```

If you wish, you can apply the changes and verify each cluster with its corresponding kubeconfig file.

bash

```
kubectl get nodes --kubeconfig kubeconfig-dev
NAME                              STATUS   ROLES   AGE    VERSION
aks-default-14429859-vmss000000   Ready    agent   3m2s   v1.18.14

kubectl get nodes --kubeconfig kubeconfig-prod
NAME                              STATUS   ROLES   AGE     VERSION
aks-default-75925878-vmss000000   Ready    agent   2m47s   v1.18.14
```

Excellent!

As you can imagine, you can add more variables to your module and create environments with different configurations and specifications.

This marks the end of your journey!

A recap on what you've built so far:

-   You've created an AKS cluster both using the Azure CLI and Terraform
-   You used the AKS add-on to enable Ingress, define a resource, and route live traffic.
-   You parameterized the cluster configuration and created a reusable module.
-   You used the module as an extension to provision multiple copies of your cluster (dev and production).
-   You made the module more flexible by allowing minor customizations such as changing the instance type.
-   Well done!

## That's all folks!

If you enjoyed this article, you might find the following articles interesting:

-   [Getting started with Docker and Kubernetes on Windows 10](https://learnk8s.io/blog/installing-docker-and-kubernetes-on-windows) where you'll get your hands dirty and install Docker and Kubernetes in your Windows environment.
-   [3 simple tricks for smaller Docker images.](https://learnk8s.io/blog/smaller-docker-images) Docker images don't have to be large. Learn how to put your Docker images on a diet!