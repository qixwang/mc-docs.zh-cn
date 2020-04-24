---
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 10/26/2018
ms.date: 03/04/2019
ms.author: v-jay
ms.openlocfilehash: 86e8af2e71c428d92606629321f9a43a1c73ff9c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63844377"
---
## <a name="sign-in-to-azure"></a>登录 Azure

运行 `Connect-AzAccount -Environment AzureChinaCloud` 命令以登录 Azure 订阅，并按照屏幕上的说明操作。

```powershell
Connect-AzAccount -Environment AzureChinaCloud
```

如果你不知道要使用哪个位置，可以列出可用的位置。 使用以下代码示例显示位置列表，并找到要使用的位置。 此示例使用“chinaeast”  。 将位置存储在变量中，并使用该变量，这样就可以在一个位置更改它。

```powershell
Get-AzLocation | select Location
$location = "China East"
```

## <a name="create-a-resource-group"></a>创建资源组

使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 创建 Azure 资源组。 资源组是在其中部署和管理 Azure 资源的逻辑容器。 

```powershell
$resourceGroup = "myResourceGroup"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

## <a name="create-a-storage-account"></a>创建存储帐户

使用 [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) 创建具有 LRS 复制功能的标准常规用途存储帐户。 接下来，获取用于定义要使用的存储帐户的存储帐户上下文。 对存储帐户执行操作时，引用上下文而不是重复传入凭据。 使用以下示例创建一个名为 mystorageaccount  的存储帐户，该帐户默认启用本地冗余存储 (LRS) 和 Blob 加密。

```powershell
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -SkuName Standard_LRS `
  -Location $location `

$ctx = $storageAccount.Context
```
