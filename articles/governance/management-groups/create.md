---
title: Create management groups to organize resources - Azure Governance
description: Learn how to create Azure management groups to manage multiple resources using the portal, Azure PowerShell, and Azure CLI. 
ms.date: 04/15/2020
ms.topic: conceptual
---
# Create management groups for resource organization and management

Management groups are containers that help you manage access, policy, and compliance across multiple
subscriptions. Create these containers to build an effective and efficient hierarchy that can be
used with [Azure Policy](../policy/overview.md) and [Azure Role Based Access
Controls](../../role-based-access-control/overview.md). For more information on management groups,
see [Organize your resources with Azure management groups](overview.md).

The first management group created in the directory could take up to 15 minutes to complete. There
are processes that run the first time to set up the management groups service within Azure for your
directory. You receive a notification when the process is complete. For more information, see
[initial setup of management groups](./overview.md#initial-setup-of-management-groups).

## Create a management group

Any Azure AD user in the tenant can create a management group without the management group write
permission assigned to that user. This new management group will be a child of the Root Management
Group and the creator will be given an "Owner" role assignment. Management group service allows this
ability so that role assignments are not needed at the root level. No users have access to the Root
Management Group when it is created. To avoid the hurdle of finding the Azure AD Global Admins to
start using management groups, we allow the creation of the initial management groups at the root  
level.

You can create the management group by using the portal, a
[Resource Manager template](../../azure-resource-manager/templates/deploy-to-tenant.md#create-management-group),
PowerShell, or Azure CLI.

### Create in portal

1. Log into the [Azure portal](https://portal.azure.com).

1. Select **All services** > **Management + governance**.

1. Select **Management Groups**.

1. Select **+ Add management group**.

   :::image type="content" source="./media/main.png" alt-text="Page for working with management groups" border="false":::

1. Fill in the management group ID field.

   - The **Management Group ID** is the directory unique identifier that is used to submit commands
     on this management group. This identifier isn't editable after creation as it is used
     throughout the Azure system to identify this group. The
     [root management group](overview.md#root-management-group-for-each-directory) is automatically
     created with an ID that is the Azure Active Directory ID. For all other management groups,
     assign a unique ID.
   - The display name field is the name that is displayed within the Azure portal. A separate
     display name is an optional field when creating the management group and can be changed at any  
     time.

   :::image type="content" source="./media/create_context_menu.png" alt-text="Options pane for creating a new management group" border="false":::

1. Select **Save**.

### Create in PowerShell

For PowerShell, use the
[New-AzManagementGroup](/powershell/module/az.resources/new-azmanagementgroup) cmdlet to create a
new management group.

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso'
```

The **GroupName** is a unique identifier being created. This ID is used by other commands to
reference this group and it can't be changed later.

If you want the management group to show a different name within the Azure portal, add the
**DisplayName** parameter. For example, to create a management group with the GroupName of Contoso
and the display name of "Contoso Group", use the following cmdlet:

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group'
```

In the preceding examples, the new management group is created under the root management group. To
specify a different management group as the parent, use the **ParentId** parameter.

```azurepowershell-interactive
$parentGroup = Get-AzManagementGroup -GroupName Contoso
New-AzManagementGroup -GroupName 'ContosoSubGroup' -ParentId $parentGroup.id
```

### Create in Azure CLI

For Azure CLI, use the
[az account management-group create](/cli/azure/account/management-group?view=azure-cli-latest#az-account-management-group-create)
command to create a new management group.

```azurecli-interactive
az account management-group create --name Contoso
```

The **name** is a unique identifier being created. This ID is used by other commands to reference
this group and it can't be changed later.

If you want the management group to show a different name within the Azure portal, add the
**display-name** parameter. For example, to create a management group with the GroupName of Contoso
and the display name of "Contoso Group", use the following command:

```azurecli-interactive
az account management-group create --name Contoso --display-name 'Contoso Group'
```

In the preceding examples, the new management group is created under the root management group. To
specify a different management group as the parent, use the **parent** parameter and provide the
name of the parent group.

```azurecli-interactive
az account management-group create --name ContosoSubGroup --parent Contoso
```

## Next steps

To learn more about management groups, see:

- [Create management groups to organize Azure resources](./create.md)
- [How to change, delete, or manage your management groups](./manage.md)
- [Review management groups in Azure PowerShell Resources Module](/powershell/module/az.resources#resources)
- [Review management groups in REST API](/rest/api/resources/managementgroups)
- [Review management groups in Azure CLI](/cli/azure/account/management-group)
