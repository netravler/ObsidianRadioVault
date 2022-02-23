# Deploying to Azure using Terraform and GitHub Actions

05 SEPTEMBER 2020 BY [VASSILI ALTYNIKOV](https://www.blendmastersoftware.com/authors/valtynikov)

![](https://www.blendmastersoftware.com/images/blog/github-merge-terraform-apply.png)

I recently had to setup an Azure infrastructure deployment pipeline for a new project and decided to experiment with GitHub Actions for workflow automation. The documentation for both Terraform and GitHub Actions is great, but I didn’t find instructions to do exactly what I wanted, so I decided to share my findings in this blog post.

Hopefully you find this information useful and it saves you some time. If you notice any issues with the approach or have other suggestions, please share your feedback in comments!

#### 1. Prerequisites

##### 1.1. GitHub repo

Create a new GitHub repo for Terraform configuration files (or use an existing repo if you already have one).

If creating a new repository, check the **Add .gitignore** option and select the `Terraform` template.

![Creating a new GitHub repo with a Terraform .gitignore template](https://www.blendmastersoftware.com/images/blog/github-new-repo-terraform.png)

If using an existing repo, update your `.gitignore` using the [GitHub’s Terraform .gitignore template](https://github.com/github/gitignore/blob/master/Terraform.gitignore).

Clone the GitHub repo to your local machine.

##### 1.2. Azure CLI

Download and install the [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

Authenticate with Azure using the `az login` command.

If you have access to multiple Azure subscriptions, select a specific one by running `az account set -s <subscription-id>`.

You can see the list of subscriptions you have access to by running `az account list`.

##### 1.3. Azure service principal

You need to create an Azure service principal to run Terraform in GitHub Actions.

Run the following command to create the service principal and grant it **Contributor** access to the Azure subscription.

```
az ad sp create-for-rbac --name "sp-hello-azure-tf" --role Contributor --scopes /subscriptions/<subscription-id> --sdk-auth
```

Save the output of the command. You’ll need this information later in the process.

For more information, please refer to [Authenticating using a Service Principal with a Client Secret](https://www.terraform.io/docs/providers/azurerm/guides/service_principal_client_secret.html) in Terraform docs.

##### 1.4. Terraform

[Download Terraform](https://www.terraform.io/downloads.html) and ensure it’s in your system’s `PATH`.

Create a Terraform backend storage account and container.

```
az group create -g rg-hello-azure-tf -l northcentralus

az storage account create -n sahelloazuretf -g rg-hello-azure-tf -l northcentralus --sku Standard_LRS

az storage container create -n terraform-state --account-name sahelloazuretf
```

#### 2. Terraform configuration

Create a new file `main.tf` in the Git repo.

```
provider "azurerm" {
  version = "=2.0.0"
  features {}
}

terraform {
  backend "azurerm" {
    resource_group_name  = "rg-hello-azure-tf"
    storage_account_name = "sahelloazuretf"
    container_name       = "terraform-state"
    key                  = "terraform.tfstate"
  }
}

resource "azurerm_resource_group" "rg-hello-azure" {
  name     = "rg-hello-azure"
  location = "northcentralus"
}
```

Run `terraform init` to initialize Terraform.

You can now run `terraform plan` and see the execution plan.

This Terraform configuration allows you to test changes locally and review the execution plan before committing the changes to Git.

#### 3. GitHub Actions

Create a folder `.github` and a subfolder `workflows` in the Git repo.

Next, we’ll create a couple of workflows based on the [GitHub Actions Workflow YAML](https://www.terraform.io/docs/github-actions/setup-terraform.html#github-actions-workflow-yaml) section of Terraform documentation.

##### 3.1. Pull request validation workflow

Create a file `terraform-plan.yml` in the `workflows` subfolder. Replace the `<client-id>`, `<subscription-id>` and `<tenant-id>` with the values from the output of the command executed in step **1.3** above. We’ll take care of the `ARM_CLIENT_SECRET` value later.

```
name: Terraform Plan

on:
  pull_request:
    branches: [ master ]

jobs:
  terraform:
    runs-on: ubuntu-latest

    env:
      ARM_CLIENT_ID: <client-id>
      ARM_CLIENT_SECRET: ${{secrets.TF_ARM_CLIENT_SECRET}}
      ARM_SUBSCRIPTION_ID: <subscription-id>
      ARM_TENANT_ID: <tenant-id>

    steps:
      - uses: actions/checkout@v2

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v1

      - name: Terraform Init
        run: terraform init

      - name: Terraform Format
        run: terraform fmt -check

      - name: Terraform Plan
        run: terraform plan

```

This workflow will automatically trigger on all pull requests into the `master` branch and generate Terraform execution plan for the proposed change. The pull request approver can then easily review the change without having to pull the branch and generating the execution plan locally.

##### 3.2. Apply changes on merge

Create another file `terraform-apply.yml` in the `workflows` subfolder. Same as before, replace the `<client-id>`, `<subscription-id>` and `<tenant-id>` with the values and leave the `ARM_CLIENT_SECRET` as-is for now.

```
name: Terraform Apply

on:
  push:
    branches: [ master ]

jobs:
  terraform:
    runs-on: ubuntu-latest

    env:
      ARM_CLIENT_ID: <client-id>
      ARM_CLIENT_SECRET: ${{secrets.TF_ARM_CLIENT_SECRET}}
      ARM_SUBSCRIPTION_ID: <subscription-id>
      ARM_TENANT_ID: <tenant-id>

    steps:
      - uses: actions/checkout@v2

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v1

      - name: Terraform Init
        run: terraform init

      - name: Terraform Apply
        run: terraform apply -auto-approve

```

This workflow will automatically deploy changes merged to the `master` branch. You’d want to make sure that the `master` [branch is protected](https://docs.github.com/en/github/administering-a-repository/about-protected-branches) and all changes successfully pass the pull request validation before they get merged.

##### 3.3. Create the repository secret

The final step of the GitHub repo configuration is creating the `TF_ARM_CLIENT_SECRET` secret referenced by the workflows.

Navigate to the repository **Settings** page, then select **Secrets** in the left nav. Create a new secret `TF_ARM_CLIENT_SECRET` using the client secret value from step **1.3**.

![Creating a new GitHub repository secret](https://www.blendmastersoftware.com/images/blog/github-tf-arm-client-secret.png)

You can learn more about GitHub secrets at [Creating and storing encrypted secrets](https://docs.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets).

#### 4. Test the workflow

All pieces are now in place and we can start using the new GitHub Actions workflows.

##### 4.1. Validate changes

Checkout a new branch using `git checkout -b <branch-name>` and commit your changes.

Publish the branch and create a pull request.

You should see the **Terraform Plan** workflow kick off automatically after a few seconds.

![GitHub Actions pull request validation workflow - Terraform Plan](https://www.blendmastersoftware.com/images/blog/github-pr-terraform-plan.png)

Click on **Details** and drilldown into the **Terraform Plan** step to review the execution plan.

![GitHub Actions pull request validation workflow - Terraform Plan details](https://www.blendmastersoftware.com/images/blog/github-pr-terraform-plan-details.png)

##### 4.2. Apply changes

If you are satisfied with the Terraform plan, merge the pull request.

Navigate to the **Actions** tab. You should see the **Terraform Apply** workflow kick off automatically after the merge.

Drilldown into the **Terraform Apply** logs to verify that the changes were deployed.

![GitHub Actions - Terraform Apply on merge](https://www.blendmastersoftware.com/images/blog/github-merge-terraform-apply.png)

#### Thoughts?

Do you have any comments, concerns or suggestions? Please feel free to share your thoughts in the comments below. I’d love to hear your feedback!

---

![Vassili Altynikov](https://www.blendmastersoftware.com/images/valtynikov_rnd_sm.jpg)

ABOUT THE AUTHOR

###### [Vassili Altynikov](https://www.blendmastersoftware.com/authors/valtynikov)

Principal DevOps Architect at Blend Master Software

---

[Backup your Azure DevOps variable groups to a Git repo](https://www.blendmastersoftware.com/blog/azure-devops-variable-groups-backup)



# ANOTHER EXAMPLE:

## [Jake Walsh](https://jakewalsh.co.uk/ "Jake Walsh")

### Azure / Cloud / DevOps / EUC / and more…

---

# Deploying Azure Resources with Terraform using GitHub Actions

July 28, 2021 · [Azure](https://jakewalsh.co.uk/category/azure/), [Community](https://jakewalsh.co.uk/category/community/), [GitHub](https://jakewalsh.co.uk/category/github/), [GitHub Actions](https://jakewalsh.co.uk/category/github-actions/), [HashiCorp](https://jakewalsh.co.uk/category/hashicorp/), [HashiCorp Ambassador](https://jakewalsh.co.uk/category/hashicorp-ambassador/), [Terraform](https://jakewalsh.co.uk/category/terraform/)

Tagged: [Azure](https://jakewalsh.co.uk/tag/azure/), [CloudFamily](https://jakewalsh.co.uk/tag/cloudfamily/), [Community](https://jakewalsh.co.uk/tag/community/), [GitHub](https://jakewalsh.co.uk/tag/github/), [GitHub Actions](https://jakewalsh.co.uk/tag/github-actions/), [HashiCorp](https://jakewalsh.co.uk/tag/hashicorp/), [Lab](https://jakewalsh.co.uk/tag/lab/), [Microsoft](https://jakewalsh.co.uk/tag/microsoft/), [Terraform](https://jakewalsh.co.uk/tag/terraform/)

To deploy any of the **[Terraform-Azure](https://github.com/jakewalsh90/Terraform-Azure)** Environments, or any Terraform code for Azure you have created yourself, using GitHub Actions, follow the steps outlined below.

##### 1. Fork the [Terraform-Azure](https://github.com/jakewalsh90/Terraform-Azure) Repository

This will then allow you to access backend settings privately and create your own deployments based on the code within. You can then of course customise code, make changes, or upload your own Terraform files.

##### 2. Setup the Backend Storage and Service Principal

Before we can start to run any Actions, we need two supporting items in place:

1. Backend Storage for Terraform to store the state file  
2. A Service Principal for Terraform to use to authenticate to our Azure Tenant/Subscriptions

I have created an Azure CLI script that does all this for you! See here: **[https://raw.githubusercontent.com/jakewalsh90/Terraform-Azure/main/GitHub-Actions-Deployment/scripts/AzCLIPreReqSetup](https://raw.githubusercontent.com/jakewalsh90/Terraform-Azure/main/GitHub-Actions-Deployment/scripts/AzCLIPreReqSetup)**

Before running the above script, be sure to run through and update the following items in the script to suit your needs. Look through the “Set the below” section of the script and update the variables. Note: Storage Account names must be unique so change this to something that suits your deployment! You will also need to provide a Subscription ID.

When the script runs, you will have a Storage Account and Container setup, and also you will see an output similar to the below:

[![](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/ScriptOutputSample.png?resize=780%2C211&ssl=1)](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/ScriptOutputSample.png?ssl=1)

Copy all of the values outputted by the script and save them somewhere. We will need them for subsequent tasks.

##### 3. Configure a Terraform Backend

Within your Terraform, you will need to configure a backend. This is so that Terraform knows where you would like the State file to be stored. This will be the Azure Resources we created earlier using the Azure CLI Script (Storage Account and Container). The script outputted the values required below – be sure to use your Resource Group, Storage Account, and Container name correctly, based on the output of the Azure CLI script:

#backend

terraform {

backend "azurerm" {

resource_group_name = "your-resource-group"

storage_account_name = "yourstorageaccount123"

container_name = "tfstate"

key = "terraform.tfstate"

}

}

Once this has been configured, save the file. For any of the projects in the **Terraform-Azure** repo, add the above (when changed with your own values) to **azuredeploy.tf** in the project folder.

##### 4. Configure the Secrets within the GitHub Repo

At the end of running the CLI Script, you will also have noticed 4 outputs:

**ARM_CLIENT_ID:**  
**ARM_CLIENT_SECRET:**  
**ARM_TENANT_ID:**  
**ARM_SUBSCRIPTION_ID:**

These are the details we will need to store as Secrets (**[https://docs.github.com/en/actions/reference/encrypted-secrets](https://docs.github.com/en/actions/reference/encrypted-secrets)**) within the Repository, so that Terraform can authenticate correctly to Azure. Configure the secrets using the Settings section of the GitHub repo – name the secrets as shown in the screenshot, and paste in the outputs for each one as the earlier Azure CLI script provided:

[![](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/GitHubSecrets.png?resize=780%2C476&ssl=1)](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/GitHubSecrets.png?ssl=1)

**We are now ready to start setting up some GitHub Actions!** ![😄](https://s.w.org/images/core/emoji/13.1.0/svg/1f604.svg)

##### 5. Setting up GitHub Actions

The actions for this task are configured using YAML. To keep things simple and easy, two sample Actions have been created within this repository:

-   **Terraform Apply** – this will setup, initialise, validate, plan and apply Terraform, based on the chosen directory.
-   **Terraform Destroy** – this will setup, initialise, and run Terraform destroy, based on the chosen directory.

These are both contained, and configured from within the **[./github/workflows](https://github.com/jakewalsh90/Terraform-Azure/tree/main/.github/workflows)** folder within the Terraform-Azure repository. Both of these are set to run only when manually triggered (using **[workflow_dispatch]**). If you browse to the Actions tab in the repo – you will see both actions are available:

[![](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/Actions1.png?resize=673%2C349&ssl=1)](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/Actions1.png?ssl=1)

All that needs to be configured before running the actions is the folder within the Action – as this will determine which Terraform files are used. For example, if you wish to deploy the Single Region Azure BaseLab within this repo, change the folder in the image below to ./Single-Region-Azure-BaseLab:

[![](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/Actions2.png?resize=780%2C446&ssl=1)](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/Actions2.png?ssl=1)

**We are now ready to run GitHub Actions! ![✅](https://s.w.org/images/core/emoji/13.1.0/svg/2705.svg)** 

##### 6. Running GitHub Actions and deploying Terraform

As previously mentioned, the actions are set to be run manually within the YAML. So we need to manually trigger the deployment. To do this, browse to the Action, and then follow the steps in the image below:

[![](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/Actions3.png?resize=780%2C344&ssl=1)](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/Actions3.png?ssl=1)

The actions will then be run, and your Azure Resources will be deployed:

[![](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/Actions4.png?resize=339%2C319&ssl=1)](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/Actions4.png?ssl=1)

Once completed – you will see the Azure Resources deployed from the Terraform you have run:

[![](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/Actions5.png?resize=527%2C175&ssl=1)](https://i0.wp.com/jakewalsh.co.uk/wp-content/uploads/2021/07/Actions5.png?ssl=1)

**Hope this helps! ![👍](https://s.w.org/images/core/emoji/13.1.0/svg/1f44d.svg) Any issues/questions please reach out via [Twitter](https://twitter.com/jakewalsh90) or open an [issue](https://github.com/jakewalsh90/Terraform-Azure/issues).![📡](https://s.w.org/images/core/emoji/13.1.0/svg/1f4e1.svg)**