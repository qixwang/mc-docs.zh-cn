---
author: lucygoldbergmicrosoft
ms.service: data-explorer
ms.topic: include
origin.date: 11/28/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.openlocfilehash: 0ca99f5508bc97cf309f4c325c9c9972acdfa586
ms.sourcegitcommit: ac70b12de243a9949bf86b81b2576e595e55b2a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919351"
---
## <a name="clean-up-resources"></a>清理资源

不再需要 Azure 资源时，请通过删除资源组来清理部署的资源。 

### <a name="clean-up-resources-using-the-azure-portal"></a>使用 Azure 门户清理资源

按[清理资源](../articles/data-explorer/create-cluster-database-portal.md#clean-up-resources)中的步骤删除 Azure 门户中的资源。

### <a name="clean-up-resources-using-powershell"></a>使用 PowerShell 清理资源

如果 Cloud Shell 仍处于打开状态，则无需复制/运行第一行 (Read-Host)。

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```