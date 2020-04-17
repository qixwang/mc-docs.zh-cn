---
title: include 文件
description: include 文件
services: azure-resource-manager
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: include
origin.date: 05/21/2018
ms.date: 04/15/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 7a37f3aef99a1941048f8e67f69eadf14aa86df3
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63825123"
---
若要为资源组添加两个标记，请使用 [Set-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/set-azresourcegroup) 命令：

```powershell
Set-AzResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

让我们假设要添加第三个标记。 每次将标记应用到某个资源或资源组时，都会覆盖该资源或资源组中的现有标记。 若要添加新标记而不会丢失现有标记，必须检索现有标记、添加新标记，并重新应用标记集合：

```powershell
# Get existing tags and add a new tag
$tags = (Get-AzResourceGroup -Name myResourceGroup).Tags
$tags.Add("Project", "Documentation")

# Reapply the updated set of tags 
Set-AzResourceGroup -Tag $tags -Name myResourceGroup
```

资源不从资源组继承标记。 目前，资源组有三个标记，但资源没有任何标记。 要将资源组中的所有标记应用于其资源，并且保留资源上不重复的现有标记，请使用以下脚本：

```powershell
# Get the resource group
$group = Get-AzResourceGroup myResourceGroup

if ($group.Tags -ne $null) {
    # Get the resources in the resource group
    $resources = Get-AzResource -ResourceGroupName $group.ResourceGroupName

    # Loop through each resource
    foreach ($r in $resources)
    {
        # Get the tags for this resource
        $resourcetags = (Get-AzResource -ResourceId $r.ResourceId).Tags

        # If the resource has existing tags, add new ones
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }

            # Reapply the updated tags to the resource 
            Set-AzResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

或者，可以将资源组中的标记应用于资源而不保留现有标记：

```powershell
# Get the resource group
$g = Get-AzResourceGroup -Name myResourceGroup

# Find all the resources in the resource group, and for each resource apply the tags from the resource group
Get-AzResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

若要将几个值组合到单个标记中，请使用 JSON 字符串。

```powershell
Set-AzResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

若要添加具有多个值的新标记而不丢失现有标记，必须检索现有标记、对新标记使用 JSON 字符串，并重新应用标记集合：

```powershell
# Get existing tags and add a new tag
$ResourceGroup = Get-AzResourceGroup -Name myResourceGroup
$Tags = $ResourceGroup.Tags
$Tags.Add("CostCenter", "{`"Dept`":`"IT`",`"Environment`":`"Test`"}")

# Reapply the updated set of tags
$ResourceGroup | Set-AzResourceGroup -Tag $Tags
```

若要删除所有标记，请传递一个空哈希表。

```powershell
Set-AzResourceGroup -Name myResourceGroup -Tag @{ }
```

