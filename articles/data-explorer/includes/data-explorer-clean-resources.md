---
author: lucygoldbergmicrosoft
ms.service: data-explorer
ms.topic: include
origin.date: 11/28/2019
ms.date: 05/15/2020
ms.author: v-tawe
ms.openlocfilehash: b7f6866abc79d9e5ae5d145aa7235e3518876e89
ms.sourcegitcommit: bfbd6694da33f703481386f2a3f16850c4e94bfa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83417651"
---
## <a name="clean-up-resources"></a>清理资源

不再需要 Azure 资源时，请通过删除资源组来清理部署的资源。 

### <a name="clean-up-resources-using-the-azure-portal"></a>使用 Azure 门户清理资源

按[清理资源](../create-cluster-database-portal.md#clean-up-resources)中的步骤删除 Azure 门户中的资源。

### <a name="clean-up-resources-using-powershell"></a>使用 PowerShell 清理资源

```azurepowershell
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -ResourceGroupName $resourceGroupName

Write-Host "Press [ENTER] to continue ..."
```
