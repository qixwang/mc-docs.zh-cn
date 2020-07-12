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
ms.openlocfilehash: 97db52cf660f899d8b744f0cfd90fa99eae31611
ms.sourcegitcommit: 6c9e5b3292ade56d812e7e214eeb66aeb9b8776e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86218838"
---
```PowerShell
$ResourceGroupName="yourResourceGroupName"
$DiskName="yourDiskName"

$disk=Get-AzDisk -ResourceGroupName $ResourceGroupName -DiskName $DiskName
$disk.Encryption.Type
```

<!-- Update_Description: new article about virtual machines disks encryption status powershell -->
<!--NEW.date: 07/13/2020-->