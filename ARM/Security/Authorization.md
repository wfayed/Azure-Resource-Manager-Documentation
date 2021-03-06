> Azure Resource Manager Community Documentation - Beta Version

> Work in progress - This community driven documentation is considered to be in preview stage at this time. Documentation might contain errors, might not cover every aspect or might lack complete parts, even important parts. Please help us make this documentation better by [contributing](CONTRIBUTING.md) anything that you think would make it better.


---


# Authorization / Access Control

## Azure Active Directory Role-based Access Control


Azure Roles-Based Access Control (RBAC) enables fine-grained access management for Azure. Users, groups, and applications can be granted access to manage resources in the Azure subscription, using Azure Management Portal, Azure Command-Line tools and Azure Management APIs.
Access is granted by assigning the appropriate RBAC role to users, groups, and applications, at the right scope. Roles can be assigned at a subscription level, resource group level or specific resource level.

Azure RBAC has three basic roles that apply to all resource types: Owner, Contributor and Reader. In addition you can use [RBAC Built-In Roles](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-built-in-roles/) for role specifies operations, or you can create [Custom Roles](http://blogs.technet.com/b/ad/archive/2015/12/10/custom-roles-in-azure-rbac-is-now-ga.aspx).

Azure RBAC **Built-In roles** is a list of roles that can be assigned to users, groups, and services. You can’t modify the definition of Built-In roles. With RBAC **Custom-Roles** you can define custom roles by composing a set of actions from a list of available actions that can be performed on Azure resources. Currently you can create Custom-Roles using PowerShell or Azure CLI. The Custom-Role creation operation involves import of a JSON file with a list of approved actions for the specific role on a specific resource type, a best practice to build a custom role is to export the JSON file with the approved actions for a build-in role and modify it.

Each subscription in Azure belongs to one and only one directory, each resource group belongs to one and only one subscription, and each resource belongs to one and only one resource group. Access that you grant at parent scopes is inherited at child scopes.

Classic administrators (service-admin and co-admins) have full access to the Azure subscription. In the RBAC model, classic administrators are assigned the Owner role at the subscription scope. The finer-grained authorization model (Azure RBAC) is supported only by the [new management portal](https://portal.azure.com) and Azure Resource Manager APIs.

For more information and instructions how to grant access using the Azure Management Portal, see: [Azure Active Directory Role-based Access Control](https://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-configure/).

For granting access through command tools like Azure PowerShell, CLI or REST API see: [Managing access to resources](https://azure.microsoft.com/en-us/documentation/articles/resource-group-rbac/).

## Lock resources

As an administrator, there are scenarios where you will want to place a lock on a subscription, resource group or resource to prevent other users in your organization from committing write actions or accidentally deleting a critical resource. 

Azure Resource Manager provides the ability to restrict operations on resources through resource management locks. Locks are policies which enforce a lock level at a particular scope. The scope can be a subscription, resource group or resource. The lock level identifies the type of enforcement for the policy, which presently has two values – CanNotDelete and ReadOnly. CanNotDelete means authorized users can still read and modify resources, but they can't delete any of the restricted resources. ReadOnly means authorized users can only read from the resource, but they can't modify or delete any of the restricted resources.

Locks are different from using role-based access control to assign user permissions to perform certain actions. Unlike role-based access control, you use management locks to apply a restriction across all users and roles, and you typically apply the locks for only limited duration.

To create or delete management locks, you must have access to Microsoft.Authorization/* or Microsoft.Authorization/locks/* actions of the built-in roles, only Owner and User Access Administrator are granted with those actions.

When you apply a lock at a parent scope, all child resources inherit the same lock.
If you apply more than one lock to a resource, the most restrictive lock takes precedence. 

Locks can be created through an ARM template, REST API or PowerShell.

For more details see: [Lock resources with Azure Resource Manager](https://azure.microsoft.com/en-us/documentation/articles/resource-group-lock-resources/)

##  Customized policy

Azure Resource Manager now allows you to control access through custom policies. With policies, you can prevent users in your organization from breaking conventions that are needed to manage your organization's resources. 

You create policy definitions that describe the actions or resources that are specifically denied. You assign those policy definitions at the desired scope, such as the subscription, resource group, or an individual resource. 

There are a few key differences between policy and role-based access control, but the first thing to understand is that policies and RBAC work together. To be able to use policy, the user must be authenticated through RBAC. Unlike RBAC, policy is a default allow and explicit deny system. 

RBAC focuses on the actions a user can perform at different scopes. For example, a particular user is added to the contributor role for a resource group at the desired scope, so the user can make changes to that resource group. 

Policy focuses on resource actions at various scopes. For example, through policies, you can control the types of resources that can be provisioned or restrict the locations in which the resources can be provisioned.

Policy definition is created using JSON. It consists of one or more conditions/logical operators which define the actions and an effect which tells what happens when the conditions are fulfilled.

Basically, a policy contains the following:

* Condition/Logical operators: It contains a set of conditions which can be manipulated through a set of logical operators.

* Effect: This describes what the effect will be when the condition is satisfied – either deny or audit. An audit effect will emit a warning event service log. 

 
Policies can be applied at different scopes like subscription, resource groups and individual resources. Policies are inherited by all child resources. So if a policy is applied to a resource group, it will be applicable to all the resources in that resource group.

Creating policy and applying policy can be done through REST API or PowerShell.


For mor details see:  [Use Policy to manage resources and control access](https://azure.microsoft.com/en-us/documentation/articles/resource-manager-policy/ )

