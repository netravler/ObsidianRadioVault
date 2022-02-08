# Install IIS on Azure VM using Terraform

[![](data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjAwIiBoZWlnaHQ9IjIwMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=)![Facundo Gauna's photo](https://gaunacode.com/_next/image?url=https%3A%2F%2Fcdn.hashnode.com%2Fres%2Fhashnode%2Fimage%2Fupload%2Fv1637693654535%2FClSq6umLB.jpeg%3Fw%3D200%26h%3D200%26fit%3Dcrop%26crop%3Dfaces%26auto%3Dcompress%2Cformat%26format%3Dwebp&w=640&q=75)](https://hashnode.com/@gaunacode)

[Facundo Gauna](https://hashnode.com/@gaunacode)

·[Apr 2, 2020](https://gaunacode.com/install-iis-on-azure-vm-using-terraform)·

2 min read

Creating a blank VM on Azure is easy, especially from the portal. Installing software and enabling features on each new VM can be time consuming, not to mention error-prone. Similar to [yesterday](https://gaunacode.com/provisioning-a-vm-with-an-azure-devops-deployment-group-agent-with-terraform), I will show how to install IIS on a Windows VM using Terraform.

In order to install IIS on a new Windows VM, we’ll use a simple powershell script. The command is:

COPY

```
Install-WindowsFeature -Name Web-Server -IncludeAllSubFeature -IncludeManagementTools
```

This Powershell command installs IIS, all it’s sub features, and IIS Management tools.

We can execute this script from an Azure VM as it’s being provisioned using the [virtual machine custom script extension](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/custom-script-windows). As the VM is being provisioned, this script will be run and the state of the VM won’t show as “running” until the custom script finishes.

To invoke this custom script with Terraform, it’s quite simple.

COPY

```
resource "azurerm_virtual_machine_extension" "vm_extension_install_iis" {
  name                       = "vm_extension_install_iis"
  virtual_machine_id         = azurerm_windows_virtual_machine.vm.id
  publisher                  = "Microsoft.Compute"
  type                       = "CustomScriptExtension"
  type_handler_version       = "1.8"
  auto_upgrade_minor_version = true

  settings = <<SETTINGS
    {
        "commandToExecute": "powershell -ExecutionPolicy Unrestricted Install-WindowsFeature -Name Web-Server -IncludeAllSubFeature -IncludeManagementTools"
    }
SETTINGS
}
```

The only weird syntax is the `settings` object. It’s raw json, so you can provide the same parameters as specified on the [custom script extension documentation](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/custom-script-windows#extension-schema).

You can also provide `protectedSettings`. This is for more sensitive items like the storage account key. Why? You could choose to store an custom script _file_ on a storage account. This would be especially useful if you want to do more than just enable IIS.

COPY

```
resource "azurerm_virtual_machine_extension" "vm_extension_install_iis" {
  name                       = "vm_extension_install_iis"
  virtual_machine_id         = azurerm_windows_virtual_machine.vm.id
  publisher                  = "Microsoft.Compute"
  type                       = "CustomScriptExtension"
  type_handler_version       = "1.8"
  auto_upgrade_minor_version = true

  settings = <<SETTINGS
    {
        "commandToExecute": "powershell -ExecutionPolicy Unrestricted Install-WindowsFeature -Name Web-Server -IncludeAllSubFeature -IncludeManagementTools"
    }
SETTINGS

protected_settings = <<PROTECTED_SETTINGS
    {
        "commandToExecute": "myExecutionCommand",
        "storageAccountName": "myStorageAccountName",
        "storageAccountKey": "myStorageAccountKey",
        "managedIdentity" : {}
    }
PROTECTED_SETTINGS
}
```

That’s it!

If you’re get errors with the custom script extension, there’s a set of [troubleshooting tips](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/custom-script-windows#troubleshoot-and-support) published by Microsoft.

[#azure](https://hashnode.com/n/azure)[#devops](https://hashnode.com/n/devops)

[![](data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjU2IiBoZWlnaHQ9IjI1NiIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=)![Facundo Gauna](https://gaunacode.com/_next/image?url=https%3A%2F%2Fcdn.hashnode.com%2Fres%2Fhashnode%2Fimage%2Fupload%2Fv1637693654535%2FClSq6umLB.jpeg%3Fw%3D256%26h%3D256%26fit%3Dcrop%26crop%3Dentropy%26auto%3Dcompress%2Cformat%26format%3Dwebp&w=640&q=75)](https://hashnode.com/@gaunacode)

### WRITTEN BY

# [Facundo Gauna](https://hashnode.com/@gaunacode)

Follow

I used to be a .NET developer. Nowaways, I am a DevOps solutions architect with a focus on Azure and Kubernetes.

I also love productivity topics, especially when it comes to doing more with less of my time. I'm also a daddy, so time is a limited resource for me.

Like

[](https://gaunacode.com/install-iis-on-azure-vm-using-terraform#write-comment)

[](https://twitter.com/share?url=https%3A%2F%2Fgaunacode.com%2Finstall-iis-on-azure-vm-using-terraform&text=Install%20IIS%20on%20Azure%20VM%20using%20Terraform%0D%0A%7B%20by%20Facundo%20Gauna%20%7D%20from%20%40hashnode%0D%0A%0A%23azure%20%23devops)

### MORE ARTICLES

[![](data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNzAiIGhlaWdodD0iNzAiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgdmVyc2lvbj0iMS4xIi8+)![Facundo Gauna's photo](https://gaunacode.com/_next/image?url=https%3A%2F%2Fcdn.hashnode.com%2Fres%2Fhashnode%2Fimage%2Fupload%2Fv1637693654535%2FClSq6umLB.jpeg%3Fw%3D70%26h%3D70%26fit%3Dcrop%26crop%3Dfaces%26auto%3Dcompress%2Cformat%26format%3Dwebp&w=256&q=75)](https://hashnode.com/@gaunacode)

[Facundo Gauna](https://hashnode.com/@gaunacode)

[![Post cover](https://gaunacode.com/_next/image?url=https%3A%2F%2Fcdn.hashnode.com%2Fres%2Fhashnode%2Fimage%2Funsplash%2FJrZ1yE1PjQ0%2Fupload%2Fv1643984704035%2FDOQ85n3iW.jpeg%3Fw%3D500%26h%3D262%26fit%3Dcrop%26crop%3Dentropy%26auto%3Dcompress%2Cformat%26format%3Dwebp&w=3840&q=75)](https://gaunacode.com/you-dont-need-secrets-to-deploy-to-azure-from-github-actions)

# [You don't need secrets to deploy to Azure from GitHub Actions 🤯](https://gaunacode.com/you-dont-need-secrets-to-deploy-to-azure-from-github-actions)

[I've been using GitHub a lot more lately for CI/CD. I have a background with Azure DevOps, so seeing…](https://gaunacode.com/you-dont-need-secrets-to-deploy-to-azure-from-github-actions)