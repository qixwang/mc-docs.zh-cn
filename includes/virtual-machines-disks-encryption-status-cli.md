---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 06/23/2020
ms.date: 07/13/2020
ms.testscope: yes
ms.testdate: 07/13/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 38b3b59fee9d1a31f7bc6528cf3aa50461737bc4
ms.sourcegitcommit: 6c9e5b3292ade56d812e7e214eeb66aeb9b8776e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86218841"
---
```azurecli
az disk show -g yourResourceGroupName -n yourDiskName --query [encryption.type] -o tsv
```

<!-- Update_Description: new article about virtual machines disks encryption status cli -->
<!--NEW.date: 07/13/2020-->