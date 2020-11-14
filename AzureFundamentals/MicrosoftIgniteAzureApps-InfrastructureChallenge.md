## Manage access to an Azure subscription by using Azure role-based access control (RBAC)

### Introduction

You're a Global Administrator in Azure Active Directory (Azure AD) for a large organization. The only administrator for the marketing department's Azure subscription recently left the organization. You've been asked to grant administrator access for the subscription to someone else in the marketing department. That person needs to manage Azure resources that were created under that subscription. The person also needs access to the billing information for that subscription.

### Learning objectives

In this module, you will:

- Identify the appropriate role to assign to an employee.
- Identify scenarios where the Global Administrator for Azure Active Directory might need to temporarily elevate their access in Azure.
- Grant the employee administrator access to a subscription.

### Prerequisites

- A basic understanding of Azure role-based access control (RBAC)
- (Optional) Access to an Azure subscription where you have the Global Administrator role for your account

## Understand the difference between Azure RBAC roles and Azure AD roles

Azure resources and **Azure Active Directory** (Azure AD) have independent permission systems. **Azure role-based access control** (RBAC) roles are used to manage access to virtual machines, storage, and other Azure resources. Azure AD roles are used to manage access to Azure AD resources, such as user accounts and passwords.

In this unit, you'll look at the essential aspects of Azure RBAC roles and Azure AD roles. You'll explore the various scopes available to you. You'll then identify the right roles to assign, depending on the scenario.

### Azure RBAC roles

Azure RBAC is the system that allows control over who has access to which Azure resources, and what those people can do with those resources. You achieve control by assigning roles to users, groups, or applications at a particular scope. A role might be described as a collection of permissions.

With Azure RBAC, you can allow one user, or a set of users, to access resources in a subscription. You can also separate responsibility for certain resources according to the specializations within your team. For example, you might want to grant your organization's data scientists access to Azure Machine Learning and any associated resources, such as Azure SQL Database. Or you might want to grant access to Azure Blob storage within a dedicated Machine Learning resource group. By granting specific access, you isolate these resources from team members who don't have the required skills or resource needs.

You can specify fine-grained permissions for applications so that a marketing web app has access to only the associated marketing database and storage account. For managers or team members who are higher up in the organization, you could grant access to all resources in a resource group, a subscription for management purposes, and an overview of billing and consumption.

Azure RBAC has many built-in roles, and you can create custom roles.

Here are four examples of built-in roles:

- **Owner**: Has full access to all resources, including the ability to delegate access to other users.
- **Contributor**: Can create and manage Azure resources.
- **Reader**: Can view only existing Azure resources.
- **User Access Administrator**: Can manage access to Azure resources.

### Identify the right scope

You can apply Azure RBAC roles at four levels of scope: _management groups, subscriptions, resource groups, and resources_. The following diagram illustrates the hierarchy of these four levels.

Azure management groups help you manage your Azure subscriptions by grouping them together. If your organization has many subscriptions, you might need a way to efficiently manage access, policies, and compliance for those subscriptions. Azure management groups provide a level of scope above subscriptions.

Azure subscriptions help you organize access to Azure resources and determine how resource usage is reported, billed, and paid for. Each subscription can have a different billing and payment setup, so you can have different subscriptions and plans by office, department, project, and so on.

Resource groups are containers that hold related resources for an Azure solution. A resource group includes those resources that you want to manage as a group. You decide which resources belong in a resource group based on what makes the most sense for your organization.

The scope is important and establishes which resources should have a specific type of access applied to them. Imagine that someone in your organization needs access to virtual machines. You could use the Virtual Machine Contributor role, which allows that person to manage virtual machines within only a specific resource group. You can limit the role's scope to a specific resource, resource group, subscription, or management group level.

By combining an Azure RBAC role and a scope, you can set finely tailored permissions for your Azure resources.

### Azure AD roles

Azure AD also has its own set of roles, which apply mostly to users, passwords, and domains. These roles have different purposes. Here are a few examples:

- **Global Administrator**: Can manage access to administrative features in Azure AD. A person in this role can grant administrator roles to other users, and they can reset a password for any user or administrator. By default, whoever signs up for the directory is automatically assigned this role.

- **User Administrator**: Can manage all aspects of users and groups, including support tickets, monitoring service health, and resetting passwords for certain types of users.

- **Billing Administrator**: Can make purchases, manage subscriptions and support tickets, and monitor service health. Azure has detailed billing permissions in addition to RBAC permissions. The available billing permissions depend on the agreement you have with Microsoft.

### Differences between Azure RBAC roles and Azure AD roles

_The main difference between Azure RBAC roles and Azure AD roles is the areas they cover_. Azure RBAC roles apply to Azure resources, and Azure AD roles apply to Azure AD resources (particularly users, groups, and domains). Also, Azure AD has only one scope, the directory. The Azure RBAC scope covers management groups, subscriptions, resource groups, and resources.

The roles share a key area of overlap. An Azure AD Global Administrator can elevate their access to manage all Azure subscriptions and management groups. This greater access grants them the Azure RBAC User Access Administrator role for all subscriptions of their directory. Through the User Access Administrator role, the Global Administrator can give other users access to Azure resources.

In our scenario, you need to grant full Azure RBAC management and billing permissions to a new manager. To achieve this, you temporarily elevate your access to include the User Access Administrator role. You can then grant the new manager the Owner role so that they can create and manage resources. You also set the scope to the subscription level, so that the manager can do this for all resources in the subscription.

The following diagram shows what resources the Global Administrator can view when their permissions are elevated to User Access Administrator.

## Understand when you might need to elevate your access

The administrator for the marketing department's Azure subscription recently left the organization. As Global Administrator, you don't have access to this subscription. You now need to grant administrator access for the subscription to another person in the marketing department.

In this unit, you'll explore when you might need to elevate your own access.

### When to elevate(nâng) access

By default, a Global Administrator doesn't have access to Azure resources. The Global Administrator for Azure Active Directory (Azure AD) can temporarily elevate their permissions to the Azure role-based access control (RBAC) role of User Access Administrator. This action grants the Azure RBAC permissions that are needed to manage Azure resources. The User Access Administrator is assigned at the scope of root. The role can view all resources in, and assign access to, any subscription or management group in that Azure AD organization.

As Global Administrator, you might need to elevate your permissions to:

- Regain lost access to an Azure subscription or management group.
- Grant another user or yourself access to an Azure subscription or management group.
- View all Azure subscriptions or management groups in an organization.
- Grant an automation app access to all Azure subscriptions or management groups.

After the Global Administrator elevates their permissions to User Access Administrator, they can then grant other users the Azure RBAC permissions that they need to control and manage Azure resources. When the task is complete, the Global Administrator should revoke their own elevated permissions.

### Assign a user administrative access to an Azure subscription

To assign a user administrative access to a subscription, you must have `Microsoft.Authorization/roleAssignments/write and Microsoft.Authorization/roleAssignments/delete` permissions at the subscription scope. Users with the subscription Owner or User Access Administrator role have these permissions.

In the next unit, you'll see how to assign a role by using the Azure portal after you've elevated your permissions to User Access Administrator. But you can also assign roles by using Azure PowerShell, the Azure CLI, or the REST API.

In the following sections, let's briefly review the commands you would use to assign the Owner role in Azure PowerShell or the Azure CLI.

### Assign the role by using Azure PowerShell

The following command shows how to assign the Owner role to a user at the subscription scope by using Azure PowerShell:

```PowerShell
New-AzRoleAssignment `
    -SignInName rbacuser@example.com `
    -RoleDefinitionName "Owner" `
    -Scope "/subscriptions/<subscriptionID>"
```

### Assign the role by using the Azure CLI

The following command shows how to assign the Owner role to a user at the subscription scope by using the Azure CLI:

```Azure CLI
 az role assignment create \
    --assignee rbacuser@example.com \
    --role "Owner" \
    --subscription <subscription_name_or_id>
```

## Exercise - Get access to an Azure subscription

Your organization needs to grant administrator access for a subscription to a new administrator. The previous administrator left the company without assigning administrator access to another employee. No one else has access to this subscription.

In this unit, you'll temporarily elevate your own permissions to get access to this subscription. You'll look at how to assign subscription ownership to the new administrator. You'll then revoke your elevated access.

This exercise is optional. To complete it, you need access to an Azure subscription where you have the Global Administrator role for your account. If you don't have an Azure subscription, create a free account before you begin.

### Elevate your access

1. Sign in to the Azure portal as Azure Active Directory (Azure AD) Global Administrator.

2. Select Azure Active Directory > Properties.

3. Under Access management for Azure resources, select Yes.
4. Select Save.

5. Sign out of the Azure portal, and sign in again to refresh your access.

### Verify that you have the User Access Administrator role

1. At the top of the Azure portal, search for Subscriptions.

2. Select the relevant subscription. Now that you have elevated access at the root scope, you should see all subscriptions in your directory.

3. Select Access control (IAM) > Role assignments.

4. Under User Access Administrator, ensure that you have the Root (inherited) scope.

### Assign a user as an administrator of a subscription

Because you're using your own subscription, you might want to walk through the following procedure without saving the Owner role assignment in step 5.

1.At the top of the Access control (AIM) pane, select Add.

2. Select Add role assignment.

3. In the Role drop-down list, select Owner.

4. In the Select box, enter the username or email address of the user you want to grant access to. Select that user.

5. If you want to complete the Owner role assignment, select Save. Otherwise, select Discard.

### Summary

The large organization where you work needed to regain access to an Azure subscription after a previous manager left the company.

In this module, you've learned:

- The differences between Azure role-based access control (RBAC), for managing Azure resources, and Azure \* Active Directory (Azure AD), for managing Azure AD resources.
- How to temporarily elevate your permissions from the Global Administrator role to the Azure RBAC User \* Access Administrator role to regain access to Azure resources.
- How to grant a user administrator access to a subscription.

## Build Azure Resource Manager templates

### Introduction

Let's say you're a systems administrator at a growing financial services company. Analysts create financial models to help recommend the best investment options to investors.

These financial models are run on virtual machines running on Azure. A model can take between a few minutes and several hours to run. You typically create a new virtual machine to run each new model, then delete it after the analyst collects the results.

The financial services business moves quickly. You find yourself constantly deploying and deleting cloud resources. You initially created resources through the Azure portal, and now you use the Azure CLI and scripts to further automate things. Although you see certain patterns among your deployments, it still takes time to connect virtual machines to networking and storage components. At the end of each run, you need to delete only the components related to that run.

How can you keep up with the pace of your analysts? Azure Resource Manager templates are one way to further

Learning objectives
In this module, you will:

- Identify what Azure Resource Manager is, and what's in a Resource Manager template
- Deploy a VM using the Azure CLI and a prebuilt Resource Manager template
- Customize your Resource Manager template to configure a basic web server on your VM

### Prerequisites

Experience using Bash and the Azure CLI
Experience working with Azure resources and resource groups

### Define Azure Resource Manager templates

If you've been using Azure for a while, you've likely heard about Azure Resource Manager. Let's review Resource Manager's role and define what makes up a Resource Manager template.

#### What's Azure Resource Manager?

Azure Resource Manager is the interface for managing and organizing cloud resources. Think of Resource Manager as a way to deploy cloud resources.

If you're familiar with Azure resource groups, you know that they enable you to treat sets of related resources as a single unit. Resource Manager is what organizes the resource groups that let you deploy, manage, and delete all of the resources together in a single action.

Think about the financial models you run for your analysts. To run a model, you might need one or more VMs, a database to store data, and a virtual network to enable connectivity between everything. With Resource Manager, you deploy these assets into the same resource group and manage and monitor them together. When you're done, you can delete all of the resources in a resource group in one operation.

#### What are Resource Manager templates?

A Resource Manager template precisely defines all the Resource Manager resources in a deployment. You can deploy a Resource Manager template into a resource group as a single operation.
A Resource Manager template is a JSON file, making it a form of declarative automation. Declarative automation means that you define what resources you need but not how to create them. Put another way, you define what you need and it is Resource Manager's responsibility to ensure that resources are deployed correctly.

You can think of declarative automation similar to how web browsers display HTML files. The HTML file describes what elements appear on the page, but doesn't describe how to display them. The "how" is the web browser's responsibility.

> Note <br>
> You may hear others refer to Resource Manager templates as "ARM templates". We prefer the full names "Azure Resource Manager templates" or "Resource Manager templates".

#### Why use Resource Manager templates?

Using Resource Manager templates will make your deployments faster and more repeatable. For example, you no longer have to create a VM in the portal, wait for it to finish, then create the next VM, and so on. Resource Manager takes care of the entire deployment for you.

Here are some other benefits to consider.

- Templates improve consistency
  - Resource Manager templates provide a common language for you and others to describe your deployments. Regardless of the tool or SDK used to deploy the template, the structure, format, and expressions inside the template remain the same.
- Templates help express complex deployments
  - Templates enable you to deploy multiple resources in the correct order. For example, you wouldn't want to deploy a virtual machine before creating OS disk or network interface. Resource Manager maps out each resource and its dependent resources and creates dependent resources first. Dependency mapping helps ensure that the deployment is carried out in the correct order.
- Templates reduce manual, error-prone tasks
  - Manually creating and connecting resources can be time consuming, and it's easy to make mistakes along the way. Resource Manager ensures that the deployment happens the same way every time.
- Templates are code
  - Templates express your requirements through code. Think of a template as a type of infrastructure as code that can be shared, tested, and versioned like any other piece of software. Also, because templates are code, you can create a "paper trail" that you can follow. The template code documents the deployment. Most users maintain their templates under some kind of revision control, such as Git. When you change the template, its revision history also documents how the template (and your deployment) has evolved over time.
- Templates promote reuse
  - Your template can contain parameters that are filled in when the template runs. A parameter can define a username or password, a domain name, and so on. Template parameters enable you to create multiple versions of your infrastructure, such as staging and production, but still utilize the exact same template.
- Templates are linkable
  - Resource Manager templates can be linked together to make the templates themselves modular. You can write small templates that each define a piece of a solution and combine them to create a complete system.

The models your financial analysts run are unique, but you see patterns in the underlying infrastructure. For example, most models require a database to store data. Many models use the same programming languages, frameworks, and operating systems to carry out the details. You can define templates that describe each individual component (compute, storage, networking, and so on), and combine them to meet each analyst's specific needs.

#### What's in a Resource Manager template?

> Note <br>
> Here you'll see a few code examples to give you a sense of how each section is structured. Don't worry if what you see is unfamiliar to you — you'll be able to read others' templates and write your own as you gain hands-on experience with them.

You may have used JSON, or JavaScript Object Notation, to send data between servers and web applications. JSON is also a popular way to describe how applications and infrastructure are configured.

JSON allows us to express data stored as an object (such as a virtual machine) in text. A JSON document is essentially a collection of key-value pairs. Each key is a string; its value can be a string, a number, a Boolean expression, a list of values, or an object (which is a collection of other key-value pairs).

A Resource Manager template can contain the following sections. These sections are expressed using JSON notation, but are not related to the JSON language itself.

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "functions": [  ],
    "resources": [  ],
    "outputs": {  }
}
```

Let's look at each of these sections in a little more detail.

#### Parameters

This is where you specify which values are configurable when the template runs. For example, you might allow users of your template to specify a username, password, or domain name.

Here's an example that illustrates two parameters – one for a VM's username and one for its password.

```JSON
"parameters": {
  "adminUsername": {
    "type": "string",
    "metadata": {
      "description": "Username for the Virtual Machine."
    }
  },
  "adminPassword": {
    "type": "securestring",
    "metadata": {
      "description": "Password for the Virtual Machine."
    }
  }
}
```

#### Variables

This is where you define values that are used throughout the template. Variables can help make your templates easier to maintain. For example, you might define a storage account name one time as a variable and use that variable throughout the template. If the storage account name changes, you need to only update the variable.

Here's an example that illustrates a few variables that describe networking features for a VM.

```JSON
"variables": {
  "nicName": "myVMNic",
  "addressPrefix": "10.0.0.0/16",
  "subnetName": "Subnet",
  "subnetPrefix": "10.0.0.0/24",
  "publicIPAddressName": "myPublicIP",
  "virtualNetworkName": "MyVNET"
}
```

#### Functions

This is where you define procedures that you don't want to repeat throughout the template. Like variables, functions can help make your templates easier to maintain. Here's an example that creates a function to create a unique name that could be used when creating resources that have globally unique naming requirements.

```JSON
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

#### Resources

This section is where you define the Azure resources that make up your deployment.

Here's an example that creates a public IP address resource.

```JSON
"resources": [
{
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicIPAddressName')]",
  "location": "[parameters('location')]",
  "apiVersion": "2018-08-01",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsLabelPrefix')]"
    }
  }
}
],
```

Here, the type of resource is `Microsoft.Network/publicIPAddresses`. Its name is read from the variables section and its location, or Azure region, is read from the parameters section.

Because resource types can change over time, apiVersion refers to the version of the resource type you want to use. As resource types evolve and change, you can modify your templates to work with the latest features when you're ready.

#### Outputs

This is where you define any information you'd like to receive when the template runs. For example, you might want to receive your VM's IP address or FQDN – information you do not know until the deployment runs.

Here's an example that illustrates an output named "hostname". The FQDN value is read from the VM's public IP address settings.

```JSON
"outputs": {
  "hostname": {
    "type": "string",
    "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]"
  }
}
```

#### How do I write a Resource Manager template?

There are many approaches to writing Resource Manager templates. Although you can write a template from scratch, it's common to start with an existing template and modify it to suit your needs.

Here are a few ways you can get a starter template:

- Use the Azure portal to create a template based on the resources in an existing resource group.
- Start with a template you or your team built that serves a similar purpose.
- Start with an Azure Quickstart template. You'll see how in the next part.

No matter your approach, writing a template involves working with a text editor. You can bring your favorite editor, but Visual Studio Code's Azure Resource Manager Tools extension is specially designed for the task of creating templates. This extension makes it easier to navigate your template code and provides autocompletion for many common tasks.

As you explore and write your templates, you'll want to refer to the documentation to understand what resource types

### Discover Azure Quickstart templates

Recall that your analysts' financial models are run on Azure virtual machines. To further automate your deployments, you want to move from Azure CLI commands and scripts to Resource Manager templates.

Before you begin, you may wonder what existing templates are available to learn from and build upon.

Here you'll explore what an Azure Quickstart template is and what prebuilt templates are available for you to use right now.

#### What are Azure Quickstart templates

Azure Quickstart templates are Resource Manager templates that are provided by the Azure community. Quickstart templates are available on GitHub.

Many templates provide everything you need to deploy your solution. Others might serve as a starting point for your template. Either way, you can study these templates to learn how to best author and structure your own templates.

#### Discover what's on the Quickstart template gallery

Let's say you want to find a Resource Manager template that brings up a basic VM configuration – one that includes a VM, basic network settings, and storage.

1. Start by browsing to the Quickstart template gallery to see what's available.

   You see a number of popular and recently updated templates. These templates work with both Azure resources and popular software packages.

2. Let's say you come across the Deploy a simple Ubuntu Linux VM template.

   The name sounds like exactly what you need. But let's take a closer look to see what this template accomplishes.

   The Deploy to Azure button enables you to deploy the template directly through the Azure portal. But you won't do that here. Rather, you'll use the Azure CLI to deploy the template from Cloud Shell.

3. Click Browse on GitHub to navigate to the template's source code on GitHub.
   The Deploy to Azure button enables you to deploy the template directly through the Azure portal, just like you saw on the gallery page.
4. Click Visualize to navigate to the Azure Resource Manager Visualizer.
   You see the resources that make up the deployment, including a VM, a storage account, and network resources.

   You can use your mouse to arrange the resources. You can also use your mouse's scroll wheel to zoom in an out.

5. Click on the Virtual Machine resource labeled MyUbuntuVM.
   You see the source code that defines the VM resource.

   You'll have more time to inspect the source code in just a bit. But for now, take a moment to review it briefly.

You see that:

- The resource's type is Microsoft.Compute/virtualMachines.
- It's location, or Azure region, comes from the template parameter named location.
- The VM's size comes from the template variable vmSize.
- The computer name is read from a template variable and the username and password for the VM are read from template parameters.

In practice, you might review the README.md file on GitHub and further inspect the source code to see whether this template suits your needs.

But for now, this template looks promising. In the next part, you'll go ahead and deploy this template.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "defaultValue": "simpleLinuxVM",
      "metadata": {
        "description": "The name of you Virtual Machine."
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "authenticationType": {
      "type": "string",
      "defaultValue": "password",
      "allowedValues": [
        "sshPublicKey",
        "password"
      ],
      "metadata": {
        "description": "Type of authentication to use on the Virtual Machine. SSH key is recommended."
      }
    },
    "adminPasswordOrKey": {
      "type": "securestring",
      "metadata": {
        "description": "SSH Key or password for the Virtual Machine. SSH key is recommended."
      }
    },
    "dnsLabelPrefix": {
      "type": "string",
      "defaultValue": "[toLower(concat('simplelinuxvm-', uniqueString(resourceGroup().id)))]",
      "metadata": {
        "description": "Unique DNS Name for the Public IP used to access the Virtual Machine."
      }
    },
    "ubuntuOSVersion": {
      "type": "string",
      "defaultValue": "18.04-LTS",
      "allowedValues": [
        "12.04.5-LTS",
        "14.04.5-LTS",
        "16.04.0-LTS",
        "18.04-LTS"
      ],
      "metadata": {
        "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version."
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    },
    "VmSize": {
      "type": "string",
      "defaultValue": "Standard_B2s",
      "metadata": {
        "description": "The size of the VM"
      }
    },
    "virtualNetworkName": {
      "type": "string",
      "defaultValue": "vNet",
      "metadata": {
        "description": "Name of the VNET"
      }
    },
    "subnetName": {
      "type": "string",
      "defaultValue": "Subnet",
      "metadata": {
        "description": "Name of the subnet in the virtual network"
      }
    },
    "networkSecurityGroupName": {
      "type": "string",
      "defaultValue": "SecGroupNet",
      "metadata": {
        "description": "Name of the Network Security Group"
      }
    }
  },
  "variables": {
    "publicIpAddressName": "[concat(parameters('vmName'), 'PublicIP' )]",
    "networkInterfaceName": "[concat(parameters('vmName'),'NetInt')]",
    "subnetRef": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworkName'), parameters('subnetName'))]",
    "osDiskType": "Standard_LRS",
    "subnetAddressPrefix": "10.1.0.0/24",
    "addressPrefix": "10.1.0.0/16",
    "linuxConfiguration": {
      "disablePasswordAuthentication": true,
      "ssh": {
        "publicKeys": [
          {
            "path": "[concat('/home/', parameters('adminUsername'), '/.ssh/authorized_keys')]",
            "keyData": "[parameters('adminPasswordOrKey')]"
          }
        ]
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Network/networkInterfaces",
      "apiVersion": "2020-06-01",
      "name": "[variables('networkInterfaceName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Network/networkSecurityGroups/', parameters('networkSecurityGroupName'))]",
        "[resourceId('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]",
        "[resourceId('Microsoft.Network/publicIpAddresses/', variables('publicIpAddressName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "subnet": {
                "id": "[variables('subnetRef')]"
              },
              "privateIPAllocationMethod": "Dynamic",
              "publicIpAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              }
            }
          }
        ],
        "networkSecurityGroup": {
          "id": "[resourceId('Microsoft.Network/networkSecurityGroups',parameters('networkSecurityGroupName'))]"
        }
      }
    },
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "apiVersion": "2020-06-01",
      "name": "[parameters('networkSecurityGroupName')]",
      "location": "[parameters('location')]",
      "properties": {
        "securityRules": [
          {
            "name": "SSH",
            "properties": {
              "priority": 1000,
              "protocol": "TCP",
              "access": "Allow",
              "direction": "Inbound",
              "sourceAddressPrefix": "*",
              "sourcePortRange": "*",
              "destinationAddressPrefix": "*",
              "destinationPortRange": "22"
            }
          }
        ]
      }
    },
    {
      "type": "Microsoft.Network/virtualNetworks",
      "apiVersion": "2020-06-01",
      "name": "[parameters('virtualNetworkName')]",
      "location": "[parameters('location')]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[parameters('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetAddressPrefix')]",
              "privateEndpointNetworkPolicies": "Enabled",
              "privateLinkServiceNetworkPolicies": "Enabled"
            }
          }
        ]
      }
    },
    {
      "type": "Microsoft.Network/publicIpAddresses",
      "apiVersion": "2020-06-01",
      "name": "[variables('publicIpAddressName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Basic",
        "tier": "Regional"
      },
      "properties": {
        "publicIpAllocationMethod": "Dynamic",
        "publicIPAddressVersion": "IPv4",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        },
        "idleTimeoutInMinutes": 4
      }
    },
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2020-06-01",
      "name": "[parameters('vmName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('networkInterfaceName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[parameters('VmSize')]"
        },
        "storageProfile": {
          "osDisk": {
            "createOption": "fromImage",
            "managedDisk": {
              "storageAccountType": "[variables('osDiskType')]"
            }
          },
          "imageReference": {
            "publisher": "Canonical",
            "offer": "UbuntuServer",
            "sku": "[parameters('ubuntuOSVersion')]",
            "version": "latest"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('networkInterfaceName'))]"
            }
          ]
        },
        "osProfile": {
          "computerName": "[parameters('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPasswordOrKey')]",
          "linuxConfiguration": "[if(equals(parameters('authenticationType'), 'password'), json('null'), variables('linuxConfiguration'))]"
        }
      }
    }
  ],
  "outputs": {
    "adminUsername": {
      "type": "string",
      "value": "[parameters('adminUsername')]"
    },
    "hostname": {
      "type": "string",
      "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]"
    },
    "sshCommand": {
      "type": "string",
      "value": "[concat('ssh ', parameters('adminUsername'), '@', reference(variables('publicIPAddressName')).dnsSettings.fqdn)]"
    }
  }
}
```

### Exercise - Deploy a VM using an Azure Quickstart template

Here you'll deploy the Quickstart template you explored in the previous part. The Quickstart template brings up a basic virtual machine configuration.

> Important <br>
> You need your own Azure subscription to run this exercise and you may incur charges. If you don't already have an Azure subscription, create a free account before you begin.

#### How do I deploy a Resource Manager template?

You can use automation scripting tools such as the Azure CLI, Azure PowerShell, or even the Azure REST APIs with your favorite programming language to deploy resources from templates. You can also deploy your templates through Visual Studio, Visual Studio Code, and the Azure portal. Shortly, you'll deploy a Resource Manager template using the Azure CLI from Cloud Shell.

#### Verifying a template

The next step might be to visualize your template. Azure Resource Manager Visualizer enables you to upload your Resource Manager template and graphically see how your resources relate to one another. You can inspect this visualization to verify that the deployment is set up correctly and meets your requirements.

Finally, you can perform a test deployment from the Azure CLI or Azure PowerShell. A test deployment doesn't create any resources, but it provides you with feedback on what would happen when the deployment runs. You'll perform a test deployment shortly to see the process in action.

#### Create a resource group

First, we'll create a resource group to hold all the things that we need to create. This allows us to administer all the VMs, disks, network interfaces, and other elements that make up our solution as a unit. We can use the Azure CLI to create a resource group with the az group create command. It takes a --name to give it a unique name in our subscription, and a --location to tell Azure what area of the world we want the resources to be located by default.

1. Sign in to the Azure portal.

2. From the menu bar on the top right-hand side, open Cloud Shell.

3. Set the resource group name.

```Bash
RESOURCEGROUP=learn-quickstart-vm-rg
```

4. Set the location. Replace the eastus value with a location near you.

The following list has some location values you can use.

```Bash
LOCATION=eastus
```

The following list has some location values you can use.

- westus2
- southcentralus
- centralus
- eastus
- westeurope
- southeastasia
- japaneast
- brazilsouth
- australiasoutheast
- centralindia

5. Run the following command to create a resource group.

```Bash
az group create --name $RESOURCEGROUP --location $LOCATION
```

#### Create template parameters

Recall that a Resource Manager template is divided into sections, one of them being Parameters.

Near the start of the template, you see a section named parameters. This section defines these parameters:

- _adminUsername_
- _authenticationType_
- _adminPasswordOrKey_
- _dnsLabelPrefix_
- _ubuntuOSVersion_
- _location_

This image highlights the first few parameters.

The _authenticationType_ parameter specifies whether to use password authentication or key-based authentication to connect to the virtual machine. The _adminPasswordOrKey_ parameter specifies the password or SSH key. Although key-based authentication is typically more secure than password authentication, here you'll use password authentication for learning purposes.

Some parameters, such as _ubuntuOSVersion_ and _location_, have default values. The default value for ubuntuOSVersion is "18.04-LTS" and the default value for location is the parent resource group's location.

Let's keep these parameters at their default values. For the remaining parameters, you have two options:

1. Provide the values in a JSON file.
2. Provide the values as command-line arguments.

For learning purposes, here you'll provide the values as command-line arguments. To make the template easy to deploy, you'll start by storing these values as Bash variables.

1. Sign in to the Azure portal.

2. From the menu bar on the top right-hand side, open Cloud Shell.

3. From Cloud Shell, create a username. For this example, let's use azureuse

```BASH
USERNAME=azureuser
```

4. Run the openssl utility to generate a random password.

```Bash
PASSWORD=$(openssl rand -base64 32)
```

There are many ways generate random passwords. The method you choose depends on your workflow and requirements. This method uses the openssl utility to generate 32 random bytes and base64 encode the output. Base64 encoding ensures that the result contains only printable characters.

5. Generate a unique DNS label prefix.

```Bash
DNS_LABEL_PREFIX=mydeployment-$RANDOM
```

The DNS label prefix must be unique. The DNS label prefix begins with "mydeployment" followed by a random number. \$RANDOM is a Bash function that generates a random positive whole number.

In practice, you would choose a DNS label prefix that fits your requirements.

#### Validate and launch the template

With your parameters in place, you have everything you need to launch the template.

As a final verification step, you'll begin by validating that the template is syntactically correct.

1. From Cloud Shell, run az deployment group validate to validate the template.

```Azure CLI
az deployment group validate \
  --resource-group $RESOURCEGROUP \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json" \
  --parameters adminUsername=$USERNAME \
  --parameters authenticationType=password \
  --parameters adminPasswordOrKey=$PASSWORD \
  --parameters dnsLabelPrefix=$DNS_LABEL_PREFIX
```

The **--template-uri** argument points to the template on GitHub. The template's filename is azuredeploy.json. Later, you'll see how to validate and run a template on your local filesystem.

You see a large JSON block as output, which tells you that the template passed validation.

Azure Resource Manager fills in the template parameters and checks whether the template would successfully run in your subscription.

If validation failed, you would see a detailed description of the failure in the output.

2. Run az deployment group create to deploy the template.

```
az deployment group create \
  --name MyDeployment \
  --resource-group $RESOURCEGROUP \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json" \
  --parameters adminUsername=$USERNAME \
  --parameters authenticationType=password \
  --parameters adminPasswordOrKey=$PASSWORD \
  --parameters dnsLabelPrefix=$DNS_LABEL_PREFIX
```

This command resembles the previous command, but also includes the --name argument to give your deployment a name.

This command takes 2-3 minutes to complete. While you wait, now's a great time to take a closer look at the source code for this template. Remember, the contents of a Resource Manager template will become more familiar to you as you read existing templates and create your own.

When the deployment completes, you see another large JSON block as output that describes the deployment.

#### Verify the deployment

The deployment succeeded. But let's run a few commands just to verify.

1, Run az deployment group show to verify the deployment.

```Azure CLI
az deployment group show \
  --name MyDeployment \
  --resource-group $RESOURCEGROUP
```

You see the same JSON block as you did previously. You can run this command later if you ever need these details about the deployment. The output is structured as JSON to make it easier to feed into other tools you might use to track your deployments and cloud usage.

2. Run az vm list to see which VMs are running.

```
az vm list \
  --resource-group $RESOURCEGROUP \
  --output table
```

Your output resembles this. Your region is shown under the Location column.

```Bash
Name        ResourceGroup                         Location        Zones
----------  ------------------------------------  --------------  -------
simpleLinuxVM  learn-quickstart-vm-rg    southcentralus
```

Recall that the template names the VM "simpleLinuxVM". Here you see that this VM exists in your resource group.

### Add a resource to the template

When you create a VM for your financial analysts, you typically include a web server that provides a dashboard for the analyst to visualize and collect the results of the run. Using a Resource Manager template to bring up a VM is a good start. But how can you extend the template to configure web server software inside the VM?

Using the Azure CLI to configure a VM to run a web server requires only a few basic commands. But what happens when:

- You need to create and delete VMs continuously?
- Your deployments require additional components such as storage and networking, increasing their complexity?
  Managing individual resources through the Azure CLI is a good start. But it quickly becomes tedious and error prone because you still need to correctly connect resources together. You need a more automated solution.

Here you'll examine the requirements for your VM's web server and learn how to build resources you can add to your templates.

#### What's the Custom Script Extension?

The Custom Script Extension is an easy way to download and run scripts on your Azure VMs. It's just one of the many ways you can configure a VM once it's up and running.

You can store your scripts in Azure storage or in a public location such as GitHub. You can run scripts manually or as part of a more automated deployment. Here, you'll define a resource that you'll soon add to your Resource Manager template. The resource will use the Custom Script Extension to download a Bash script from GitHub and execute that script on your VM. The script installs and configures the Nginx web server software package and configures a basic home page.

#### How do I extend a Resource Manager template?

Your Resource Manager template creates an Ubuntu Linux VM. There are a few ways to extend the template to also install Nginx when the VM starts.

One way to extend your template is to create multiple templates, each defining one piece of the system. You then link or nest them together to build a more complete system. As you create your own templates, you can build a library of smaller, more granular templates and combine them how you need.

Another way is to modify an existing template to suit your needs. You'll do that in this module because that's often the fastest way to get started writing your own templates.

#### Build the template resource

Here you'll learn how to build template resources, using the Custom Script Extension as an example. You'll use the reference documentation as a starting point and then define the resource properties you need.

Let's start by reviewing your requirements.

#### Gather requirements

Here's a brief summary of what you want to accomplish through your template.

1. Create a VM.
2. Open port 80 through the network firewall.
3. Install and configure web server software on your VM.

The Resource Manager template you used in the previous part already covers the first two requirements. For the third, let's start by taking a look at an Azure CLI command we could run manually from the command line to install and configure Nginx on your VM using the Custom Script Extension.

```Azure CLI
az vm extension set \
  --resource-group $RESOURCEGROUP \
  --vm-name simpleLinuxVM \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
  --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
```

This command uses the Custom Script Extension to run a Bash script on your VM that installs Nginx. You can examine the Bash script from a separate browser tab if you'd like.

You need a way to map this command to use the Resource Manager template syntax. The az vm extension set example shown above is a command you'd enter on the command line. What you need is the declarative JSON in template format.

#### Locate the resource schema

To discover how to use the Custom Script Extension in your template, one approach is to learn by example. For example, you might find an Azure Quickstart template that does something similar and adapt it to your needs.

Another approach is to look up the resource you need in the reference documentation . In this case, searching the documentation would reveal Microsoft.Compute virtualMachines/extensions template reference .

You can start by copying the schema to a temporary document, like this.

```JSON
{
  "name": "string",
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "apiVersion": "2018-10-01",
  "location": "string",
  "tags": {},
  "properties": {
    "publisher": "string",
    "type": "string",
    "typeHandlerVersion": "string",
    "autoUpgradeMinorVersion": boolean,
    "settings": {},
    "protectedSettings": {},
    "instanceView": {
      "name": "string",
      "type": "string",
      "typeHandlerVersion": "string",
      "substatuses": [
        {
          "code": "string",
          "level": "string",
          "displayStatus": "string",
          "message": "string",
          "time": "string"
        }
      ],
      "statuses": [
        {
          "code": "string",
          "level": "string",
          "displayStatus": "string",
          "message": "string",
          "time": "string"
        }
      ]
    }
  }
}
```

#### Specify required properties

The schema shows all of the properties you can provide. Some properties are required; others are optional. You might start by identifying all of the required properties. Locate these below the schema definition on the reference page.

Here are the required parameters.

- `name`
- `type`
- `piVersion`
- `location`
- `properties`

After you remove all the parameters that aren't required, your resource definition might look like this.

```
{
  "name": "[concat(parameters('vmName'), '/', 'ConfigureNginx')]",
  "type": "Microsoft.Azure.Extensions/virtualMachines/extensions",
  "apiVersion": "2018-06-01",
  "location": "[parameters('location')]",
  "properties": { }
}
```

The values for the `type` and `apiVersion` properties come directly from the documentation. properties is required but for now can be empty.

You know that your existing VM template has a parameter named location. This example uses the built-in parameters function to read that value.

The `name` property follows a special convention. This example uses the built-in concat function to concatenate, or combine, multiple strings. The complete string contains the VM name followed by a slash / character followed by a name you choose (here, it's "ConfigureNginx"). The VM name helps the template identify which VM resource to run the script on.

#### Specify additional properties

Next, you might add in any additional properties that you need. Referring back to the CLI example earlier, you need the location, or URI, of the script file and the Bash command to execute on the VM to run that script. As a recommended practice, you might also include the resource's publisher name, its type, and version.

Referring back to the documentation, you need these values, shown here using "dot" notation.

- `properties.publisher`
- `properties.type`
- `properties.typeHandlerVersion`
- `properties.autoUpgradeMinorVersion`
- `properties.settings.fileUris`
- `properties.protectedSettings.commandToExecute`

Your Custom Script Extension resource now looks like this.

```JSON
{
  "name": "[concat(parameters('vmName'), '/', 'ConfigureNginx')]",
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "apiVersion": "2018-06-01",
  "location": "[parameters('location')]",
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "customScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "./configure-nginx.sh"
    }
  }
}
```

#### Specify dependent resources

You can't run the Custom Script Extension until the VM is available. All template resources provide a `dependsOn` property. This property helps Resource Manager determine the correct order to apply resources.

Here's what your template resource might look like after you add the `dependsOn` property.

```JSON
{
  "name": "[concat(parameters('vmName'), '/', 'ConfigureNginx')]",
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "apiVersion": "2018-06-01",
  "location": "[parameters('location')]",
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "customScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "./configure-nginx.sh"
    }
  },
  "dependsOn": [
    "[resourceId('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
  ]
}
```

The bracket `[ ]` syntax means that you can provide an array, or list, of resources that must exist before applying this resource.

There are multiple ways to define a resource dependency. You can provide its name, such as "simpleLinuxVM", its full name (including its namespace, type, and name), such as "Microsoft.Compute/virtualMachines/simpleLinuxVM", or by its resource ID.

This example uses the built-in resourceId function to get the VM's resource ID using its full name. This helps clarify which resource you're referring to and can help avoid ambiguity when more than one resource has a similar name.

The existing template provides a vmName parameter that defines the VM's name. This example uses the built-in parameters function to read it.

#### Summary

You now have everything you need to define the Custom Script Extension resource in your template. In the next part, you'll extend your template to use it.

### Exercise - Extend the Quickstart template to deploy a basic web site

Now that you've defined the template resource for the Custom Script Extension that configures Nginx on your VM, let's add it to the existing VM template and run it.

Here's what the Nginx configuration will look like.

#### Build the template

1. Here you'll download the template and modify it.

From Cloud Shell, run curl to download the template you used previously from GitHub.

```Bash
curl https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json > azuredeploy.json
```

2. Open azuredeploy.json through the Cloud Shell editor.

```Bash
code azuredeploy.json
```

3. In the file, locate the resources section. Add the Custom Script Extension resource you built in the previous part to the top of this section, then save the file.

Here's the code as a refresher.

```JSON
{
  "name": "[concat(parameters('vmName'),'/', 'ConfigureNginx')]",
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "apiVersion": "2018-06-01",
  "location": "[parameters('location')]",
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "customScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"
      ]
    },
    "protectedSettings": {
       "commandToExecute": "./configure-nginx.sh"
    }
  },
  "dependsOn": [
    "[resourceId('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
  ]
},
```

Note the comma `,` character at the end, which is needed to separate resources. The order you define resources doesn't matter, but here you add it to the top for simplicity.

4. In the file, under resources locate the "Microsoft.Network/networkSecurityGroups" section. Under "securityRules, add a new rule to open port 80. Use the following rule:

```JSON
{
   "name":"allow_80",
   "properties": {
   "priority": 101,
   "protocol": "TCP",
   "access": "Allow",
   "direction": "Inbound",
   "sourceAddressPrefix": "Internet",
   "sourcePortRange": "*",
   "destinationAddressPrefix": "*",
   "destinationPortRange": "80"
   }
 },
```

5. If you get stuck or want to compare your work, you can download the resulting file from GitHub.

```Bash
curl https://raw.githubusercontent.com/MicrosoftDocs/mslearn-build-azure-vm-templates/master/linux/azuredeploy.json > azuredeploy.json
```

6. You're all done editing files. Select the ellipses in the corner and Save.

7. To close the editor, click the ellipses in the corner and then select Close Editor.

#### Verify the template

Here you'll validate the template from the CLI.

In practice, you might run lint tests or run your template through the Azure Resource Manager Visualizer before you run a test deployment.

Similar to what you did previously, run `az deployment group validate` to validate your template.

```Azure CLIU
az deployment group validate \
  --resource-group $RESOURCEGROUP \
  --template-file azuredeploy.json \
  --parameters adminUsername=$USERNAME \
  --parameters authenticationType=password \
  --parameters adminPasswordOrKey=$PASSWORD \
  --parameters dnsLabelPrefix=$DNS_LABEL_PREFIX
```

Notice that this time you use the --template-file argument, and not `--template-uri`, because you are referencing a local file.

If you see errors, refer back to the previous part or compare your code to the reference implementation .

#### Deploy the template

Here you'll run a command that's similar to the one you ran earlier to deploy the template. Because you haven't modified any existing resources – the VM, its network settings, or the storage account – Resource Manager won't take any action on those resources. It will only apply the resource you just added that runs the Custom Script Extension that installs Nginx on your VM.

Run `az deployment group create` to update your deployment.

```Azure CLI
az deployment group create \
  --name MyDeployment \
  --resource-group $RESOURCEGROUP \
  --template-file azuredeploy.json \
  --parameters adminUsername=$USERNAME \
  --parameters authenticationType=password \
  --parameters adminPasswordOrKey=$PASSWORD \
  --parameters dnsLabelPrefix=$DNS_LABEL_PREFIX
```

Again, notice that this time you use the `--template-file` argument because you are referencing a local file.

The command takes a couple minutes to complete. You see a large block of JSON as output, which describes the deployment.

#### Verify the deployment

The deployment succeeded, so let's see the resulting configuration in action.

1. Run `az vm` show to get the VM's IP address and store the result in a Bash variable.

```Azure CLI
IPADDRESS=$(az vm show \
  --name simpleLinuxVM \
  --resource-group $RESOURCEGROUP \
  --show-details \
  --query [publicIps] \
  --output tsv)
```

2. Run `curl` to access your web server.

```Bash
curl $IPADDRESS
```

You see this.

```HTML
<html><body><h2>Welcome to Azure! My name is simpleLinuxVM.</h2></body></html>
```

3. From a separate browser tab, navigate to your web site.

First, print the IP address

```BASH
echo $IPADDRESS
```

4. Navigate to the IP address you see from a separate browser tab. You see something like the following message:

Great work! With the Custom Script Extension resource in place, you're able to extend your deployment to do more.

#### Summary

In this module, you defined what Azure Resource Manager means and what's in a Resource Manager template.

Resource Manager templates are `declarative`, meaning that you define what resources you need and let Resource Manager handle the deployment details.

You used an existing Azure Quickstart template to deploy a VM. Quickstart templates are a great way to jump-start your deployments. They also demonstrate recommended practices you can learn from as you author your own templates.

You also extended your Quickstart template to configure web server software on your VM. Extending an existing template is a great way to get something running quickly and understand how the pieces fit together.

Resource Manager templates are also composable. As you build out your deployments, you can write smaller templates that each define a piece of the system and then combine them to create a complete system.

Think about the analysts you support at your financial services company. As you build your library of Resource Manager templates, you'll be able to assemble deployments for each analyst much more quickly. After each financial model completes, you need to run only a single Azure CLI command to tear down the deployment.

## Build a scalable application with virtual machine scale sets

### Introduction

Managing virtual machines at scale can be challenging, especially when usage patterns vary and demands on applications fluctuate(dao động). You want to be able to adjust your virtual machine resources to match demands. At the same time, you want to keep the virtual machine configuration consistent to ensure application stability. Achieving these goals means you maintain throughput and responsiveness while minimizing the costs of continually running a large collection of virtual machines.

Imagine that you work for a domestic shipping company. Your customers use one of the company's websites to manage and check the status of their shipments. This website is deployed to virtual machines and hosted on-premises. You've noticed that increased usage on the site is straining the virtual machines' resources. However, you can't adjust to load fluctuations without manually intervening and creating or deallocating virtual machines.

You decide to move the application to Azure. You need a solution that automatically handles load fluctuations and ensures consistent performance of the website. You also need to quickly roll out application updates to the servers while minimizing the effect on end users.

In this module, you'll learn how a virtual machine scale set helps address these challenges of load balancing a web application.

#### Learning objectives

In this module, you'll:

- Identify the features and capabilities of virtual machine scale sets.
- Identify the use cases for running applications on virtual machine scale sets.
- Deploy an application on a virtual machine scale set.

#### Prerequisites

- Basic knowledge of Azure virtual machines
- Basic knowledge of load-balancing concepts

### Features and benefits of virtual machine scale sets

Azure virtual machine scale sets provide a scalable way to run applications on a set of virtual machines (VMs). The VMs in this type of scale set all have the same configuration and run the same applications. As demand grows, the number of VMs running in the scale set increases. As demand slackens, excess VMs can be shut down. Virtual machine scale sets are ideal for scenarios that include compute workloads, big-data workloads, and container workloads.

In our example scenario, your customers use one of the company's websites to manage and check the status of their shipments. Because the website is accessed globally, the load is sometimes difficult to predict at any particular time of day. Additionally, loading might vary seasonally, with December being busy because of the holidays at the end of the year. You decide to use a virtual machine scale set to handle the fluctuating load while maintaining a low response time for customer requests.

In this unit, you'll explore the features of virtual machine scale sets. By the end of this unit, you'll be able to describe how a scale set works. You'll understand how a scale set supports scale-out and scale-up scenarios. You'll see how to use autoscaling and schedule-based scaling to adjust the resources available to a scale set.

#### What is a virtual machine scale set?

Virtual machine scale sets in Azure are designed to allow you to deploy and manage many load-balanced, identical VMs. These machines run with the same configurations. Virtual machine scale sets are intelligent enough to automatically scale up or down the number of VM instances. A scale set can also change the size of VM instances.

The criteria used to activate the upscale or downscale can depend on a customized schedule or actual demand and usage. Scale sets apply the same configuration to a group of VMs simultaneously. They don't require you to manually configure instances individually.

A scale set uses a load balancer to distribute requests across the VM instances. It uses a health probe to determine the availability of each instance. The health probe pings the instance. If the instance responds, the scale set knows the instance is still available. If the ping fails or times out, the scale set knows the instance is unavailable and doesn't send requests to it.

Virtual machine scale sets support both Linux and Windows VMs in Azure. However, keep in mind that you're limited to running 1,000 VMs on a single scale set.

If you deal with large workloads whose demand varies and is unpredictable, scale sets are a great solution. Because virtual machine scale sets offer identical VMs scaled and load-balanced in response to demand, they automatically provide a highly available environment.

#### Scaling options for scale sets

Scale sets are designed for cost-effectiveness. New VM instances are created only when needed. A scale set can scale VMs either horizontally or vertically.

#### What is horizontal scaling?

**Horizontal scaling** is the process of adding or removing several VMs in a scale set.

Sometimes you might need to add or remove machines in a scale set, depending on demand. For example, you might not need to run some machines during periods of the week or day when demand is low. You could manually adjust the number of VMs in a scale set by increasing or decreasing the instance count. But in many cases, it's better to automatically add or remove VMs by using rules. The rules are based on metrics. They ensure that the right number of VMs are added, depending on the demand or schedule.

#### What is vertical scaling?

**Vertical scaling** is the process of adding resources such as memory, CPU power, or disk space to VMs.

In contrast to horizontal scaling, where new, identically sized VMs are added to or removed from a scale set, vertical scaling focuses on increasing the size of the VMs in the scale set.

For example, you might want to reduce the CPU performance of a group of VMs in a scale set. In this case, you might not necessarily need to remove an entire group of machines. In scale sets, you create rules based on metrics. These rules automatically trigger an increase in the sizes of the VMs.

Vertical scaling typically requires rebooting the affected VMs in the scale set. This process can lead to temporary degraded performance across the scale set while the VMs restart.

#### Scaling a scale set

Virtual machine scale sets address the need to quickly create and manage VMs for a fluctuating workload. You can configure two types of scaling for a scale set:

- **Scheduled scaling**: You can proactively schedule the scale set to deploy one or N number of additional instances to accommodate a spike in traffic and then scale back down when the spike ends.

- **Autoscaling**: If the workload is variable and can't always be scheduled, you can use metric-based threshold scaling. Autoscaling horizontally scales out based on node usage. It then scales back in when the resources return to a baseline

Both of these options address the requirement to scale while managing associated costs. The following examples describe scenarios where you might use different types of scaling.

#### Scheduled scaling

Suppose you're part of the DevOps team for a large food delivery company. Friday night is typically your busiest time. Conversely, 7 AM on Wednesday is generally your quietest time.

Azure charges based on the consumption of resources, so don't run services you don't need. If you need 100 web servers to meet your demand on a Friday night, you're happy to pay for them. But if you need only two servers on a Wednesday morning, you don't want to pay for the 98 idle servers. To manage your costs while fulfilling operational requirements, consider using scheduled scaling.

#### Autoscaling

Suppose you're on the DevOps team for a popular footwear company. As a product launch approaches, you think you'll see significant demand for your service. However, the demand spike might be unpredictable and hard to quantify. You want your service to meet demand by scaling horizontally as current resources are used.

For this scenario, use metrics-based autoscaling. This type of autoscaling scales out your infrastructure as demand rises. It scales back in when demand declines.

#### Reducing costs by using low-priority scale sets

A low-priority virtual machine scale set allows you to use Azure compute resources at cost savings of up to 80 percent. The global Azure infrastructure frequently has underused compute resources available. A low-priority scale set provisions VMs through this underused compute capability.

When you use these VMs, keep in mind that they're temporary. Availability depends on size, region, time of day, and so on. These VMs have no SLA.

When Azure needs the computing power again, you'll receive a notification about the VM that will be removed from your scale set. If you need to clean up or gracefully exit code on your VM, you can use Azure Scheduled Events to react to the notification within the VM. You can also make the scale set try to create another VM to replace the one that's being removed. The creation of the new VM is, however, not guaranteed.

In a low-priority scale set, you specify two kinds of removal:

- Delete: The entire VM is removed, including all of the underlying disks.
- Deallocate: The VM is stopped. The processing and memory resources are deallocated. Disks are left intact and data is kept. You're charged for the disk space while the VM isn't running.

Low-priority scale sets are useful for workloads that run with interruptions or when you need larger VMs at a much-reduced cost. Just keep in mind that you can't control when a VM might be removed.

### Exercise - Deploy a scale set in the Azure portal

n the example scenario, you've decided to use a scale set to run the web application for the shipping company. Using a scale set, the shipping company can maintain short response times for users as the workload varies.

Your first task is to create a scale set. You'll configure it to run a web server, in this case nginx. When you've configured the scale set correctly, you'll deploy your web application. Then you'll set up a health probe that Azure will use to verify the availability of each VM in the scale set. Finally, you'll test the scale set by sending requests from a web browser.

> Note <br>
> This exercise is optional. If you don't have an Azure account, you can read through the instructions so you understand how to use the REST API to retrieve metrics.<br>
> If you want to complete this exercise but you don't have an Azure subscription or prefer not to use your own account, create a free account before you begin.

#### Deploy a virtual machine scale set

1. Sign in to the Azure portal and open Azure Cloud Shell.

2. In Cloud Shell, start the code editor and create a file named _cloud-init.yaml_.

```Bash
code cloud-init.yaml
```

3. Add the following text to the file:

```YAML
#cloud-config
package_upgrade: true
packages:
  - nginx
write_files:
  - owner: www-data:www-data
  - path: /var/www/html/index.html
    content: |
        Hello world from Virtual Machine Scale Set !
runcmd:
  - service nginx restart
```

This file contains configuration information to install nginx on the VMs in the scale set.

4. Press Ctrl+S to save the file. Then press Ctrl+Q to close the code editor.

5. Run the following command to create a new resource group named scalesetrg for your scale set:

```Azure CLI
az group create \
  --location westus \
  --name scalesetrg
```

6. Run the following command to create the virtual machine scale set:

```Azure CLI
az vmss create \
  --resource-group scalesetrg \
  --name webServerScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.yaml \
  --admin-username azureuser \
  --generate-ssh-keys
```

By default, the new virtual machine scale set has two instances and a load balancer.

> Note <br>
> The `custom-data` flag specifies that the VM configuration should use the settings in the cloud-init.yaml file after the VM has been created. You can use a cloud-init file to install additional packages, configure security, and write to files when the machine is first installed.
> For more information, see Cloud-init support for VMs in Azure.

#### Configure the virtual machine scale set

1. Run the following command to add a health probe to the load balancer:

```Azure CLI
az network lb probe create \
  --lb-name webServerScaleSetLB \
  --resource-group scalesetrg \
  --name webServerHealth \
  --port 80 \
  --protocol Http \
  --path /
```

The health probe pings the root of the website through port 80. If the website doesn't respond, the server is considered unavailable. The load balancer won't route traffic to the server.

2. Run the following command to configure the load balancer to route HTTP traffic to the instances in the scale set:

```Azure CLI
az network lb rule create \
  --resource-group scalesetrg \
  --name webServerLoadBalancerRuleWeb \
  --lb-name webServerScaleSetLB \
  --probe-name webServerHealth \
  --backend-pool-name webServerScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```

#### Test the virtual machine scale set

1. In the Azure portal, on the left, select Resource groups > scalesetrg.

2. Select the webServerScaleSet virtual machine scale set.

3. On the Overview page, note the public IP address of the virtual machine scale set.

4. Under Settings, select Instances. Verify that the scale set contains two running VMs.

5. Select Operating system. Verify that the VMs are running Ubuntu Linux.

6. Select Size. The VMs should be running on DS1_v2 hardware.

7. In your web browser, go to the public IP address of the scale set. Verify that the message Hello World from Virtual Machine Scale Set ! appears

### Configure a virtual machine scale set

If you need to handle a steady expansion of work over time, the best approach is to scale horizontally. But if the workload increases in complexity rather than in volume, and this complexity demands more of your resources, you might prefer to scale vertically.

When you scale horizontally, you add instances to your virtual machine scale set. In the shipping company scenario, horizontal scaling is a good way to handle the changing number of requests over time. Horizontal scaling adjusts the number of virtual machines that run the web application as the number of users changes. In this way, the system maintains an even response time, regardless of the current load.

In this unit, you'll learn how to scale a virtual machine scale set. You can scale manually by explicitly setting the number of virtual machine instances in the scale set. Or you can configure autoscaling by defining scale rules that trigger the allocation and deallocation of virtual machines. These scale rules determine when to scale the system by monitoring various performance metrics.

#### Manually scale virtual machine scale sets

You scale a virtual machine scale set manually by increasing or decreasing the instance count. You can do this task programmatically or in the Azure portal.

The following code uses the Azure CLI to change the number of instances in a virtual machine scale set:

```Bash
az vmss scale \
    --name MyVMScaleSet \
    --resource-group MyResourceGroup \
    --new-capacity 6
```

#### Autoscale virtual machine scale sets

Manual scaling is useful in some circumstances. But in many situations, autoscaling is better. It lets the system control the number of instances in a scale set.

You can base the autoscale on:

- **Schedule**: Use this approach if you know you'll have an increased workload on a specified date or time period.
- **Metrics**: Adjust scaling by monitoring performance metrics associated with the scale set. When these metrics exceed a specified threshold, the scale set can automatically start new virtual machine instances. When the metrics indicate that the additional resources are no longer required, the scale set can stop any excess instances.

#### Define autoscale conditions, rules, and limits

Autoscaling is based on a set of scale conditions, rules, and limits. A scale condition combines time and a set of scale rules. If the current time falls within the period defined in the scale condition, the condition's scale rules are evaluated. The results of this evaluation determine whether to add or remove instances in the scale set. The scale condition also defines the limits of scaling for the maximum and minimum number of instances.

In the shipping company scenario, you can add scale rules that monitor the CPU usage across the scale set. If the CPU usage exceeds the 75 percent threshold, the scale rule can increase the number of virtual machine instances. A second scale rule can also monitor CPU usage but reduce the number of virtual machine instances when usage falls below 50 percent. Because the application is global, these rules should be active all the time rather than just at specific hours.

A virtual machine scale set can contain many scale conditions. Each matching scale condition is acted on. A scale set can also contain a default scale condition that's used if no other scale conditions match the current time and performance metrics. The default scale condition is always active. It contains no scale rules, effectively acting like a null scale condition that doesn't scale in or out. However, you can modify the default scale condition to set a default instance count. Or you can add a pair of scale rules that scale out and back in again.

#### Use schedule-based autoscaling

Schedule-based scaling specifies a start and end time, and the number of instances to add to the scale set. The following screenshot shows an example in the Azure portal. The number of instances is scaled out to 20 between 6 AM and 6 PM each Monday and Wednesday. Outside of these times, if there are no other scale conditions, the default scale condition is applied.

In this case, the default rule scales the system back down to two instances. This value is the Maximum in this default scale condition.

#### Use metrics-based autoscaling

A metrics-based scale rule specifies the resources to monitor, such as CPU usage or response time. This scale rule adds or removes instances from the scale set according to the values of these metrics. You specify limits on the number of instances to prevent a scale set from excessively scaling in or out.

In the example scenario, you want to increase the instance count by one when the average CPU usage exceeds 75 percent. Additionally, you want to limit the scale-out operation to 50 instances. This limit can help to prevent costly runaway scaling caused by an attack. Similarly, you want to scale in when the average CPU usage drops below 50 percent.

These metrics are commonly used to monitor a virtual machine scale set:

- **Percentage CPU**: This metric indicates the CPU usage across all instances. A high value shows that instances are becoming CPU-bound, which could delay the processing of client requests.
- **Inbound flows and outbound flows**: These metrics show how fast network traffic is flowing into and out of virtual machines in the scale set.
- **Disks read operations/sec and disk write operations/sec**: These metrics show the volume of disk I/O across the scale set.
- **Data disk queue depth**: This metric shows how many I/O requests to only the data disks on the virtual machines are waiting to be serviced.

A scale rule aggregates the values retrieved for a metric for all instances. It aggregates the values across a period known as the time grain. Each metric has an intrinsic time grain, but usually this period is one minute. The aggregated value is known as the time aggregation. The time-aggregation options are average, minimum, maximum, total, last, and count.

A one-minute interval is too short to determine whether any change in the metric is long-lasting enough to make autoscaling worthwhile. A scale rule takes a second step, further aggregating the time aggregation's value over a longer, user-specified period. This period is called the duration. The minimum duration is five minutes. For example, if the duration is set to 10 minutes, the scale rule aggregates the 10 values calculated for the time grain.

The duration's aggregation calculation can differ from the time grain's aggregation calculation. For example, let's say the time aggregation is average and the statistic gathered is percentage CPU across a one-minute time grain. So for every minute, the average CPU percentage usage across all instances during that minute will be calculated. If the time-grain statistic is set to maximum and the rule's duration is set to 10 minutes, the maximum of the 10 average values for the CPU usage percentage determines whether the rule threshold has been crossed.

When a scale rule detects that a metric has crossed a threshold, it can do a scale action. A scale action can be a scale-out or a scale-in. A scale-out action increases the number of instances. A scale-in action reduces the instance count.

A scale action uses an operator such as less than, greater than, or equal to to determine how to react to the threshold. Scale-out actions typically use the greater than operator to compare the metric value to the threshold. Scale-in actions tend to compare the metric value to the threshold by using the less than operator. A scale action also sets the instance count to a specific level rather than increasing or decreasing the number available.

A scale action has a cool down period, specified in minutes. During this period, the scale rule isn't triggered again. The cool-down allows the system to stabilize between scale events. Starting or shutting down instances takes time, so any metrics gathered might not show significant changes for several minutes. The minimum cool-down period is five minutes.

Finally, plan for a scale-in when a workload decreases. Consider defining scale rules in pairs in the same scale condition. One scale rule should indicate how to scale the system out when a metric exceeds an upper threshold. The other rule needs to define how to scale the system back in again when the same metric drops below a lower threshold. Don't make both threshold values the same. Otherwise you could trigger a series of oscillating events that scale out and back in again.

The following image shows a scale rule defined in the Azure portal.

### Exercise - Configure a virtual machine scale set

Recall from the example scenario that your customers use one of the company's websites to manage and check the status of their shipments. This website is deployed to VMs and hosted on-premises.

You notice that users of the website have significant delays in response times when the overall CPU usage of the VMs exceeds 75 percent. You need the virtual machine scale set that hosts your web application to scale horizontally when the system hits this threshold. To save costs, you also want to scale back in when demand falls and the overall CPU usage across the scale set drops below 50 percent.

In this exercise, you'll configure autoscaling. You'll define scale rules that scale out and in again, according to the system's CPU usage.

#### Create a scale-out rule

1. In the Azure portal, go to the page for the virtual machine scale set.

2. On the virtual machine scale set page, under Settings, select Scaling.

3. Select Custom autoscale.

4. In the Default scale rule, ensure that the Scale mode is set to Scale based on a metric. Then select + Add a rule.

5. On the Scale rule page, specify the following settings, and then select Add:

| Property             | Value                                |
| -------------------- | ------------------------------------ |
| Metric source        | Current resource (webServerScaleSet) |
| Time aggregation     | Average                              |
| Metric name          | Percentage CPU                       |
| Time grain statistic | Average                              |
| Operator             | Greater than                         |
| Threshold            | 75                                   |
| Duration             | 10                                   |
| Operation            | Increase count by                    |
| Instance count       | 1                                    |
| Cool down (minutes)  | 5                                    |

#### Create a scale-in rule

1. In the Default scale rule, select + Add a rule.

2. On the Scale rule page, specify the following settings, and then select Add:

| Property             | Value                                |
| -------------------- | ------------------------------------ |
| Metric source        | Current resource (webServerScaleSet) |
| Time aggregation     | Average                              |
| Metric name          | Percentage CPU                       |
| Time grain statistic | Average                              |
| Operator             | Less than                            |
| Threshold            | 50                                   |
| Duration             | 10                                   |
| Operation            | Decrease count by                    |
| Instance count       | 1                                    |
| Cool down (minutes)  | 5                                    |

3. Select Save.

The Default scale condition now contains two scale rules. One rule scales the number of instances out. Another rule scales the number of instances back in.

### Install and update applications in virtual machine scale sets

When you deploy an application across a scale set, you need a mechanism that updates your application consistently, across all instances in the scale set. You achieve this outcome by using a custom script extension.

In the shipping company scenario, you need a quick way to roll out updates to the application while minimizing disruption to the end users. A custom script extension is an ideal solution.

In this unit, you'll learn how to use a custom script extension to update an application that runs on a scale se

#### What is an Azure custom script extension?

An Azure custom script extension downloads and runs a script on an Azure VM. It can automate the same tasks on all the VMs in a scale set.

Store your custom scripts in Azure Storage or in GitHub. To add one to a VM, you can use the Azure portal. To run custom scripts as part of a templated deployment, combine a custom script extension with Azure Resource Manager templates.

#### Install an application across a scale set by using a custom script extension

To use a custom script extension with the Azure CLI, you create a configuration file that defines the files to get and the commands to run. This file is in JSON format.

The following example shows a custom script configuration that downloads an application from a repository in GitHub and installs it on a host instance by running a script named `custom_application_v1.sh`:

```JSON
# yourConfigV1.json
{
  "fileUris": ["https://raw.githubusercontent.com/yourrepo/master/custom_application_v1.sh"],
  "commandToExecute": "./custom_application_v1.sh"
}
```

To deploy this configuration on the scale set, you use a custom script extension. The following code shows how to create a custom script extension for a virtual machine scale set by using the Azure CLI. This command installs the new app on the VMs across the scale set:

```Azure CLI
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name yourScaleSet \
  --settings @yourConfigV1.json
```

#### Update an application across a scale set by using a custom script extension

You can use a custom script extension to update an existing app across a virtual machine scale set. You reference an updated deployment script and then reapply the extension to your scale set. For example, the following JSON code shows a configuration that fetches a new version of an application and installs it:

```JSON
# yourConfigV2.json
{
  "fileUris": ["https://raw.githubusercontent.com/yourrepo/master/custom_application_v2.sh"],
  "commandToExecute": "./custom_application_v2.sh"
}
```

Use the same `az vmss extension set` command shown previously to deploy the updated app. But this time, reference the new configuration file:

```Azure CLI
az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --resource-group yourResourceGroup \
    --vmss-name yourScaleSet \
    --settings @yourConfigV2.json
```

The VMs are updated according to the upgrade policy for the scale set. You specify this policy when you first create the scale set. The upgrade policy can have one of the following three modes:

- **Automatic**: The scale set doesn't define when the VMs are upgraded. They could all update at the same time, causing a service outage.
- **Rolling**: The scale set rolls out the update in batches across the VMs in the scale set. An optional pause can minimize or eliminate a service outage. In this mode, machines in the scale set might run different versions of the app for a short time. This mode requires that you either add a health probe to the scale set or apply the application health extension to the scale set.
- **Manual**: Existing VMs in the scale set aren't updated. All changes must be done manually. This mode is the default.

To specify the upgrade policy mode when you provision a virtual machine scale set, use the `upgrade-policy-mode` option. The following code example uses the Azure CLI:

```Azure CLI
az vmss create \
  --resource-group MyResourceGroup \
  --name MyScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys
```

### Exercise - Update applications in virtual machine scale sets

In the shipping company scenario, you installed a web application by creating the virtual machine scale set. You now need to update the web app and install a new version across all VMs in the scale set.

You must ensure that the system will remain available during the roll-out. A good way to ensure availability is to use a custom script extension to do the update. Apply this script across the virtual machine scale set. The scale set will apply the update to one VM at a time, leaving the other VMs up and running.

In this exercise, you'll use a custom script extension to roll out a new version of the web app. You'll edit the message that's provided by the nginx server. You can use the same approach for bigger updates.

#### Deploy the update by using a custom script extension

1. In the Azure portal, run the following command to view the current upgrade policy for the scale set:

```Azure CLI
az vmss show \
    --name webServerScaleSet \
    --resource-group scalesetrg \
    --query upgradePolicy.mode
```

Verify that the upgrade policy is set to `Automatic`. You specified this policy when you created the scale set in the first lab. If the policy were `Manual`, you would apply any VM changes by hand. Because the policy is `Automatic`, you can use the custom script extension and allow the scale set to do the update.

2. Run the following command to apply the update script:

```Azure CLI
az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --vmss-name webServerScaleSet \
    --resource-group scalesetrg \
    --settings "{\"commandToExecute\": \"echo This is the updated app installed on the Virtual Machine Scale Set ! > /var/www/html/index.html\"}"
```

#### Test the updated web application

1. Run the following command to retrieve the IP address of the load balancer for the scale set:

```Azure CLI
az network public-ip show \
    --name webServerScaleSetLBPublicIP \
    --resource-group scalesetrg \
    --output tsv \
    --query ipAddress
```

2. In your web browser, go to the public address of the scale set load balancer. Verify that you see the message This is the updated app installed on the Virtual Machine Scale Set !.

### Summary

In this module, you looked at the features of virtual machine scale sets. You saw how to manually scale a scale set. You saw how to configure a scale set to autoscale based on a schedule or on performance metrics. You also learned how to use a custom extension script to deploy and update an application across the VMs in a scale set.

In the example scenario, you applied your knowledge to the shipping company's system. You deployed the website by using a scale set, and you configured the scale set to scale out and back in as the CPU usage across the VMs changed. You also rolled out a new version of the web application across the scale set while allowing the VMs to continue running. These actions allow users to access the system and maintain a good response time, even when the application is being updated.

#### Clean up

In Cloud Shell, run the following command to delete the scalesetrg resource group. This action also removes the scale set.

```Bash
az group delete \
      --name scalesetrg \
      --yes
```

## Run Docker containers with Azure Container Instances

### Introduction to Azure Container Instances

Containers offer a standardized and repeatable way to package, deploy and manage cloud applications. Azure Container Instances let you run a container in Azure without managing virtual machines and without a higher-level service.

#### Learning objectives

In this module, you will:

- Run containers in Azure Container Instances
- Control what happens when your container exits
- Use environment variables to configure your container when it starts
- Attach a data volume to persist data when your container exits
- Learn some basic ways to troubleshoot issues on your Azure containers

### Exercise - Run Azure Container Instances

Here, you create a container in Azure and expose it to the Internet with a fully qualified domain name (FQDN).

#### Why use Azure Container Instances?

Azure Container Instances is useful for scenarios that can operate in isolated containers, including simple applications, task automation, and build jobs. Here are some of the benefits:

- **Fast startup**: Launch containers in seconds.
- **Per second billing**: Incur costs only while the container is running.
- **Hypervisor-level security**: Isolate your application as completely as it would be in a VM.
- **Custom sizes**: Specify exact values for CPU cores and memory.
- **Persistent storage**: Mount Azure Files shares directly to a container to retrieve and persist state.
- **Linux and Windows**: Schedule both Windows and Linux containers using the same API.
  For scenarios where you need full container orchestration, including service discovery across multiple containers, automatic scaling, and coordinated application upgrades, we recommend Azure Kubernetes Service (AKS).

#### Create a container

1. Sign into the Azure portal with your Azure subscription.

2. Open the Azure Cloud Shell from the Azure portal using the Cloud Shell icon.

3. Create a new resource group with the name learn-deploy-aci-rg so that it will be easier to clean up these resources when you are finished with the module. If you choose a different resource group name, remember it for the rest of the exercises in this module. You also need to choose a region in which you want to create the resource group, for example `East US`.

```Azure CLI
az group create --name learn-deploy-aci-rg --location eastus
```

You create a container by providing a name, a Docker image, and an Azure resource group to the `az container create` command. You can optionally expose the container to the Internet by specifying a DNS name label. In this example, you deploy a container that hosts a small web app. You can also select the location to place the image - you'll use the East US region, but you can change it to a location close to you.

4. You provide a DNS name to expose your container to the Internet. Your DNS name must be unique. For learning purposes, run this command from Cloud Shell to create a Bash variable that holds a unique name.

```Azure CLI
DNS_NAME_LABEL=aci-demo-$RANDOM
```

5. Run the following az container create command to start a container instance.

```Azure CLI
az container create \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer \
  --image microsoft/aci-helloworld \
  --ports 80 \
  --dns-name-label $DNS_NAME_LABEL \
  --location eastus
```

`$DNS_NAME_LABEL` specifies your DNS name. The image name, microsoft/aci-helloworld, refers to a Docker image hosted on Docker Hub that runs a basic Node.js web application.

6. When the az container create command completes, run az container show to check its status.

```Azure CLI
az container show \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer \
  --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
  --out table
```

You see your container's fully qualified domain name (FQDN) and its provisioning state. Here's an example.

```Output
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

If your container is in the Creating state, wait a few moments and run the command again until you see the Succeeded state.

From a browser, navigate to your container's FQDN to see it running. You see this.

### Exercise - Control restart behavior

The ease and speed of deploying containers in Azure Container Instances makes it a great fit for executing run-once tasks like image rendering or building and testing applications.

With a configurable restart policy, you can specify that your containers are stopped when their processes have completed. Because container instances are billed by the second, you're charged only for the compute resources used while the container executing your task is running.

#### What are container restart policies?

Azure Container Instances has three restart-policy options:

| Restart policy | Description                                                                                                                                                                                                                                                  |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Always         | Containers in the container group are always restarted. This policy makes sense for long-running tasks such as a web server. This is the default setting applied when no restart policy is specified at container creation.                                  |
| Never          | Containers in the container group are never restarted. The containers run one time only.                                                                                                                                                                     |
| OnFailure      | Containers in the container group are restarted only when the process executed in the container fails (when it terminates with a nonzero exit code). The containers are run at least once. This policy works well for containers that run short-lived tasks. |

#### Run a container to completion

To see the restart policy in action, create a container instance from the `microsoft/aci-wordcount` Docker image and specify the OnFailure restart policy. This container runs a Python script that analyzes the text of Shakespeare's Hamlet, writes the 10 most common words to standard output, and then exits.

1. Run this `az container create` command to start the container.

```Azure CLI
az container create \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer-restart-demo \
  --image microsoft/aci-wordcount:latest \
  --restart-policy OnFailure \
  --location eastus
```

Azure Container Instances starts the container and then stops it when its process (a script, in this case) exits. When Azure Container Instances stops a container whose restart policy is Never or OnFailure, the container's status is set to Terminated. 2. Run `az container show` to check your container's status.

```Azure CLI
az container show \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer-restart-demo \
  --query containers[0].instanceView.currentState.state
```

Repeat the command until it reaches the Terminated status.

3. View the container's logs to examine the output. To do so, run az container logs like this.

```Azure CLI
az container logs \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer-restart-demo
```

```JSON
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]
```

### Exercise - Set environment variables

Environment variables enable you to dynamically configure the application or script the container runs. You can use the Azure CLI, PowerShell, or the Azure portal to set variables when you create the container. Secured environment variables enable you to prevent sensitive information from displaying in the container's output.

Here, you'll create an Azure Cosmos DB instance and use environment variables to pass the connection information to an Azure container instance. An application in the container uses the variables to write and read data from Azure Cosmos DB. You will create both an environment variable and a secured environment variable so you can see the difference between them.

#### Deploy Azure Cosmos DB

1. When you deploy Azure Cosmos DB, you provide a unique database name. For learning purposes, run this command from Cloud Shell to create a Bash variable that holds a unique name.

```Bash
COSMOS_DB_NAME=aci-cosmos-db-$RANDOM
```

2. Run this az cosmosdb create command to create your Azure Cosmos DB instance.

```Azure CLI
COSMOS_DB_ENDPOINT=$(az cosmosdb create \
  --resource-group learn-deploy-aci-rg \
  --name $COSMOS_DB_NAME \
  --query documentEndpoint \
  --output tsv)
```

This command can take a few minutes to complete.

`$COSMOS_DB_NAME` specifies your unique database name. The command prints the endpoint address for your database. Here, the command saves this address to the Bash variable `COSMOS_DB_ENDPOINT`.

3. Run `az cosmosdb keys` list to get the Azure Cosmos DB connection key and store it in a Bash variable named `COSMOS_DB_MASTERKEY`.

```Azure CLI
COSMOS_DB_MASTERKEY=$(az cosmosdb keys list \
  --resource-group learn-deploy-aci-rg \
  --name $COSMOS_DB_NAME \
  --query primaryMasterKey \
  --output tsv)
```

#### Deploy a container that works with your database

Here you'll create an Azure container instance that can read from and write records to your Azure Cosmos DB instance.

The two environment variables you created in the last part, `COSMOS_DB_ENDPOINT` and `COSMOS_DB_MASTERKEY`, hold the values you need to connect to the Azure Cosmos DB instance.

1. Run the following `az container create` command to create the container.

```Azure CLI
az container create \
  --resource-group learn-deploy-aci-rg \
  --name aci-demo \
  --image microsoft/azure-vote-front:cosmosdb \
  --ip-address Public \
  --location eastus \
  --environment-variables \
    COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

`microsoft/azure-vote-front:cosmosdb` refers to a Docker image that runs a fictitious voting app.

Note the `--environment-variables` argument. This argument specifies environment variables that are passed to the container when the container starts. The container image is configured to look for these environment variables. Here, you pass the name of the Azure Cosmos DB endpoint and its connection key.

2. Run az container show to get your container's public IP address.

```Azure CLI
az container show \
  --resource-group learn-deploy-aci-rg \
  --name aci-demo \
  --query ipAddress.ip \
  --output tsv
```

3. From a browser, navigate to your container's IP address.

#### Use secured environment variables to hide connection information

In the previous part, you used two environment variables to create your container. By default, these environment variables are accessible through the Azure portal and command-line tools in plain text.

In this part, you'll learn how to prevent sensitive information, such as connection keys, from being displayed in plain text.

1. Let's start by seeing the current behavior in action. Run the following az container show command to display your container's environment variables.

```Azure CLI
az container show \
  --resource-group learn-deploy-aci-rg \
  --name aci-demo \
  --query containers[0].environmentVariables
```

You see that both values appear in plain text. Here's an example.

```JSON
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": "https://aci-cosmos.documents.azure.com:443/"
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": "Xm5BwdLlCllBvrR26V00000000S2uOusuglhzwkE7dOPMBQ3oA30n3rKd8PKA13700000000095ynys863Ghgw=="
  }
]
```

Although these values don't appear to your users through the voting application, it's a good security practice to ensure that sensitive information, such as connection keys, are not stored in plain text.

Secure environment variables prevent clear text output. To use secure environment variables, you use the `--secure-environment-variables` argument instead of the `--environment-variables` argument.

2. Run the following command to create a second container, named aci-demo-secure, that makes use of secured environment variables.

```Azure CLI
az container create \
  --resource-group learn-deploy-aci-rg \
  --name aci-demo-secure \
  --image microsoft/azure-vote-front:cosmosdb \
  --ip-address Public \
  --location eastus \
  --secure-environment-variables \
    COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

Note the use of the `--secure-environment-variables` argument. 3. Run the following `az container show` command to display your container's environment variables.

```Azure CLI
az container show \
  --resource-group learn-deploy-aci-rg \
  --name aci-demo-secure \
  --query containers[0].environmentVariables
```

```JSON
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": null
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": null
  }
]
```

In fact, the values of your environment variables do not appear at all. That's OK because these values refer to sensitive information. Here, all you need to know is that the environment variables exist.

### Exercise - Use data volumes

By default, Azure Container Instances are stateless. If the container crashes or stops, all of its state is lost. To persist state beyond the lifetime of the container, you must mount a volume from an external store.

Here, you'll mount an Azure file share to an Azure container instance so you can store data and access it later.

#### Create an Azure file share

Here you'll create a storage account and a file share that you'll later make accessible to an Azure container instance.

1. Your storage account requires a unique name. For learning purposes, run the following command to store a unique name in a Bash variable.

```Bash
STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
```

2. Run the following `az storage account` create command to create your storage account.

```Azure CLI
az storage account create \
  --resource-group learn-deploy-aci-rg \
  --name $STORAGE_ACCOUNT_NAME \
  --sku Standard_LRS \
  --location eastus
```

3. Run the following command to place the storage account connection string into an environment variable named `AZURE_STORAGE_CONNECTION_STRING`.

```Azure CLI
export AZURE_STORAGE_CONNECTION_STRING=$(az storage account show-connection-string \
  --resource-group learn-deploy-aci-rg \
  --name $STORAGE_ACCOUNT_NAME \
  --output tsv)
```

AZURE_STORAGE_CONNECTION_STRING is a special environment variable that's understood by the Azure CLI. The export part makes this variable accessible to other CLI commands you'll run shortly.

4. Run this command to create a file share, named aci-share-demo, in the storage account.

```Bash
az storage share create --name aci-share-demo
```

#### Get storage credentials

To mount an Azure file share as a volume in Azure Container Instances, you need these three values:

- The storage account name
- The share name
- The storage account access key

You already have the first two values. The storage account name is stored in the `STORAGE_ACCOUNT_NAME` Bash variable. You specified **aci-share-demo** as the share name in the previous step. Here you'll get the remaining value — the storage account access key.

1. Run the following command to get the storage account key.

```Azure CLI
STORAGE_KEY=$(az storage account keys list \
  --resource-group learn-deploy-aci-rg \
  --account-name $STORAGE_ACCOUNT_NAME \
  --query "[0].value" \
  --output tsv)
```

The result is stored in a Bash variable named `STORAGE_KEY`.

2. As an optional step, print the storage key to the console.

```Bash
echo $STORAGE_KEY
```

#### Deploy a container and mount the file share

To mount an Azure file share as a volume in a container, you specify the share and volume mount point when you create the container.

Run this `az container create` command to create a container that mounts `/aci/logs/ `to your file share.

```Azure CLI
az container create \
  --resource-group learn-deploy-aci-rg \
  --name aci-demo-files \
  --image microsoft/aci-hellofiles \
  --location eastus \
  --ports 80 \
  --ip-address Public \
  --azure-file-volume-account-name $STORAGE_ACCOUNT_NAME \
  --azure-file-volume-account-key $STORAGE_KEY \
  --azure-file-volume-share-name aci-share-demo \
  --azure-file-volume-mount-path /aci/logs/
```

2. Run az container show to get your container's public IP address.

```Azure CLI
az container show \
  --resource-group learn-deploy-aci-rg \
  --name aci-demo-files \
  --query ipAddress.ip \
  --output tsv
```

3. From a browser, navigate to your container's IP address. You see this.

4. Enter some text into the form and click Submit. This action creates a file that contains the text you entered in the Azure file share.

5. Run this `az storage file list` command to display the files that are contained in your file share.

```Azure CLI
az storage file list -s aci-share-demo -o table
```

6. Run `az storage file download` to download a file to your Cloud Shell session. Replace <filename> with one of the files that appeared in the previous step.

```Azure CLI
az storage file download -s aci-share-demo -p <filename>
```

7. Run the cat command to print the contents of the file.

```Azure CLI
cat <filename>
```

### Exercise - Troubleshoot Azure Container Instances

To help you understand basic ways to troubleshoot container instances, here you'll perform some basic operations such as:

- Pulling container logs
- Viewing container events
- Attaching to a container instance

#### Create a container

Run the following az container create command to create a basic container.

```Azure CLI
az container create \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer \
  --image microsoft/sample-aks-helloworld \
  --ports 80 \
  --ip-address Public \
  --location eastus
```

The `microsoft/sample-aks-helloworld` image runs a web server that displays a basic web page.

### Get logs from your container instance

Run the following `az container logs` command to see the output from the container's running application.

```Azure CLI
az container logs \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer
```

You see output that resembles the following.

```Output
Checking for script in /app/prestart.sh
Running script /app/prestart.sh
Running inside /app/prestart.sh, you could add migrations to this file, e.g.:

#! /usr/bin/env bash

# Let the DB start
sleep 10;
# Run migrations
alembic upgrade head
```

#### Get container events

The `az container attach` command provides diagnostic information during container startup. Once the container has started, it also writes standard output and standard error streams to your local terminal.

Run `az container attach` to attach to your container.

```Azure CLI
az container attach \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer
```

You see output that resembles the following.

```Output
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-09-21 23:48:14+00:00) pulling image "microsoft/sample-aks-helloworld"
(count: 1) (last timestamp: 2018-09-21 23:49:09+00:00) Successfully pulled image "microsoft/sample-aks-helloworld"
(count: 1) (last timestamp: 2018-09-21 23:49:12+00:00) Created container
(count: 1) (last timestamp: 2018-09-21 23:49:13+00:00) Started container

Start streaming logs:
Checking for script in /app/prestart.sh
Running script /app/prestart.sh
```

#### Execute a command in your container

As you diagnose and troubleshoot issues, you may need to run commands directly on your running container.

1. To see this in action, run the following `az container exec` command to start an interactive session on your container.

```Azure CLI
az container exec \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer \
  --exec-command /bin/sh
```

2. At this point, you are effectively working inside of the container.

```Output
# ls
__pycache__  main.py  prestart.sh  static  templates  uwsgi.ini
```

3. You can explore the system further if you wish. When you're done, run the exit command to stop the interactive session.

#### Monitor CPU and memory usage on your container

Here you'll see how to monitor CPU and memory usage on your container.

1. Run the following `az container show` command to get the ID of your Azure container instance and store the ID in a Bash variable.

```Azure CLI
CONTAINER_ID=$(az container show \
  --resource-group learn-deploy-aci-rg \
  --name mycontainer \
  --query id \
  --output tsv)
```

2. Run the `az monitor metrics list` command to retrieve CPU usage information.

```Azure CLI
az monitor metrics list \
  --resource $CONTAINER_ID \
  --metric CPUUsage \
  --output table
```

Note the `--metric` argument. Here, CPUUsage specifies to retrieve CPU usage.

You see output similar to this.

```Output
Timestamp            Name              Average
-------------------  ------------  -----------
2018-08-20 21:39:00  CPU Usage
2018-08-20 21:40:00  CPU Usage
2018-08-20 21:41:00  CPU Usage
2018-08-20 21:42:00  CPU Usage
2018-08-20 21:43:00  CPU Usage      0.375
2018-08-20 21:44:00  CPU Usage      0.875
2018-08-20 21:45:00  CPU Usage      1
2018-08-20 21:46:00  CPU Usage      3.625
2018-08-20 21:47:00  CPU Usage      1.5
2018-08-20 21:48:00  CPU Usage      2.75
2018-08-20 21:49:00  CPU Usage      1.625
2018-08-20 21:50:00  CPU Usage      0.625
2018-08-20 21:51:00  CPU Usage      0.5
2018-08-20 21:52:00  CPU Usage      0.5
2018-08-20 21:53:00  CPU Usage      0.5
```

3. Run this `az monitor metrics` list command to retrieve memory usage information.

```Azure CLI
az monitor metrics list \
  --resource $CONTAINER_ID \
  --metric MemoryUsage \
  --output table
```

Here, you specify MemoryUsage for the `--metric` argument to retrieve memory usage information.

You see output similar to this.

```Output
Timestamp            Name              Average
-------------------  ------------  -----------
2018-08-20 21:43:00  Memory Usage
2018-08-20 21:44:00  Memory Usage  0.0
2018-08-20 21:45:00  Memory Usage  15917056.0
2018-08-20 21:46:00  Memory Usage  16744448.0
2018-08-20 21:47:00  Memory Usage  16842752.0
2018-08-20 21:48:00  Memory Usage  17190912.0
2018-08-20 21:49:00  Memory Usage  17506304.0
2018-08-20 21:50:00  Memory Usage  17702912.0
2018-08-20 21:51:00  Memory Usage  17965056.0
2018-08-20 21:52:00  Memory Usage  18509824.0
2018-08-20 21:53:00  Memory Usage  18649088.0
2018-08-20 21:54:00  Memory Usage  18845696.0
2018-08-20 21:55:00  Memory Usage  19181568.0
```

#### Clean up resources

In this module you created resources using your Azure subscription. You want to clean up these resources so that you will not continue to be charged for them.

1. In Azure, select Resource groups on the left.

2. Find the learn-deploy-aci-rg resource group, or whatever resource group name you used, and select it.

3. In the Overview tab of the resource group, select Delete resource group.

4. This opens a new dialog box. Type the name of the resource group again and select Delete. This will delete all of the resources we created in this module.

## Host your domain on Azure DNS

### Introduction

Azure DNS enables you to host your DNS records for your domains on Azure infrastructure. With Azure DNS, you can use the same credentials, APIs, tools, and billing as your other Azure services.

Let's say that your company recently bought the custom domain name wideworldimporters.com from a third-party domain name registrar. The domain name is for a new website that your organization plans to launch. You need a hosting service for DNS domains. This hosting service would resolve the wideworldimporters.com domain to the IP address of your web server.

You're already using Azure to build your website. You decide to use Azure DNS to manage your domain.

This module shows you how to configure Azure DNS to host your domain. You'll also see how to add an alias and other DNS records to resolve your domain name to a website.

#### Learning objectives

In this module, you will:

- Configure Azure DNS to host your domain.

#### Prerequisites

Knowledge of networking concepts, like name resolution and IP addresses.

### What is Azure DNS?

Azure DNS is a hosting service for DNS domains that provides name resolution by using Microsoft Azure infrastructure.

In this unit, you'll learn what DNS is and how it works. Then learn about Azure DNS, and why you would use it.

#### What is DNS?

DNS, or the Domain Name System, is a protocol within the TCP/IP standard. DNS serves an essential role of translating the human-readable domain names, for example, `www.wideworldimports.com`, into a known IP address. IP addresses enable computers and network devices to identify and route requests between themselves.

DNS uses a global directory hosted on servers around the world. Microsoft is part of that network that provides a DNS service through Azure DNS.

A DNS server is also known as a DNS name server, or just a name server.

#### How does DNS work?

A DNS server carries out one of two primary functions:

- Maintains a local cache of recently accessed or used domain names and their IP addresses. This cache provides a faster response to a local domain lookup request. If the DNS server can't find the requested domain, it passes the request to another DNS server. This process repeats at each DNS server until either a match is made, or the search times out.

- Maintains the key-value pair database of IP addresses and any host or subdomain that the DNS server has authority over. This function is often associated with mail, web, and other internet domain services.

#### DNS server assignment

In order for a computer, server, or other network-enabled device to access web-based resources, it must reference a DNS server.

When you connect by using your on-premises network, the DNS settings come from your server. When you connect by using an external location, like a hotel, the DNS settings come from the internet service provider (ISP).

#### Domain lookup requests

Here's a simplified overview of the process a DNS server uses when it resolves a domain name lookup request:

- Checks to see if the domain name is stored in the short-term cache. If so, the DNS server resolves the domain request.
- If the domain isn't in the cache, it contacts one or more DNS servers on the web to see if they have a match. When a match is found, the DNS server updates the local cache and resolves the request.
- If the domain isn't found after a reasonable number of DNS checks, the DNS server responds with a domain cannot be found (404) error.

#### IPv4 and IPv6

Every computer, server, or network-enabled device on your network has an IP address. An IP address, within your domain, is unique. There are two standards of IP address: IPv4 and IPv6.

- IPv4 is composed of four numbers, in the range 0 to 255, separated by a dot. Example: 127.0.0.1. Today, IPv4 is the most commonly used standard. Yet, with the increase in IoT devices, the IPv4 standard will eventually be unable to keep up.

- IPv6 is a relatively new standard and will eventually replace IPv4. It's made up of eight groups of hexadecimal numbers, each separated by a colon. Example: fe80:11a1:ac15:e9gf:e884:edb0:ddee:fea3.

Many network devices are now provisioned with both an IPv4 and an IPv6 address. The DNS name server can resolve domain names to both IPv4 and IPv6 addresses.

#### DNS settings for your domain

Whether the DNS server for your domain is hosted by a third party or managed in-house, you'll need to configure it for each host type you're using. Host types include web, email, or other services you're using.

As the administrator for your company, you want to set up a DNS server by using Azure DNS. In this instance, the DNS server will act as a start of authority (SOA) for your domain.

#### DNS record types

The configuration information for your DNS server is stored as a file within a zone on your DNS server. Each file is called a record. The following record types are the most commonly created and used:

- **A** is the host record, and is the most common type of DNS record. It maps the domain or host name to the IP address.

- **CNAME** is the canonical name, or the alias for an A record. If you had different domain names that all accessed the same website, you would use CNAME.

- **MX** is the mail exchange record. It maps mail requests to your mail server, whether hosted on-premises or in the cloud.
- **TXT** is the text record. It's used to associate text strings with a domain name. Azure and Microsoft 365 use TXT records to verify domain ownership.

Additionally, there are the following record types:

- Wildcards
- CAA (certificate authority)
- NS (name server)
- SOA (start of authority)
- SPF (sender policy framework)
- SRV (server locations)

The SOA and NS records are created automatically when you create a DNS zone by using Azure DNS.

#### Record sets

Some record types support the concept of record sets, or resource record sets. A record set allows for multiple resources to be defined in a single record. For example, here is an A record that has one domain with two IP addresses:

```Output
www.wideworldimports.com.     3600    IN    A    127.0.0.1
www.wideworldimports.com.     3600    IN    A    127.0.0.2
```

SOA and CNAME records can't contain record sets.

#### What is Azure DNS?

Azure DNS allows you to host and manage your domains by using a globally distributed name server infrastructure. It allows you to manage all of your domains by using your existing Azure credentials.

Azure DNS acts as the SOA for the domain.

You can't use Azure DNS to register a domain name. You use a third-party domain registrar to register your domain.

#### Why use Azure DNS to host your domain?

Azure DNS is built on the Azure Resource Manager service, which offers the following benefits:

- Improved security
- Ease of use
- Private DNS domains
- Alias record sets

At this time, Azure DNS doesn't support Domain Name System Security Extensions. If you require this security extension, you should host those portions of your domain with a third-party provider.

#### Security features

Azure DNS provides the following security features:

- Role-based access control, which gives you fine-grained control over users' access to Azure resources. You can monitor their usage, and control the resources and services they have access to.
- Activity logs, which let you track changes to a resource, and pinpoint where faults occurred.
- Resource locking, which gives a greater level of control to restrict or remove access to resource groups, subscriptions, or any Azure resources.

#### Ease of use

Azure DNS can manage DNS records for your Azure services, and provide DNS for your external resources. Azure DNS uses your same Azure credentials, support contract, and billing as your other Azure services.

You can manage your domains and records by using the Azure portal, Azure PowerShell cmdlets, or the Azure CLI. Applications that require automated DNS management can integrate with the service by using the REST API and SDKs.

#### Private domains

Azure DNS handles the translation of external domain names to an IP address. Azure DNS lets you create private zones. These provide name resolution for virtual machines (VMs) within a virtual network, and between virtual networks, without having to create a custom DNS solution. This allows you to use your own custom domain names rather than the Azure-provided names.

To publish a private DNS zone to your virtual network, you specify the list of virtual networks that are allowed to resolve records within the zone.

Private DNS zones have the following benefits:

- There's no need to invest in a DNS solution. DNS zones are supported as part of the Azure infrastructure.
- All DNS record types are supported: A, CNAME, TXT, MX, SOA, AAAA, PTR, and SVR.
- Host names for VMs in your virtual network are automatically maintained.
- Split-horizon DNS support allows the same domain name to exist in both private and public zones. It resolves to the correct one based on the originating request location.

#### Alias record sets

Alias records sets can point to an Azure resource. For example, you can set up an alias record to direct traffic to an Azure public IP address, an Azure Traffic Manager profile, or an Azure Content Delivery Network endpoint.

The alias record set is supported in the following DNS record types:

- A
- AAAA
- CNAME

### Configure Azure DNS to host your domain

The new company website is in final testing. You're working on the plan to deploy the wideworldimports.com domain by using Azure DNS. You need to understand what steps are involved.

In this unit, you'll learn how to:

- Create and configure a DNS zone for your domain by using Azure DNS.
- Understand how to link your domain to an Azure DNS zone.
- Create and configure a private DNS zone.

#### Configure a public DNS zone

You use a DNS zone to host the DNS records for a domain, such as wideworldimports.com.

##### Step 1: Create a DNS zone in Azure

You used a third-party domain-name registrar to register the wideworldimports.com domain. The domain doesn't point to your organization's website yet.

To host the domain name with Azure DNS, you first need to create a DNS zone for that domain. A DNS zone holds all the DNS entries for your domain.

When creating a DNS zone, you need to supply the following details:

- Subscription: The subscription to be used.

- Resource group: The name of the resource group to hold your domains. If one doesn't exist, create one to allow for better control and management.

- Name: The name of your domain, which in this case is wideworldimports.com.

- Resource group location: The location defaults to the location of the resource group.

##### Step 2: Get your Azure DNS name servers

After you create a DNS zone for the domain, you need to get the name server details from the name servers (NS) record. You use these details to update your domain registrar's information, and point to the Azure DNS zone.

#### Step 3: Update the domain registrar setting

As the owner of the domain, you need to sign in to the domain management application provided by your domain registrar. In the management application, edit the NS record, and change the NS details to match your Azure DNS name server details.

Changing the NS details is called domain delegation. When you delegate the domain, you must use all four name servers provided by Azure DNS.

##### Step 4: Verify delegation of domain name services

The next step is to verify that the delegated domain now points to the Azure DNS zone you created for the domain. This can take as few as 10 minutes, but might take longer.

To verify the success of the domain delegation, query the start of authority (SOA) record. The SOA record was automatically created when the Azure DNS zone was set up. You can do this by using a third-party tool, like nslookup.

The SOA record represents your domain and will become the reference point when other DNS servers are searching for your domain on the internet.

To verify the delegation, use nslookup like this:

```dos
nslookup -type=SOA wideworldimports.com
```

##### Step 5: Configure your custom DNS settings

The domain name is wideworldimports.com. When it's used in a browser, the domain resolves to your website. But what if you want to add in web servers, or load balancers? These resources need to have their own custom settings in the DNS zone, either as an A record or a CNAME.

##### A record

Each A record requires the following details:

- Name: The name of the custom domain, for example webserver1.
- Type: In this instance, it's A.
- TTL: Represents the "time-to-live" as a whole unit, where 1 is one hour. This value indicates how long the A record lives in a DNS cache before it expires.
- IP address: The IP address of the server this A record should resolve to.

##### CNAME record

The CNAME is the canonical name, or the alias for an A record. Use CNAME when you have different domain names that all access the same website. For example, you might need a CNAME in the wideworldimports zone, if you want both www.wideworldimports.com and wideworldimports.com to resolve to the same IP address.

You would create the CNAME record in the wideworldimports zone with the following information:

- NAME: www
- TTL: 600 seconds
- Record type: CNAME
  If you exposed a web function, you would create a CNAME record that resolves to the Azure function.

#### Configure private DNS zone

To provide name resolution for virtual machines (VMs) within a virtual network and between virtual networks, create a private DNS zone.

##### Step 1: Create private DNS zone

In the Azure portal, search on private dns zones. To create the private zone, you need enter a resource group and the name of the zone. For example, the name might be something like private.wideworldimports.com.

##### Step 2: Identify virtual networks

Let's assume that your organization has already created your VMs and virtual networks in a production environment. Identify the virtual networks associated with VMs that need name resolution support. To link the virtual networks to the private zone, you need the virtual network names.

##### Step 3: Link your virtual network to a private DNS zone

To link the private DNS zone to a virtual network, you create a virtual network link. In the Azure portal, go to the private zone and select **Virtual network links**.

Select Add to pick the virtual network you want to link to the private zone.

You add a virtual network link record for each virtual network that needs private name resolution support.

In the next unit, you'll learn how to create a public DNS zone.

### Exercise - Create a DNS zone and an A record by using Azure DNS

In the previous unit, we discussed setting up and configuring the widewworldimports.com domain to point to your Azure hosting on Azure DNS.

In this unit, you'll:

Set up an Azure DNS and create a public DNS zone.
Create an A record.
Verify that the A record resolves to an IP address.

#### Create a DNS zone in Azure DNS

Before you can host the wideworldimports.com domain on your servers, you need to create a DNS zone. The DNS zone holds all the configuration records associated with your domain.

To create your DNS zone:

1. Sign in to the Azure portal with the account you used to activate the sandbox.

2. Select + Create a resource.

3. Search for and select DNS zone.

4. Select Create.

5. Enter the following information:

| Field          | Value                                                                                                                         |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| Subscription   | Concierge subscription                                                                                                        |
| Resource group | learn-acc8480d-3289-4033-9abf-41623c46cc1d                                                                                    |
| Name           | The name needs to be unique in the sandbox. So use wideworldimportsXXXX.com where you replace the Xs with letters or numbers. |

6.Select Review + create.

7. Select Create. It will take a few minutes to create the DNS zone.

8. Select Go to resource.

By default, the NS and SOA records are automatically created. The NS record defines the Azure DNS name spaces and contains the four Azure DNS record sets. You use all four record sets when you update the registrar.

The SOA record represents your domain, and is used when other DNS servers are searching for your domain.

```Output
Deployment name: Microsoft.DnsZone-20200926170650484
Subscription: Concierge Subscription
Resource group: learn-acc8480d-3289-4033-9abf-41623c46cc1d
Start time: 9/27/2020, 12:06:54 AM
Correlation ID: bf5025c0-fdcc-4c07-8cab-17df6284aeeb

Resource
Type
Status
Operation details
wideworldimport20XXX.com
Microsoft.Network/dnsZones
Created
Operation details
```

9. Make a note of the NS record values. You need them in the next section.

#### Create a DNS record

Now that the DNS zone exists, you need to create the necessary records to support the domain.

The primary record to create is the A record. This record contains the pairing between the IP address and the domain name. The A record can have multiple entries, called record sets. In record sets, the domain name remains constant, while the IP addresses are different.

1. In the Azure portal, select All Resources.

2. Select the DNS zone you created (wideworldimportsXXXX.com).

3. Select + Record set.

| Field            | Value       | Description                                                                                                                      |
| ---------------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Name             | www         | The host name that you want to resolve to an IP address.                                                                         |
| Type             | A           | The A record is the most commonly used. If you're using IPv6, select the AAAA type.                                              |
| Alias record set | No          | This can only be applied to A, AAAA, and CNAME record types.                                                                     |
| TTL              | 1           | The time-to-live period, which specifies how long each DNS server caches the resolution before it's purged.                      |
| TTL unit         | hours       | This value can be seconds, minutes, hours, days, or weeks. Here, you're selecting hours.                                         |
| IP Address       | 10.10.10.10 | The IP address the record name resolves to. In a real-world scenario, you would enter the public IP address for your web server. |

Select OK to add the record to your zone.

Note that it's possible to have more than one IP address set up for your web server. In that case, you add all the associated IP addresses as part of a record set. You can update the record set after it's created with additional IP addresses.

#### Verify your global Azure DNS

In a real-world scenario, after you create the public DNS zone, you update the NS records of the domain-name registrar, to delegate the domain to Azure.

Even though we don't have a registered domain, it's still possible to verify that the DNS zone works as expected, by using the nslookup tool.

#### Use nslookup to verify the configuration

Here's how to use nslookup to verify the DNS zone configuration.

Use Cloud Shell to run the following command. Replace the DNS zone name with the zone you created. Also replace the name server address with one of the NS values you copied after you created the DNS zone.

```Bash
nslookup www.wideworldimportsXXXX.com <name server address>
```

The command should look something like the following:

```Bash
nslookup www.wideworldimport20XXX.com ns1-08.azure-dns.com
```

2. You should see that your host name www.wideworldimportsXXXX.com resolves to 10.10.10.10.

```Output
Server:         ns1-08.azure-dns.com
Address:        40.90.4.8#53

Name:   www.wideworldimport20XXX.com
Address: 10.10.10.10
```

You have successfully set up a DNS zone and created an A record.

### Dynamically resolve resource name by using alias record

You have now successfully delegated the domain from the domain registrar to your Azure DNS, and configured an A record to link the domain to your web server.

The next phase of the deployment is to improve resiliency by using a load balancer. Load balancers distribute inbound data requests and traffic across one or more servers. They reduce the load on any one server, and improve performance. This technology is well established, and will be used throughout your on-premises network.

You know that the A record and CNAME record don't support direct connection to Azure resources like your load balancers. You've been tasked with finding out how to link the apex domain with a load balancer.

#### What is an apex domain?

The apex domain is the highest level of your domain. In our case, that's wideworldimports.com. Note that the apex domain is also sometimes referred to as the `zone apex` or `root apex`. It's often represented by the @ symbol in your DNS zone records.

If you check the DNS zone for wideworldimports.com, you'll see there are two apex domain records: NS and SOA. The NS and SOA records are automatically created when you created the DNS zone.

CNAME records that you might need for an Azure Traffic Manager profile or Azure Content Delivery Network endpoints aren't supported at the zone apex level. Alias records are supported at the zone apex level.

#### What are alias records?

Azure alias records enable a zone apex domain to reference other Azure resources from the DNS zone. You don't need to create complex redirection policies. You can also use an Azure alias to route all traffic through Traffic Manager.

The Azure alias record can point to the following Azure resources:

- A Traffic Manager profile
- Azure Content Delivery Network endpoints
- A public IP resource
- A front door profile

Alias records provide lifecycle tracking of target resources, ensuring that changes to any target resource are automatically applied to the DNS zone. Alias records also provide support for load-balanced applications in the zone apex.

The alias record set supports the following DNS zone record types:

- A: The IPv4 domain name-mapping record.
- AAAA: The IPv6 domain name-mapping record.
- CNAME: The alias for your domain, and links to the A record.

#### Uses for alias records

The following are some of the advantages of using alias records:

- Prevents dangling DNS records: A dangling DNS record occurs when the DNS zone records aren't up-to-date with changes to IP addresses. Alias records prevent dangling references by tightly coupling the lifecycle of a DNS record with an Azure resource.
- Updates DNS record set automatically when IP addresses change: When the underlying IP address of a resource, service, or application is changed, the alias record ensures that any associated DNS records are automatically refreshed.
- Hosts load-balanced applications at the zone apex: Alias records allow for zone apex resource routing to Traffic Manager.
- Points zone apex to Azure Content Delivery Network endpoints: With alias records, you can now directly reference your Azure Content Delivery Network instance.

An alias record enables you to link the zone apex (wideworldimports.com) to a load balancer. It creates a link to the Azure resource, rather than a direct IP-based connection. So, if the IP address of your load balancer changes, the zone apex record continues to work.

### Exercise - Create alias records for Azure DNS

The deployment of your new website was a huge success. Usage volumes are much higher than anticipated. The single web server that the website runs on is showing signs of strain. Your organization wants to increase the number of servers, and distribute the load using a load balancer.

You now know you can use an Azure alias record to provide a dynamic, automatically refreshing link between the zone apex and the load balancer.

In this unit, you'll:

- Set up a virtual network with two VMs and a load balancer.
- Learn how to configure an Azure alias at the zone apex to direct to the load balancer.
- Verify that the domain name resolves to one or either of the VMs on your virtual network.

#### Set up a virtual network, load balancer, and VMs in Azure

Manually creating a virtual network, load balancer, and two VMs will take some time. To reduce this, you can use a Bash setup script, which is available on GitHub. Follow these instructions to create a test environment for your alias record.

1. In Azure Cloud Shell, run the following setup script.

```Bash
git clone https://github.com/MicrosoftDocs/mslearn-host-domain-azure-dns.git
```

2. To run the setup script, run the following commands:

```Bash
cd mslearn-host-domain-azure-dns
chmod +x setup.sh
./setup.sh
```

The setup script is going to take a few minutes to run. The script:

- Creates a network security group.
- Creates two NIC's and two VMs.
- Creates a virtual network and assigns the VMs.
- Creates a public IP address and updates the configuration of the VMs.
- Creates a load balancer that references the VMs, including rules for the load balancer.
- Links the NICs to the load balancer.
  After the script completes, it shows you the public IP address for the load balancer. Copy the IP address to use it later.

#### Create an alias record in your zone apex

Now that you've created a test environment, you're ready to set up the Azure alias record in your zone apex.

In the Azure portal , go to the learn-acc8480d-3289-4033-9abf-41623c46cc1d

1. Select your DNS zone (wideworldimport20XXX.com).

2. Select + Record set.

3. Use the following settings to create an alias record.

| Field            | Setting                                                                                              |
| ---------------- | ---------------------------------------------------------------------------------------------------- |
| Name             | Leave the name blank. By leaving it blank, it indicates the DNS zone for wideworldimportsXXXX.com    |
| Type             | A. Even though we're creating an alias, the base record type must still be either A, AAAA, or CNAME. |
| Alias record set | Yes                                                                                                  |
| Alias type       | You can select either Azure resource or Zone record set. In this case, select the Azure resource.    |
| Azure resource   | From the list of resources, select myPublicIP.                                                       |

#### Verify that the alias resolves to the load balancer

Now you need to verify that the alias record is set up correctly. In a real-world scenario, you'll have an actual domain, and would have completed the domain delegation to Azure DNS. So you would use the registered domain name for this exercise. Because this unit assumes there's no registered domain, you'll use the public IP address.

1. If you didn't copy the public IP address in a previous step, go to the resource group, and select myPublicIP. The IP address is on the upper-right side.

2. In a web browser, paste in the public IP address as the URL.
3. You'll see a basic web page that shows the name of the VM that the load balancer sent the request to.

#### Summary

Your company recently bought the custom domain name wideworldimporters.com from a third-party domain name registrar. The domain name is for a new website your organization plans to launch. You need a hosting service for DNS domains. This hosting service would resolve the wideworldimporters.com domain to the IP address of your Azure-based web server.

Your company wanted to manage all their infrastructure and related domain name information in one place. You've seen how easy it was to manage DNS information by using an Azure DNS zone. First, you created an Azure DNS zone, and then you updated the NS records at your domain registrar to point at it.

You learned the uses of the different record sets, A, AAAA, CNAME, NS, and SOA. You also learned how you could use Azure aliases to override the static A/AAAA/CNAME record to provide a dynamic reference to your resources. Using an Azure DNS zone improved your company's administration of resources, because your staff only needed one place to manage DNS-related tasks.

The Azure DNS zone allows better control and integration with your Azure resources. It's possible to achieve some of the more basic record set functions by using the domain registrar's management console. However, linking to any of your Azure resources becomes difficult or impossible without a high degree of complex redirection.

By using an Azure DNS zone to host your domain, your organization benefits by having all the resources managed through a single, common interface. This includes better integration with existing Azure resources, improved security, and monitoring tools.

## Secure and isolate access to Azure resources by using network security groups and service endpoints

### Introduction

Imagine you are the solution architect for a manufacturing company. Your company has several sites, and users throughout the company will need to use an enterprise resource planning (ERP) application to migrate to Azure. The company will only consider moving key systems onto the platform if stringent security requirements can be met, including tight control over which computers have network access to the servers running the application. You want to secure both virtual machine networking and Azure services networking as part of your company's network security strategy. Your goal is to prevent unwanted or unsecured network traffic from being able to reach key systems.

You'll use network security groups to secure network traffic for virtual machines running on Azure. You'll learn to use virtual network service endpoints to control network traffic to and from Azure services, such as storage or database services.

#### Learning objectives

In this module, you will:

- Identify the capabilities and features of network security groups.
- Identify the capabilities and features of virtual network service endpoints.
- Use network security groups to restrict network connectivity.
- Use virtual network service endpoints to control network traffic to and from Azure services.

#### Prerequisites

- Knowledge of basic networking concepts, including subnets and IP addressing.
- Basic familiarity with Azure services, specifically Azure SQL Database and Azure Storage.
- Familiarity with Azure virtual machines and virtual networking.

### Use network security groups to control network access

As part of the project to move your ERP system to Azure, you need to ensure that servers have proper isolation, so that only allowed systems can make network connections. For example, you have database servers that store data for your ERP application. You want to block prohibited systems from communicating with the servers over the network, while allowing application servers to communicate with the database servers.

#### Network security groups

Network security groups filter network traffic to and from Azure resources. Network security groups contain security rules that you configure to allow or deny inbound and outbound traffic. You can use network security groups to filter traffic between virtual machines or subnets, both within a virtual network and from the internet.

#### Network security group assignment and evaluation

Network security groups are assigned to a network interface or a subnet. When you assign a network security group to a subnet, the rules apply to all network interfaces in that subnet. You can restrict traffic further by associating a network security group to the network interface of a virtual machine.

When you apply network security groups to both a subnet and a network interface, each network security group is evaluated independently. Inbound traffic is first evaluated by the network security group applied to the subnet, and then by the network security group applied to the network interface. Conversely, outbound traffic from a virtual machine is first evaluated by the network security group applied to the network interface, and then by the network security group applied to the subnet.

Applying a network security group to a subnet instead of individual network interfaces can reduce administration and management efforts. This approach also ensures that all virtual machines within the specified subnet are secured with the same set of rules.

Each subnet and network interface can have one network security group applied to it. Network security groups support TCP, UDP, and ICMP, and operate at Layer 4 of the OSI model.

In our manufacturing company scenario, network security groups can help you secure the network. You can control which computers can connect to your application servers. You configure the network security group so that only a specific range of IP addresses can connect to the servers. You can lock this down even more by only allowing access to or from specific ports, or from individual IP addresses. These rules can be applied to devices that are connecting remotely from on-premises networks, or between resources within Azure.

#### Security rules

A network security group contains one or more security rules. Configure security rules to either allow or deny traffic.

Rules have several properties:

| Property              | Explanation                                                                                                                                           |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Name                  | A unique name within the network security group.                                                                                                      |
| Priority              | A number between 100 and 4096.                                                                                                                        |
| Source or destination | Any, or an individual IP address, classless inter-domain routing (CIDR) block (10.0.0.0/24, for example), service tag, or application security group. |
| Protocol              | TCP, UDP, or Any.                                                                                                                                     |
| Direction             | Whether the rule applies to inbound, or outbound traffic.                                                                                             |
| Port range            | An individual port or range of ports.                                                                                                                 |
| Action                | Allow or deny the traffic.                                                                                                                            |

Network security group security rules are evaluated by priority, using the 5-tuple information (source, source port, destination, destination port, and protocol) to allow or deny the traffic. When the conditions for a rule match the device configuration, rule processing stops.

For example, suppose your company has created a security rule to allow inbound traffic on port 3389 (RDP) to your web servers, with a priority of 200. Then suppose that another administrator has created a rule to deny inbound traffic on port 3389, with a priority of 150. The deny rule takes precedence, because it's processed first. The rule with priority 150 is processed before the rule with priority 200.

With network security groups, the connections are stateful. Return traffic is automatically allowed for the same TCP/UDP session. For example, an inbound rule allowing traffic on port 80 also allows the virtual machine to respond to the request (typically on an ephemeral port). You don't need a corresponding outbound rule.

With regard to the ERP system, the web servers for the ERP application connect to database servers that are in their own subnets. You can apply security rules to state that the only allowed communication from the web servers to the database servers is port 1433 for SQL Server database communications. All other traffic to the database servers will be denied.

#### Default security rules

When you create a network security group, Azure creates several default rules. These default rules can't be changed, but can be overridden with your own rules. These default rules allow connectivity within a virtual network and from Azure load balancers. They also allow outbound communication to the internet, and deny inbound traffic from the internet.

The default rules for inbound traffic are:

| Priority | Rule name                     | Description                                                               |
| -------- | ----------------------------- | ------------------------------------------------------------------------- |
| 65000    | AllowVnetInbound              | Allow inbound coming from any VM to any VM within the subnet.             |
| 65001    | AllowAzureLoadBalancerInbound | Allow traffic from the default load balancer to any VM within the subnet. |
| 65500    | DenyAllInBound                | Deny traffic from any external source to any of the VMs.                  |

The default rules for outbound traffic are:

| Priority | Rule name             | Description                                                                |
| -------- | --------------------- | -------------------------------------------------------------------------- |
| 65000    | AllowVnetOutbound     | Allow outbound going from any VM to any VM within the subnet.              |
| 65001    | AllowInternetOutbound | Allow outbound traffic going to the internet from any VM.                  |
| 65500    | DenyAllOutBound       | Deny traffic from any internal VM to a system outside the virtual network. |

#### Augmented security rules

You use augmented security rules for network security groups to simplify the management of large numbers of rules. Augmented security rules also help when you need to implement more complex network sets of rules. Augmented rules let you add the following options into a single security rule:

- multiple IP addresses
- multiple ports
- service tags
- application security groups
  Suppose your company wants to restrict access to resources in your datacenter, spread across several network address ranges. With augmented rules, you can add all these ranges into a single rule, reducing the administrative overhead and complexity in your network security groups.

#### Service tags

You use service tags to simplify network security group security even further. You can allow or deny traffic to a specific Azure service, either globally or per region.

Service tags simplify security for virtual machines and Azure virtual networks, by allowing you to restrict access by resources or services. Service tags represent a group of IP addresses, and help simplify the configuration of your security rules. For resources that you can specify by using a tag, you don't need to know the IP address or port details.

You can restrict access to many services. Microsoft manages the service tags (you can't create your own). Some examples of the tags are:

- VirtualNetwork - This tag represents all virtual network addresses anywhere in Azure, and in your on-premises network if you're using hybrid connectivity.
- AzureLoadBalancer - This tag denotes Azure's infrastructure load balancer. The tag translates to the virtual IP address of the host (168.63.129.16) where Azure health probes originate.
- Internet - This tag represents anything outside the virtual network address that is publicly reachable, including resources that have public IP addresses. One such resource is the Web Apps feature of Azure App Service.
- AzureTrafficManager - This tag represents the IP address for Azure Traffic Manager.
- Storage - This tag represents the IP address space for Azure Storage. You can specify whether traffic is allowed or denied. You can also specify if access is allowed only to a specific region, but you can't select individual storage accounts.
- SQL - This tag represents the address for Azure SQL Database, Azure Database for MySQL, Azure Database for PostgreSQL, and Azure SQL Data Warehouse services. You can specify whether traffic is allowed or denied, and you can limit to a specific region.
- AppService - This tag represents address prefixes for Azure App Service.

#### Application security groups

Application security groups let you configure network security for resources used by specific applications. You can group virtual machines logically, no matter what their IP address or subnet assignment.

Use application security groups within a network security group to apply a security rule to a group of resources. It's easier to deploy and scale up specific application workloads. You just add a new virtual machine deployment to one or more application security groups, and that virtual machine automatically picks up your security rules for that workload.

An application security group allows you to group network interfaces together. You can then use that application security group as a source or destination rule within a network security group.

For example, your company has a number of front-end servers in a virtual network. The web servers must be accessible over ports 80 and 8080. Database servers must be accessible over port 1433. You assign the network interfaces for the web servers to one application security group, and the network interfaces for the database servers to another application security group. You then create two inbound rules in your network security group. One rule allows HTTP traffic to all servers in the web server application security group. The other rule allows SQL traffic to all servers in the database server application security group.

Without application security groups, you'd need to create a separate rule for each virtual machine.

The key benefit of application security groups is that it makes administration easier. You can easily add and remove network interfaces to an application security group as you deploy or redeploy application servers. You can also dynamically apply new rules to an application security group, which are then automatically applied to all the virtual machines in that application security group.

#### When to use network security groups

As a best practice, you should always use network security groups to help protect your networked assets against unwanted traffic. Network security groups give you granular control access over the network layer, without the potential complexity of setting security rules for every virtual machine or virtual network.

### Exercise - Create and manage network security groups

As the solution architect for the manufacturing company, you now want to start moving the ERP application and database servers to Azure. As a first step, you're going to test out your network security plan, using two of your servers.

In this unit, you'll configure a network security group and security rules to restrict network traffic to specific servers. You want your application server to be able to connect to your database server over HTTP. You don't want the database server to be able to use HTTP to connect to the application server.

#### Create a virtual network and network security group

First, you'll create a resource group, the virtual network, and subnets for your server resources. You'll then create a network security group.

1. Open the Azure Cloud Shell in your browser, and log in to the directory with access to the subscription you want to create resources in. Use the Bash version of Cloud Shell.

2. Run the following command in the Cloud Shell to create a variable to store your resource group name, and a resource group for your resources. Replace <resource group name> with a name for your resource group, and <location> with the Azure region you'd like to deploy your resources in.

```Azure CLI
rg=<resource group name>

az group create --name $rg --location <location>
```

3. Run the following command in Azure Cloud Shell to create the ERP-servers virtual network and the Applications subnet.

```Azure CLI
az network vnet create \
    --resource-group $rg \
    --name ERP-servers \
    --address-prefix 10.0.0.0/16 \
    --subnet-name Applications \
    --subnet-prefix 10.0.0.0/24
```

4. Run the following command in Cloud Shell to create the Databases subnet.

```Azure CLI
az network vnet subnet create \
    --resource-group $rg \
    --vnet-name ERP-servers \
    --address-prefix 10.0.1.0/24 \
    --name Databases
```

5. Run the following command in Cloud Shell to create the ERP-SERVERS-NSG network security group.

```Azure CLI
az network nsg create \
    --resource-group $rg \
    --name ERP-SERVERS-NSG
```

#### Create virtual machines running Ubuntu

Next, you create two virtual machines called **AppServer** and **DataServer**. You deploy AppServer to the Applications subnet, and DataServer to the Databases subnet. Add the virtual machine network interfaces to the `ERP-SERVERS-NSG` network security group. Then use these virtual machines to test your network security group.

1. Run the following command in Cloud Shell to build the AppServer virtual machine. Define a <password> for the admin account.

```Azure CLI
wget -N https://raw.githubusercontent.com/MicrosoftDocs/mslearn-secure-and-isolate-with-nsg-and-service-endpoints/master/cloud-init.yml && \
az vm create \
    --resource-group $rg \
    --name AppServer \
    --vnet-name ERP-servers \
    --subnet Applications \
    --nsg ERP-SERVERS-NSG \
    --image UbuntuLTS \
    --size Standard_DS1_v2 \
    --admin-username azureuser \
    --custom-data cloud-init.yml \
    --no-wait \
    --admin-password <password>
```

2.  Run the following command in Cloud Shell to build the DataServer virtual machine. Define a <password> for the admin account.

```Azure CLI
az vm create \
    --resource-group $rg \
    --name DataServer \
    --vnet-name ERP-servers \
    --subnet Databases \
    --nsg ERP-SERVERS-NSG \
    --size Standard_DS1_v2 \
    --image UbuntuLTS \
    --admin-username azureuser \
    --custom-data cloud-init.yml \
    --admin-password <password>
```

3. It can take several minutes for the virtual machines to be in a running state. To confirm that the virtual machines are running, run the following command in Cloud Shell.

```Azure CLI
az vm list \
    --resource-group $rg \
    --show-details \
    --query "[*].{Name:name, Provisioned:provisioningState, Power:powerState}" \
    --output table
```

When virtual machine creation is complete, you should see the following output.

```Output
Name        Provisioned    Power
----------  -------------  ----------
AppServer   Succeeded      VM running
DataServer  Succeeded      VM running
```

#### Check default connectivity

Now, you'll try to open a Secure Shell (SSH) session to each of your virtual machines. Remember, so far you've deployed a network security group with default rules.

1. To connect to your virtual machines, use SSH directly from Cloud Shell. To do this, you need the public IP addresses that have been assigned to your virtual machines. Run the following command in Cloud Shell to list the IP addresses that you'll use to connect to the virtual machines.

```Azure CLI
az vm list \
    --resource-group $rg \
    --show-details \
    --query "[*].{Name:name, PrivateIP:privateIps, PublicIP:publicIps}" \
    --output table
```

2. To make it easier to connect to your virtual machines during the rest of this exercise, assign the public IP addresses to variables. Run the following command in Cloud Shell to save the public IP address of AppServer and DataServer to a variable.

```Bash
APPSERVERIP="$(az vm list-ip-addresses \
                 --resource-group $rg \
                 --name AppServer \
                 --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
                 --output tsv)"

DATASERVERIP="$(az vm list-ip-addresses \
                 --resource-group $rg \
                 --name DataServer \
                 --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
                 --output tsv)"
```

3. Run the following command in Cloud Shell to check whether you can connect to your AppServer virtual machine.

```Bash
ssh azureuser@$APPSERVERIP -o ConnectTimeout=5
```

You'll get a Connection timed out message.

4. Run the following command in Cloud Shell to check whether you can connect to your DataServer virtual machine.

```Bash
ssh azureuser@$DATASERVERIP -o ConnectTimeout=5
```

Remember that the default rules deny all inbound traffic into a virtual network, unless this traffic is coming from another virtual network. The Deny All Inbound rule blocked the inbound SSH connections you just attempted.

Inbound

| Name               | Priority | Source IP       | Destination IP        | Access |
| ------------------ | -------- | --------------- | --------------------- | ------ |
| Allow VNet Inbound | 65000    | VIRTUAL_NETWORK | VIRTUAL_NETWORK Allow |
| Deny All Inbound   | 65500    | \*              | \*                    | Deny   |

#### Create a security rule for SSH

As you've now experienced, the default rules in your ERP-SERVERS-NSG network security group include a Deny All Inbound rule. You'll now add a rule so that you can use SSH to connect to AppServer and DataServer.

1. Run the following command in Cloud Shell to create a new inbound security rule to enable SSH access.

```Azure CLI
az network nsg rule create \
    --resource-group $rg \
    --nsg-name ERP-SERVERS-NSG \
    --name AllowSSHRule \
    --direction Inbound \
    --priority 100 \
    --source-address-prefixes '*' \
    --source-port-ranges '*' \
    --destination-address-prefixes '*' \
    --destination-port-ranges 22 \
    --access Allow \
    --protocol Tcp \
    --description "Allow inbound SSH"
```

2. Run the following command in Cloud Shell to check whether you can now connect to your AppServer virtual machine.

```Bash
ssh azureuser@$APPSERVERIP -o ConnectTimeout=5
```

The network security group rule might take a minute or two to take effect. If you receive a connection failure message, try again.

3. You should now be able to connect. After the Are you sure you want to continue connecting (yes/no)? message, type yes.

4. Enter the password you used when you created the virtual machine.

5. Type exit to close the AppServer session.

6. Run the following command in Cloud Shell to check whether you can now connect to your DataServer virtual machine.

```Bash
ssh azureuser@$DATASERVERIP -o ConnectTimeout=5
```

7.You should now be able to connect. After the Are you sure you want to continue connecting (yes/no)? message, type yes.

8. Enter the password you used when you created the virtual machine.

9. Type exit to close the DataServer session.

#### Create a security rule to prevent web access

Now add a rule so that AppServer can communicate with DataServer over HTTP, but DataServer can't communicate with AppServer over HTTP. These are the internal IP addresses for these servers:

| Server name | IP address |
| ----------- | ---------- |
| AppServer   | 10.0.0.4   |
| DataServer  | 10.0.1.4   |

1. Run the following command in Cloud Shell to create a new inbound security rule to deny HTTP access over port 80.

```Azure CLI
az network nsg rule create \
    --resource-group $rg \
    --nsg-name ERP-SERVERS-NSG \
    --name httpRule \
    --direction Inbound \
    --priority 150 \
    --source-address-prefixes 10.0.1.4 \
    --source-port-ranges '*' \
    --destination-address-prefixes 10.0.0.4 \
    --destination-port-ranges 80 \
    --access Deny \
    --protocol Tcp \
    --description "Deny from DataServer to AppServer on port 80"
```

#### Test HTTP connectivity between virtual machines

Here, you check if your new rule works. AppServer should be able to communicate with DataServer over HTTP. DataServer shouldn't be able to communicate with AppServer over HTTP.

1. Run the following command in Cloud Shell to connect to your AppServer virtual machine, and check if AppServer can communicate with DataServer over HTTP.

```Bash
ssh -t azureuser@$APPSERVERIP 'wget http://10.0.1.4; exit; bash'
```

2. Enter the password you used when you created the virtual machine.

3. The response should include a 200 OK message.

4. Run the following command in Cloud Shell to connect to your DataServer virtual machine, and check if DataServer can communicate with AppServer over HTTP.

```Bash
ssh -t azureuser@$DATASERVERIP 'wget http://10.0.0.4; exit; bash'
```

5. Enter the password you used when you created the virtual machine.

6. This shouldn't succeed, because you've blocked access over port 80. After several minutes, you should get a Connection timed out message. Press Ctrl+C to stop the command prior to the timeout.

#### Deploy an application security group

Next, create an application security group for database servers, so that all servers in this group can be assigned the same settings. You're planning to deploy more database servers, and want to prevent these servers from accessing application servers over HTTP. By assigning sources in the application security group, you don't need to manually maintain a list of IP addresses in the network security group. Instead, you assign the network interfaces of the virtual machines you want to manage to the application security group.

1. Run the following command in Cloud Shell to create a new application security group called ERP-DB-SERVERS-ASG.

```Azure CLI
az network asg create \
    --resource-group $rg \
    --name ERP-DB-SERVERS-ASG
```

2. Run the following command in Cloud Shell to associate DataServer with the application security group.

```Azure CLI
az network nic ip-config update \
    --resource-group $rg \
    --application-security-groups ERP-DB-SERVERS-ASG \
    --name ipconfigDataServer \
    --nic-name DataServerVMNic \
    --vnet-name ERP-servers \
    --subnet Databases
```

3. Run the following command in Cloud Shell to update the HTTP rule in the ERP-SERVERS-NSG network security group. It should reference the ERP-DB-Servers application security group.

```Azure CLI
az network nsg rule update \
    --resource-group $rg \
    --nsg-name ERP-SERVERS-NSG \
    --name httpRule \
    --direction Inbound \
    --priority 150 \
    --source-address-prefixes "" \
    --source-port-ranges '*' \
    --source-asgs ERP-DB-SERVERS-ASG \
    --destination-address-prefixes 10.0.0.4 \
    --destination-port-ranges 80 \
    --access Deny \
    --protocol Tcp \
    --description "Deny from DataServer to AppServer on port 80 using application security group"
```

#### Test the updated HTTP security rule

1. Run the following command in Cloud Shell to connect to your AppServer virtual machine, and check if AppServer can communicate with DataServer over HTTP.

```Bash
ssh -t azureuser@$APPSERVERIP 'wget http://10.0.1.4; exit; bash'
```

2. Enter the password you used when you created the virtual machine.

3. As before, the response should include a 200 OK message. The application security group settings can take a minute or two to take effect. If you don't initially receive the 200 OK message, wait a minute and try again.

4. Run the following command in Cloud Shell to connect to your DataServer virtual machine, and check if DataServer can communicate with AppServer over HTTP.

```Bash
ssh -t azureuser@$DATASERVERIP 'wget http://10.0.0.4; exit; bash'
```

5. Enter the password you used when you created the virtual machine.

6. As before, this shouldn't succeed, because you've blocked access over port 80. After several minutes, you should get a Connection timed out message. Press Ctrl+C to stop the command prior to the timeout.

You've now confirmed that your network security group rule works using an application security group, in the same way as when you used a source IP address. If we were to add additional data servers, we can easily ensure they have the proper network security by adding the new servers to the ERP-DB-SERVERS-ASG.

### Secure network access to PaaS services with virtual network service endpoints

You've migrated your existing application and database servers for your ERP system to Azure as virtual machines. Now you're considering using some Azure platform as a service (PaaS) services to reduce your costs and administrative requirements. Storage services will hold certain large file assets, such as engineering diagrams. These engineering diagrams have proprietary information in them, and must remain secure from unauthorized access. These files must only be accessible from specific systems.

In this unit, you'll look at how to use virtual network service endpoints for securing supported Azure services.

#### Virtual network service endpoints

Use virtual network service endpoints to extend your private address space in Azure by providing a direct connection to your Azure services. Service endpoints let you secure your Azure resources to only your virtual network. Service traffic will remain on the Azure backbone, and doesn't go out to the internet.

By default, Azure services are all designed for direct internet access. All Azure resources have public IP addresses, including PaaS services such as Azure SQL Database and Azure Storage. Because these services are exposed to the internet, anyone can potentially access your Azure services.

Service endpoints can connect certain PaaS Services directly to your private address space in Azure, so they act like they’re on the same virtual network. You use your private address space to access the PaaS services directly. Adding service endpoints doesn't remove the public endpoint. It simply provides a redirection of traffic.

Azure service endpoints are available for many services, such as:

- Azure Storage
- Azure SQL Database
- Azure Cosmos DB
- Azure Key Vault
- Azure Service Bus
- Azure Data Lake

For a service like SQL Database, which can't be accessed until you add IP addresses to its firewall, service endpoints should still be considered. Using a service endpoint for SQL Database restricts access to specific virtual networks, providing greater isolation and reducing the attack surface.

#### How service endpoints work

To enable a service endpoint, you must do two things:

1. Turn off public access to the service.
2. Add the service endpoint to a virtual network.

When you enable a service endpoint, you restrict the flow of traffic, and allow your Azure virtual machines to access the service directly from your private address space. Devices cannot access the service from a public network. On a deployed virtual machine vNIC, if you look at Effective routes, you'll notice the service endpoint as the Next Hop Type.

This is an example route table, before enabling a service endpoint:

| SOURCE  | STATE  | ADDRESS PREFIXES | NEXT HOP TYPE |
| ------- | ------ | ---------------- | ------------- |
| Default | Active | 10.1.1.0/24      | VNet          |
| Default | Active | 0.0.0.0./0       | Internet      |
| Default | Active | 10.0.0.0/8       | None          |
| Default | Active | 100.64.0.0./10   | None          |
| Default | Active | 192.168.0.0/16   | None          |

And here's an example route table after you've added two service endpoints to the virtual network:

| SOURCE  | STATE  | ADDRESS PREFIXES        | NEXT HOP TYPE                 |
| ------- | ------ | ----------------------- | ----------------------------- |
| Default | Active | 10.1.1.0/24             | VNet                          |
| Default | Active | 0.0.0.0./0              | Internet                      |
| Default | Active | 10.0.0.0/8              | None                          |
| Default | Active | 100.64.0.0./10          | None                          |
| Default | Active | 192.168.0.0/16          | None                          |
| Default | Active | 20.38.106.0/23, 10 more | VirtualNetworkServiceEndpoint |
| Default | Active | 20.150.2.0/23, 9 more   | VirtualNetworkServiceEndpoint |

#### Service endpoints and hybrid networks

Service resources that you've secured by using virtual network service endpoints are not, by default, accessible from on-premises networks. To access resources from an on-premises network, use NAT IPs. If you use ExpressRoute for connectivity from on-premises to Azure, you have to identify the NAT IP addresses that are used by ExpressRoute. By default, each circuit uses two NAT IP addresses to connect to the Azure backbone network. You then need to add these IP addresses into the IP firewall configuration of the Azure service resource (for example, Azure Storage).

This diagram shows how you can use a service endpoint and firewall configuration to enable on-premises devices to access Azure Storage resources.

### Exercise - Restrict access to Azure Storage by using service endpoints

As the solution architect, you're planning to move sensitive engineering diagram files into Azure Storage. The files must only be accessible from computers inside the corporate network. You want to create a virtual network service endpoint for Azure Storage to secure the connectivity to your storage accounts.

In this unit, you'll create a service endpoint, and use network rules to restrict access to Azure Storage. You'll create a virtual network service endpoint for Azure Storage on the Databases subnet. You'll then verify that your DataServer virtual machine can access Azure Storage. Finally, you'll check that the AppServer virtual machine, which is on a different subnet, can't access storage.

#### Add rules to the network security group

Ensure that communications with Azure Storage pass through the service endpoint. Add outbound rules to allow access to the Storage service, but deny all other internet traffic.

1. Run the following command in Azure Cloud Shell to create an outbound rule to allow access to Storage.

```Azure CLI
az network nsg rule create \
    --resource-group $rg \
    --nsg-name ERP-SERVERS-NSG \
    --name Allow_Storage \
    --priority 190 \
    --direction Outbound \
    --source-address-prefixes "VirtualNetwork" \
    --source-port-ranges '*' \
    --destination-address-prefixes "Storage" \
    --destination-port-ranges '*' \
    --access Allow \
    --protocol '*' \
    --description "Allow access to Azure Storage"
```

2. Run the following command in Cloud Shell to create an outbound rule to deny all internet access.

```Azure CLI
az network nsg rule create \
    --resource-group $rg \
    --nsg-name ERP-SERVERS-NSG \
    --name Deny_Internet \
    --priority 200 \
    --direction Outbound \
    --source-address-prefixes "VirtualNetwork" \
    --source-port-ranges '*' \
    --destination-address-prefixes "Internet" \
    --destination-port-ranges '*' \
    --access Deny \
    --protocol '*' \
    --description "Deny access to Internet."
```

You should now have the following rules in ERP-SERVERS-NSG:

| Rule name     | Direction | Priority | Purpose                                 |
| ------------- | --------- | -------- | --------------------------------------- |
| AllowSSHRule  | Inbound   | 100      | Allow inbound SSH                       |
| httpRule      | Inbound   | 150      | Deny from DataServer to AppServer on 80 |
| Allow_Storage | Outbound  | 190      | Allow access to Azure Storage           |
| Deny_Internet | Outbound  | 200      | Deny access to Internet from VNet       |

At this point, both AppServer and DataServer have access to the Azure Storage service.

#### Configure storage account and file share

In this step, you'll create a new storage account, and then add an Azure file share to this account. This share is where you'll store your engineering diagrams.

1. Run the following command in Cloud Shell to create a storage account for engineering documents.

```Bash
STORAGEACCT=$(az storage account create \
                --resource-group $rg \
                --name engineeringdocs$RANDOM \
                --sku Standard_LRS \
                --query "name" | tr -d '"')
```

2. Run the following command in Cloud Shell to store the primary key for your storage in a variable.

```Bash
STORAGEKEY=$(az storage account keys list \
                --resource-group $rg \
                --account-name $STORAGEACCT \
                --query "[0].value" | tr -d '"')
```

3. Run the following command in Cloud Shell to create an Azure file share called erp-data-share.

```Azure CLI
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "erp-data-share"
```

#### Enable the service endpoint

You now need to configure the storage account to be accessible only from database servers, by assigning the storage endpoint to the Databases subnet. You then add a security rule to the storage account.

1. Run the following command in Cloud Shell to assign the Microsoft.Storage endpoint to the subnet.

```Azure CLI
az network vnet subnet update \
    --vnet-name ERP-servers \
    --resource-group $rg \
    --name Databases \
    --service-endpoints Microsoft.Storage
```

2. Run the following command to deny all access to change the default action to Deny. After network access is denied, the storage account is not accessible from any network.

```Azure CLI
az storage account update \
    --resource-group $rg \
    --name $STORAGEACCT \
    --default-action Deny
```

3. Run the following command in Cloud Shell to restrict access to the storage account. By default, storage accounts are open to accept all traffic. You want only traffic from the Databases subnet to be able to access the storage.

```Azure CLI
az storage account network-rule add \
    --resource-group $rg \
    --account-name $STORAGEACCT \
    --vnet ERP-servers \
    --subnet Databases
```

#### Test access to storage resources

In this step, you'll connect to both of your servers, and verify that only DataServer has access to the Azure file share on the storage account.

1. Run the following command in Cloud Shell to save the public IP addresses of AppServer and DataServer to variables.

```Bash
APPSERVERIP="$(az vm list-ip-addresses \
                    --resource-group $rg \
                    --name AppServer \
                    --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
                    --output tsv)"

DATASERVERIP="$(az vm list-ip-addresses \
                    --resource-group $rg \
                    --name DataServer \
                    --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
                    --output tsv)"
```

2. Run the following command in Cloud Shell to connect to your AppServer virtual machine, and attempt to mount the Azure file share

```Bash
ssh -t azureuser@$APPSERVERIP \
    "mkdir azureshare; \
    sudo mount -t cifs //$STORAGEACCT.file.core.windows.net/erp-data-share azureshare \
    -o vers=3.0,username=$STORAGEACCT,password=$STORAGEKEY,dir_mode=0777,file_mode=0777,sec=ntlmssp; findmnt \
    -t cifs; exit; bash"
```

3. Enter the password you used when you created the virtual machine.

4. The response should include a mount error message. This connection isn't allowed, because there is no service endpoint for the storage account on the Applications subnet.

5. Run the following command in Cloud Shell to connect to your DataServer virtual machine, and attempt to mount the Azure file share.

```Bash
ssh -t azureuser@$DATASERVERIP \
    "mkdir azureshare; \
    sudo mount -t cifs //$STORAGEACCT.file.core.windows.net/erp-data-share azureshare \
    -o vers=3.0,username=$STORAGEACCT,password=$STORAGEKEY,dir_mode=0777,file_mode=0777,sec=ntlmssp;findmnt \
    -t cifs; exit; bash"
```

6.Enter the password you used when you created the virtual machine.

7. The mount should be successful, and the response should include details of the mount point. This is allowed because you created the service endpoint for the storage account on the Databases subnet.

You've now verified that DataServer can access storage, by using the storage service endpoint on the Databases subnet. You've also verified that AppServer can't access storage. This is because this server is on a different subnet, and doesn't have access to the virtual network service endpoint.

#### Summary

You've learned about isolating and securing network resources in Azure. You now know how to use network security groups to secure virtual networks and virtual machines by creating rules to control network traffic. You've also learned how to use network service endpoints to control traffic to services such as Azure Storage and Azure SQL Database.

You can now use network security groups and service endpoints to ensure that network access to your Azure virtual machines and services is properly secured.

## Protect your virtual machines by using Azure Backup

### Introduction

Your company has several critical virtual machine workloads running on Azure. As the lead solution architect, you've been asked to ensure that the company can recover these virtual machines if there's data loss or corruption. You've been asked to use the built-in capabilities of Azure Backup to help protect these virtual machines.

Azure Backup is a service that allows you to back up Azure virtual machines, on-premises servers, Azure file shares, and SQL Server or SAP HANA running on Azure VMs, and other application workloads.

In this module, you'll learn about Azure Backup. And you'll see how you can use the Azure portal to back up and restore a machine.

### Learning objectives

In this module, you'll:

- Identify the scenarios for which Azure Backup provides backup and restore capabilities
- Back up and restore an Azure virtual machine

### Prerequisites

- Basic knowledge of Azure virtual machines
- Basic knowledge of disk storage for virtual machines

### Azure Backup features and scenarios

To address your company's business continuity and disaster recovery (BCDR) plan, there must be a full backup and restore capability for all of your high-risk servers. You've been asked to enable and test backup and restore functionality for these critical Windows and Linux assets.

In this unit, you'll look at how Azure Backup works, and study some of the supported use cases for Azure Backup.

### What is Azure Backup?

Azure Backup is a built-in Azure service that provides secure backup for all Azure-managed data assets. It uses zero-infrastructure solutions to enable self-service backups and restores, with at-scale management at a lower and predictable cost. At present, Azure Backup offers specialized backup solutions for Azure and on-premises virtual machines. Azure Backup also enables workloads like SQL Server or SAP HANA running in Azure VMs to have enterprise-class backup and restore options.

In contrast to traditional backup solutions that can take considerable effort to set up, Azure Backup is easily managed through the Azure portal.

#### Azure Backup versus Azure Site Recovery

Both Backup and Site Recovery aim to make the system more resilient to faults and failures. However, while the primary goal of backup is to maintain copies of stateful data that allow you to go back in time, site-recovery replicates the data in almost real time and allows for a failover.

In that sense, if there are issues like network or power outages, you can use availability zones. For a region-wide disaster (such as natural disasters), Site Recovery is used. Backups are used in cases of accidental data loss, data corruption, or ransomware attacks.

Additionally, the choice of a recovery approach depends on the criticality of the application, RPO and RTO requirements, and the cost implications.

#### Why use Azure Backup?

Traditional backup solutions, such as disk and tape, don't offer the highest level of integration with cloud-based solutions. Azure Backup has several benefits over more traditional backup solutions:

**Zero-infrastructure backup**: Azure Backup eliminates the need to deploy and manage any backup infrastructure or storage. This means there's no overhead in maintaining backup servers or scaling the storage up or down as the needs vary.

**Long-term retention**: Meet rigorous compliance and audit needs by retaining backups for many years, beyond which the recovery points will be pruned automatically by the built-in lifecycle management capability.

**Security**: Azure Backup provides security to your backup environment - both when your data is in transit and at rest.

- **Azure role-based access control**: RBAC allows you to segregate duties within your team and grant only the amount of access to users necessary to do their jobs.

- **Encryption of backups**: Backup data is automatically encrypted using Microsoft-managed keys. Alternatively, you can also encrypt your backed-up data using customer-managed keys stored in the Azure Key Vault.
- **No internet connectivity required**: When using Azure VMs, all the data transfer happens only on the Azure backbone network without needing to access your virtual network. So no access to any IPs or FQDNs is required.

- **Soft delete**: With soft delete, the backup data is retained for 14 additional days even after the deletion of the backup item. This protects against accidental deletion or malicious deletion scenarios, allowing the recovery of those backups with no data loss.

Azure Backup also offers the ability to back up virtual machines encrypted with Azure Disk Encryption.

**High availability**: Azure Backup offers three types of replication - LRS, GRS, and RA-GRS (to enable customer-controlled cross region restore) to keep your backup data highly available.

**Centralized monitoring and management**: Azure Backup provides built-in monitoring and alerting capabilities in a Recovery Services vault. These capabilities are available without any additional management infrastructure.

#### Azure Backup supported scenarios

\***Azure VMs**- Back up Windows or Linux Azure virtual machines
Azure Backup provides independent and isolated backups to guard against unintended destruction of the data on your VMs. Backups are stored in a Recovery Services vault with built-in management of recovery points. Configuration and scaling are simple, backups are optimized, and you can easily restore as needed.

- **On-premises** - Back up files, folders, and system state using the Microsoft Azure Recovery Services (MARS) agent . Or use Azure Backup Server (MABS) or Data Protection Manager (DPM) server to protect on-premises VMs (Hyper-V and VMWare) and other on-premises workloads.
- **Azure Files shares** - Azure Files - Snapshot management by Azure Backup
- **SQL Server in Azure VMs and SAP HANA databases in Azure VMs** - Azure Backup offers stream-based, specialized solutions to back up SQL Server or SAP HANA running in Azure VMs. These solutions take workload-aware backups that support different backup types such as full, differential and log, 15-minute RPO, and point-in-time recovery.

### Back up an Azure virtual machine by using Azure Backup

You want to ensure that the backup and restore jobs you put in place offer a way to recover your company's servers. With this requirement in mind, you want to investigate the best way to implement backup for your virtual machines.

Virtual machines that are hosted on Azure can take advantage of Azure Backup. You can easily back up and restore machines without installing additional software.

In this unit, you'll explore all the methods of backing up Azure virtual machines provided by Azure Backup and make a decision on which to implement.

Azure VMs are backed up by taking snapshots of the underlying disks at user-defined intervals and transferring those snapshots to the Recovery Services Vault as per the customer-defined policy.

#### Recovery Services vault

Azure Backup uses a Recovery Services vault to manage and store the backup data. A vault is a storage-management entity, which provides a simple experience to carry out and monitor backup and restore operations. With Azure Backup, you need not worry about deploying or managing storage accounts. In fact, all you need to specify is the vault you want to back up the VM to. The backup data is transferred to the Azure Backup storage accounts (in a separate fault domain) in the background. The vault also acts as an RBAC boundary to allow secure access to the data.

#### Snapshots

A snapshot is a point-in-time backup of all disks on the virtual machine. For Azure virtual machines, Azure Backup uses different extensions for each supporting operating system:

| Extension       | OS      | Description                                                                                                 |
| --------------- | ------- | ----------------------------------------------------------------------------------------------------------- |
| VMSnapshot      | Windows | The extension works with Volume Shadow Copy Service (VSS) to take a copy of the data on disk and in memory. |
| VMSnapshotLinux | Linux   | The snapshot is a copy of the disk.                                                                         |

Depending on how the snapshot is taken and what it includes, you can achieve different levels of consistency:

- Application consistent
  - The snapshot captures the virtual machine as a whole. It uses VSS writers to capture the content of the machine memory and any pending I/O operations.
  - For Linux machines, you'll need to write custom pre or post scripts per app to capture the application state.
  - You can get complete consistency for the virtual machine and all running applications.
- File system consistent
  - If VSS fails on Windows, or the pre and post scripts fail on Linux, Azure Backup will still create a file-system-consistent snapshot.
  - During a recovery, no corruption occurs within the machine. But installed applications need to do their own cleanup during startup to become consistent.
- Crash consistent
- This level of consistency typically occurs if the virtual machine is shut down at the time of the backup.
- No I/O operations or memory contents are captured during this type of backup. This method doesn't guarantee data consistency for the OS or app.

#### Backup policy

You can define the backup frequency and retention duration for your backups. Currently, the VM backup can be triggered daily or weekly, and can be stored for multiple years. The backup policy supports two access tiers - snapshot tier and the vault tier.

**Snapshot tier**: All the snapshots are stored locally for a maximum period of five days. This is referred to as the snapshot tier. For all types of operation recoveries, it's recommended that you restore from the snapshots since it's much faster to do so. This capability is called Instant Restore.

**Vault tier**: All snapshots are additionally transferred to the vault for additional security and longer retention. At this point, the recovery point type changes to “snapshot and vault”.

#### Backup process for an Azure virtual machine

Here's how Azure Backup completes a backup for Azure VMs:

1. For Azure VMs that are selected for backup, Azure Backup starts a backup job according to the backup frequency you specify in the backup policy.

2. During the first backup, a backup extension is installed on the VM, if the VM is running.

- For Windows VMs, the VMSnapshot extension is installed.
- For Linux VMs, the VMSnapshotLinux extension is installed.

3. After the snapshot is taken, it's stored locally as well transferred to the vault.

- The backup is optimized by backing up each VM disk in parallel.
- For each disk that's being backed up, Azure Backup reads the blocks on the disk and identifies and transfers only the data blocks that changed (the delta) since the previous backup.
- Snapshot data might not be immediately copied to the vault. It might take several hours at peak times. Total backup time for a VM will be less than 24 hours for daily backup policies.

### Exercise - Back up an Azure virtual machine

Your company is running a combination of Windows and Linux workloads. You've been asked to prove that Azure Backup is a good fit for both kinds of virtual machines. By using a combination of the Azure CLI and the Azure portal, you'll help protect both kinds of virtual machines with Azure Backup.

Azure Backup can be quickly enabled for virtual machines in Azure. You can enable Azure Backup from the portal, from the Azure CLI, or by using PowerShell commands.

In this exercise, you'll create a virtual machine, set up a backup, and start a backup.

#### Create a backup for Azure virtual machines

##### Set up the environment

1. Sign in to the Azure portal , and open Azure Cloud Shell.

2. Create a resource group to contain all the resources for this exercise.

```Azure CLI
RGROUP=$(az group create --name vmbackups --location westus2 --output tsv --query name)
```

3. Use Cloud Shell to create the NorthwindLive virtual network and the NorthwindInternal1 subnet.

```Azure CLI
az network vnet create \
    --resource-group $RGROUP \
    --name NorthwindInternal \
    --address-prefix 10.0.0.0/16 \
    --subnet-name NorthwindInternal1 \
    --subnet-prefix 10.0.0.0/24
```

##### Create a Windows virtual machine by using the Azure CLI

Create the `NW-APP01` virtual machine by using the following command. Replace <password> with a password of your choice.

```Azure CLI
az vm create \
    --resource-group $RGROUP \
    --name NW-APP01 \
    --size Standard_DS1_v2 \
    --vnet-name NorthwindInternal \
    --subnet NorthwindInternal1 \
    --image Win2016Datacenter \
    --admin-username admin123 \
    --no-wait \
    --admin-password <password>
```

##### Create a Linux virtual machine by using the Azure CLI

Create the NW-RHEL01 virtual machine by using the following command.

```Azure CLI
az vm create \
    --resource-group $RGROUP \
    --name NW-RHEL01 \
    --size Standard_DS1_v2 \
    --image RedHat:RHEL:7-RAW:latest \
    --authentication-type ssh \
    --generate-ssh-keys \
    --vnet-name NorthwindInternal \
    --subnet NorthwindInternal1
```

##### Enable backup for a virtual machine by using the Azure portal

1. In the Azure portal, search for and select Virtual machines.
2. From the list, select the `NW-RHEL01` virtual machine that you created.
3. In the sidebar, scroll down to Operations, select Backup, and then use the following information to create a backup:

|                         |                                                                                                       |     |
| ----------------------- | ----------------------------------------------------------------------------------------------------- | --- |
| Recovery Services vault | Select Create new, and enter azure-backup for the name.                                               |
| Resource group          | Select the vmbackups resource group that you created earlier.                                         |
| Choose a backup policy  | Select (new) DailyPolicy, which is a daily backup at 12:00 PM UTC, and a retention range of 180 days. |

4. Select Enable Backup.

5. After the deployment finishes, select NW-RHEL01 from the list of virtual machines.

6. You can access backup settings from the virtual machine menu by scrolling down to Operations and selecting Backup.

7. To perform the first backup for this server, select Backup now.
8. On the Backup Now page, select OK.

##### Enable a backup by using the Azure CLI

1. By using Cloud Shell, enable a backup for the NW-APP01 virtual machine.

```Azure CLI
az backup protection enable-for-vm \
    --resource-group vmbackups \
    --vault-name azure-backup \
    --vm NW-APP01 \
    --policy-name DefaultPolicy
```

2. Monitor the progress of the setup by using the Azure CLI.

```Azure CLI
az backup job list \
    --resource-group vmbackups \
    --vault-name azure-backup \
    --output table
```

Keep running the preceding command until you see that ConfigureBackup has finished:

```Output
Name                                  Operation        Status      Item Name    Start Time UTC                    Duration
------------------------------------  ---------------  ----------  -----------  --------------------------------  --------------
a3df79b4-be4f-4cc9-8b2c-a5ead44a6a12  ConfigureBackup  Completed   NW-APP01     2019-08-01T06:19:12.101048+00:00  0:00:31.305975
5e1531a9-8b3d-4983-a642-86ee982f7036  Backup           InProgress  NW-RHEL01    2019-08-01T06:18:35.955118+00:00  0:01:22.734182
860d4dca-9603-4a4e-9f3b-93f242a0a64d  ConfigureBackup  Completed   NW-RHEL01    2019-08-01T06:13:33.860598+00:00  0:00:31.256773
```

3. Do an initial backup of the virtual machine, instead of waiting for the schedule to run it.

```Azure CLI
az backup protection backup-now \
    --resource-group vmbackups \
    --vault-name azure-backup \
    --container-name NW-APP01 \
    --item-name NW-APP01 \
    --retain-until 18-10-2030 \
    --backup-management-type AzureIaasVM
```

#### Monitor backups in the portal

View the status of a backup for a single virtual machine

1. Sign in to the Azure portal.

2. On the Azure portal menu or from the Home page, select All resources.

3. Select the NW-APP01 virtual machine.

4. Under Operations, select Backup

5. Last backup status displays the current status of the backup.

#### View the status of backups in the Recovery Services vault

1. Sign in to the Azure portal.

2. On the Azure portal menu or from the Home page, select All resources.

3. Select the azure-backup Recovery Services vault.

4. Select the Backup tab on the Overview page to see a summary of all the backup items, the storage being used, and the current status of any backup jobs.

### Restore virtual machine data

Companies that have a business continuity and disaster recovery (BCDR) plan typically schedule test runs to ensure that the business can successfully recover from disasters. Now that you have successfully backed up your VMs, you want to explore the options available for restoring them as part of your BCDR testing.

In this unit, you'll learn about the options for restoring an Azure VM from a previous backup.

#### Restore types

Azure Backup provides a number of ways to restore a VM. As explained earlier, you can either instantly restore from the snapshot tier (optimal for operational recoveries) or from the vault tier.

| Restore option                  | Details                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Create a new VM                 | Quickly creates and gets a basic VM up and running from a restore point. The new VM must be created in the same region as the source VM.                                                                                                                                                                                                                                                                                                                                              |
| Restore disk                    | Restores a VM disk, which can then be used to create a new VM. The disks are copied to the Resource Group you specify. Azure Backup provides a template to help you customize and create a VM. Alternatively, you can attach the disk to an existing VM, or create a new VM.<br>This option is useful if you want to customize the VM, add configuration settings that weren't there at the time of backup, or add settings that must be configured using the template or PowerShell. |
| Replace existing                | You can restore a disk and use it to replace a disk on the existing VM. Azure Backup takes a snapshot of the existing VM before replacing the disk and stores it in the staging location you specify. Existing disks connected to the VM are replaced with the selected restore point. The current VM must exist. If it's been deleted, this option can't be used.                                                                                                                    |
| Cross Region (secondary region) | Cross Region restore can be used to restore Azure VMs in the secondary region, which is an Azure paired region. <br>This feature is available for the options below: <br>Create a VM<br> Restore Disks <br> We don't currently support the Replace existing disks option.                                                                                                                                                                                                             |

#### Recover files from a backup

You can also recover individual files from a recovery point by mounting the snapshot on the target machine using the iSCSI initiator in the machine. The steps do so are listed [here](https://docs.microsoft.com/en-us/azure/backup/backup-azure-restore-files-from-vm).

#### Restore an encrypted virtual machine

Azure Backup supports the backup and restore of machines encrypted through Azure Disk Encryption. Disk Encryption works with Azure Key Vault to manage the relevant secrets that are associated with the encrypted disk. For an additional layer of security, you can use key vault encryption keys (KEKs) to encrypt the secrets before they're written to the key vault.

Certain limitations apply when you restore encrypted virtual machines:

- Azure Backup supports only standalone key encryption. Any key that's part of a certificate isn't supported currently.
- File-level or folder-level restores are not supported with encrypted virtual machines. To restore to that level of granularity, the whole virtual machine has to be restored. You can then manually copy the file or folders.
- The Replace existing VM option isn't available for encrypted virtual machines.

### Exercise - Restore Azure virtual machine data

A few days after you backed up your first Azure virtual machine, the server had issues. It needs to be restored from a backup. You want to restore the virtual machine's disk and attach it to the problematic live server, and then track the restore to ensure that it has finished successfully.

In this exercise, you'll see how to restore a successful backup to replace a VM that has become corrupted, and monitor its progress.

#### Restore a virtual machine in the Azure portal

##### Create a storage account to use as a staging location

1. Sign in to the Azure portal by using the same account that you used in the previous exercise.

2. In the Azure portal, search for and select Storage accounts.

3. Select + Add, and then use the following information to create a storage account:

|                      |                                                                                                |
| -------------------- | ---------------------------------------------------------------------------------------------- |
| Resource group       | Select vmbackups.                                                                              |
| Storage account name | Enter a unique name like restorestagingYYYYMMDD, where YYYYMMDD is replaced with today's date. |
| Location             | Select (US) West US 2.                                                                         |

4. Select Review + create.

5. On the Create storage account page, select Create.

6. Wait for the storage account to be deployed.

#### Stop the VM to allow for the restore

A backup can't be restored if the VM is allocated and running. If you forget to stop the VM, you'll see an error that's similar to the following example.

To prevent this error, use the following steps:

1. In the Azure portal, search for and select Virtual machines, and then select NW-APP01.
2. Select Stop to shut down the VM.
3. In the Stop this virtual machine dialog box, select OK.

#### Restore the VM

The Recovery Services vaults are accessible at the subscription level. When you're viewing the VM, Azure provides a quick link to the specific vault under Operations.

1. On the left menu, under Operations, select Backup.
2. To restore the virtual machine, select Restore VM.
3. Select the restore point to use for the recovery, and then select OK.
4. In the Restore Configuration window, select Replace Existing and use the following information to configure the restore:

|                  |                                                                                                         |
| ---------------- | ------------------------------------------------------------------------------------------------------- |
| Restore Type     | Select Replace Disk(s). This is the restore point that will be used to replace the existing VM's disks. |
| Staging Location | Select the storage account that you created previously.                                                 |

5. Select OK.

6. On the confirmation screen, select Restore.

#### Track a restore

1. At the top of the page, select View all Jobs.

2. Select the restore job. You can now monitor the progress of the VM restore:

- Job Progress: Real-time percentage of the job as a whole.
- Sub Tasks: Status of the current task within the job.

### Summary

In this module, you learned the importance of having a tested backup and recovery strategy for your organization. You learned about the different types of Azure backups, and the reasons why you would choose one backup type versus another depending on your scenario.

You learned how to back up an Azure VM. You then restored it by using the various options available to you, and you were able to monitor the progress.

You've seen that you can back up Azure virtual machines or on-premises machines. In addition, you learned how to back up an Azure VM. You then restored it by using the various options available to you, and you were able to monitor the progress.

You can now use Azure Backup to help protect your environment against data loss or disk corruption. You can restore services according to your business continuity and disaster recovery plan.

The sandbox automatically cleans up your resources when you're finished with this module.

When you're working in your own subscription, it's a good idea at the end of a project to identify whether you still need the resources you created. Resources left running can cost you money. You can delete resources individually or delete the resource group to delete the entire set of resources.

## Analyze your Azure infrastructure by using Azure Monitor logs

### Introduction

Logging and monitoring the health of your services is a vital component of production applications. You need to be able to determine the causes of failures. You also need to identify any problems before they occur.

Azure Monitor is an important tool to help you in this process. It enables you to gather monitoring and diagnostic information about the health of your services. You can use this information to visualize and analyze the causes of problems that might occur in your app.

Suppose that you work for the operations team of a large organization. The organization is running large-scale production apps in the cloud. The operations team wants to consolidate its log data in a single service to improve visibility across services and simplify its logging strategy.

The team began implementing Azure Monitor logs. It wants to fully understand how the logs work. It also wants to know what the capabilities are to query and evaluate the log data that's fed into the service.

#### Learning objectives

In this module, you will:

- Identify the features and capabilities of Azure Monitor logs.
- Create basic Azure Monitor log queries to extract information from log data.

### Features of Azure Monitor logs

Azure Monitor is a service for collecting and analyzing telemetry. It helps you get maximum performance and availability for your cloud applications, and for your on-premises resources and applications. It shows how your applications are performing and identifies any issues with them.

#### Data collection in Azure Monitor

Azure Monitor collects two fundamental types of data: metrics and logs. Metrics tell you how the resource is performing, and the other resources that it's consuming. Logs contain records that show when resources are created or modified.

The following diagram gives a high-level view of Azure Monitor. On the left are the sources of monitoring data: Azure, operating systems, and custom sources. At the center of the diagram are the data stores for metrics and logs. On the right are the functions that Azure Monitor performs with this collected data, such as analysis, alerting, and streaming to external systems.

Azure Monitor collects data automatically from a range of components. For example:

- **Application data**: Data that relates to your custom application code.
- **Operating system data**: Data from the Windows or Linux virtual machines that host your application.
- **Azure resource data**: Data that relates to the operations of an Azure resource, such as a web app or a load balancer.
- **Azure subscription data**: Data that relates to your subscription. It includes data about Azure health and availability. \***Azure tenant data**: Data about your Azure organization-level services, such as Azure Active Directory.

Because Azure Monitor is an automatic system, it begins to collect data from these sources as soon as you create Azure resources such as virtual machines and web apps. You can extend the data that Azure Monitor collects by:

- **Enabling diagnostics**: For some resources, such as Azure SQL Database, you receive full information about a resource only after you have enabled diagnostic logging for it. You can use the Azure portal, the Azure CLI, or PowerShell to enable diagnostics.
- **Adding an agent**: For virtual machines, you can install the Log Analytics agent and configure it to send data to a Log Analytics workspace. This agent increases the amount of information that's sent to Azure Monitor.

Your developers might also want to send data to Azure Monitor from custom code, such as a web app, an Azure function, or a mobile app. They send data by calling the Data Collector API. You communicate with this REST interface through HTTP. This interface is compatible with a variety of development frameworks, such as .NET Framework, Node.js, and Python. Developers can choose their favorite language and framework to log data in Azure Monitor.

#### Logs

Logs contain time-stamped information about changes made to resources. The type of information recorded varies by log source. The log data is organized into records, with different sets of properties for each type of record. The logs can include numeric values such as Azure Monitor metrics, but most include text data rather than numeric values.

The most common type of log entry records an event. Events can occur sporadically rather than at fixed intervals or according to a schedule. Events are created by applications and services, which provide the context for the events. You can store metric data in logs to combine them with other monitoring data for analysis.

You log data from Azure Monitor in a Log Analytics workspace. Azure provides an analysis engine and a rich query language. The logs show the context of any problems and are useful for identifying root causes.

#### Metrics

Metrics are numerical values that describe some aspect of a system at a point in time. Azure Monitor can capture metrics in near real time. The metrics are collected at regular intervals and are useful for alerting because of their frequent sampling. You can use a variety of algorithms to compare a metric to other metrics and observe trends over time.

Metrics are stored in a time-series database. This data store is most effective for analyzing time-stamped data. Metrics are suited for alerting and fast detection of issues. They can tell you about system performance. If needed, you can combine them with logs to identify the root cause of issues.

#### Analyzing logs by using Kusto

To retrieve, consolidate, and analyze data, you specify a query to run in Azure Monitor logs. You write a log query with the Kusto query language, which is also used by Azure Data Explorer.

Log queries can be tested in the Azure portal so you can work with them interactively. You typically start with basic queries and then progress to more advanced functions as your requirements become more complex.

In the Azure portal, you can create custom dashboards, which are targeted displays of resources and data. Each dashboard is built from a set of tiles. Each tile might show a set of resources, a chart, a table of data, or some custom text. Azure Monitor provides tiles that you can add to dashboards. For example, you might use a tile to display the results of a Kusto query in a dashboard.

In the example scenario, the operations team can consolidate its data by visualizing monitoring data such as charts and tables. These tools are effective for summarizing data and presenting it to different audiences.

By using Azure dashboards, you can combine various kinds of data, including both logs and metrics, into a single pane in the Azure portal. For example, you might want to create a dashboard that combines tiles that show a graph of metrics, a table of activity logs, charts from Azure Monitor, and the output of a log query.

### Create basic Azure Monitor log queries to extract information from log data

You use Azure Monitor log queries to extract information from log data. Querying is an important part of examining the log data that Azure Monitor captures.

In the example scenario, the operations team will use Azure Monitor log queries to examine the health of its system.

#### Write Azure Monitor log queries by using Log Analytics

You can find the Log Analytics tool in the Azure portal and use it to run sample queries or to create your own queries:

1. In the Azure portal, select **Monitor** in the left pane.

You see the Azure Monitor page and more options, including Activity Log, Alerts, Metrics, and Logs.

Select **Query** & **Analyze** Logs.

Here you can enter your query and see the output.

#### Write queries by using the Kusto language

You use the Kusto Query Language to query log information for your services running in Azure. A Kusto query is a read-only request to process data and return results. You state the query in plain text, by using a data-flow model that's designed to make the syntax easy to read, write, and automate. The query uses schema entities that are organized in a hierarchy similar to that of Azure SQL Database: databases, tables, and columns.

A Kusto query consists of a sequence of query statements, delimited by a semicolon `(;)`. At least one statement is a tabular expression statement. A tabular expression statement formats the data arranged as a table of columns and rows.

The syntax of a tabular expression statement has a tabular data flow from one tabular query operator to another, starting with a data source. A data source might be a table in a database, or an operator that produces data. The data then flows through a set of data transformation operators that are bound together with the pipe `(|)`delimiter.

For example, the following Kusto query has a single tabular expression statement. The statement starts with a reference to a table called `Events`. The database that hosts this table is implicit here, and is part of the connection information. The data for that table, stored in rows, is filtered by the value of the `StartTime` column. The data is filtered further by the value of the State column. The query then returns the count of the resulting rows.

```Kusto
Events
| where StartTime >= datetime(2018-11-01) and StartTime < datetime(2018-12-01)
| where State == "FLORIDA"
| count
```

> Note <br>
> The Kusto query language that Azure Monitor uses is case-sensitive. Language keywords are typically written in lowercase. When you're using names of tables or columns in a query, make sure to use the correct case.

Events, captured from the event logs of monitored computers, are just one type of data source. Azure Monitor provides many other types of data sources. For example, the `Heartbeat` data source reports the health of all computers that report to your Log Analytics workspace. You can also capture data from performance counters, and update management records.

The following example retrieves the most recent heartbeat record for each computer. The computer is identified by its IP address. In this example, the `summarize` aggregation with the `arg_max` function returns the record with the most recent value for each IP address.

```Kusto
Heartbeat
| summarize arg_max(TimeGenerated, *) by ComputerIP
```

### Exercise - Create basic Azure Monitor log queries to extract information from log data

#### Create basic Azure Monitor log queries to extract information from log data

Let's use the Azure Monitor Demo Logs pane to practice writing queries against sample data. This site is provided for demonstration with pre-populated sample data. The user interface is similar to the Azure portal, and you can use the Kusto language.

1. In your browser, open the Azure Monitor Demo Logs pane in the Azure portal.

2. Enter a basic query in the **Type your query here** box. The example query retrieves the details of the most recent 10 security events.

```Kusto
SecurityEvent
    | take 10
```

3. Select Run to execute the query and see the results. You can view each row in the results to get more information.

4. Sort the data by time, by running the following query.

```Kusto
SecurityEvent
    | top 10 by TimeGenerated
```

5. Enter a query by using a filter clause and a time range. This query fetches records that are more than 30 minutes old and that have a level of 10 or more.

```Kusto
SecurityEvent
    | where TimeGenerated < ago(30m)
    | where toint(Level) >= 10
```

6. Run the following example. This example searches the Events table for records from the Application event log for the last 24 hours.

```Kusto
Event
| where EventLog == "Application"
| where TimeGenerated > ago(24h)
```

7. Run the following query. This query displays the number of different computers that generated heartbeat events each week, for the last three weeks. The results are displayed as a bar chart.

```Kusto
Heartbeat
    | where TimeGenerated >= startofweek(ago(21d))
    | summarize dcount(Computer) by endofweek(TimeGenerated) | render barchart kind=default
```

In addition to writing queries from scratch, the operations team can also take advantage of predefined example queries in Azure Monitor Logs that answer common questions related to the health, availability, usage and performance of their resources. Use the **Time Range** parameter above the query editor to select **Last 24 hours** as the time period of concern. Navigate to the **Queries** tab in the left pane to view a list of the sample queries grouped by Category, Resource Type, Solution or Topic.

1. Leave the Group by setting on **Resource Type**, expand **Virtual Machine** and run the query called Distinct missing updates cross computers. This query returns a list missing Windows updates from Virtual Machines sending logs into the workspace.

2. Change the Group by setting to **Category**, expand **Azure Monitor** and run the query called Computers availability today. This query shows a time series chart with the number of unique IP addresses sending logs into the workspace each hour for the last day.

3. Change the Group by setting to Topic, expand **Firewall Logs**, run the query called **Network rule log data**. This query returns a list of firewall actions the details of the associated network flows.

You can see from the Kusto queries you used here that it's easy to target a query to a specific time window, event level, or event log type. The security team can easily examine heartbeats to identify when servers are unavailable, which might indicate a denial-of-service attack. If the team spots the time when a server was unavailable, it can query for events in the security log around that time to diagnose whether an attack caused the interruption. Additionally, pre-defined queries can also be used to evaluate the availability of VMs, identify missing Windows updates and review firewall logs to view denied network flows intended for the VMs of interest.

### Summary

In this module, you learned how to use Azure Monitor. You looked at Azure Monitor logs to extract valuable information about your infrastructure from log data by using queries. You performed these queries by using the Kusto query language.

You saw how to:

Explore the types of data that Azure Monitor collects.
Create Azure Monitor log queries to extract information from the log data.
You can now use Azure Monitor to analyze your environment and troubleshoot issues.
