---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
ms.date: 11/07/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 01e90bfb9c20b6b53df2b007f03833bea46f1383
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74655484"
---
## <a name="shared-image-management"></a>共享映像管理 

下面是一些常见的管理任务示例，并说明如何使用 PowerShell 完成这些任务。

按名称列出所有库。

```azurepowershell
$galleries = Get-AzResource -ResourceType Microsoft.Compute/galleries
$galleries.Name
```

按名称列出所有映像定义。

```azurepowershell
$imageDefinitions = Get-AzResource -ResourceType Microsoft.Compute/galleries/images
$imageDefinitions.Name
```

按名称列出所有映像版本。

```azurepowershell
$imageVersions = Get-AzResource -ResourceType Microsoft.Compute/galleries/images/versions
$imageVersions.Name
```

删除映像版本。 此示例将删除名为“1.0.0”的映像版本  。

```azurepowershell
Remove-AzGalleryImageVersion `
   -GalleryImageDefinitionName myImageDefinition `
   -GalleryName myGallery `
   -Name 1.0.0 `
   -ResourceGroupName myGalleryRG
```

<!--Update_Description: wording update-->
