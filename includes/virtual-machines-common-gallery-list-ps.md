---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 04/28/2020
ms.date: 05/25/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 9ec45dd17941cd8fc3c040def27b2ff19d7fc2a2
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801274"
---
## <a name="list-information"></a>列出信息

按名称列出所有库。

```powershell
$galleries = Get-AzResource -ResourceType Microsoft.Compute/galleries
$galleries.Name
```

按名称列出所有映像定义。

```powershell
$imageDefinitions = Get-AzResource -ResourceType Microsoft.Compute/galleries/images
$imageDefinitions.Name
```

按名称列出所有映像版本。

```powershell
$imageVersions = Get-AzResource -ResourceType Microsoft.Compute/galleries/images/versions
$imageVersions.Name
```

删除映像版本。 此示例将删除名为“1.0.0”的映像版本。

```powershell
Remove-AzGalleryImageVersion `
   -GalleryImageDefinitionName myImageDefinition `
   -GalleryName myGallery `
   -Name 1.0.0 `
   -ResourceGroupName myGalleryRG
```

<!-- Update_Description: update meta properties, wording update, update link -->