---
title: 获取托管资源组并调整 VM 大小 - Azure PowerShell
description: 提供 Azure PowerShell 示例脚本，该脚本获取 Azure 托管应用程序的托管资源组。 该脚本重设 VM 大小。
author: rockboyfor
ms.devlang: powershell
ms.topic: sample
origin.date: 10/27/2017
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 9b16f074c620bd9270f782f5c052f9b08df9a2cb
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76170709"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-powershell"></a>使用 PowerShell 获取托管资源组中的资源并重设 VM 大小

此脚本从托管资源组中检索资源，并重设该资源组中 VM 的大小。

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

```powershell
# Get managed applications from known resource group
Get-AzManagedApplication -ResourceGroupName "DemoApp"

# Get ID of managed resource group
(Get-AzManagedApplication -ResourceGroupName "DemoApp").Properties.managedResourceGroupId

# Get virtual machines in the managed resource group
Get-AzResource -ResourceGroupName DemoApp6zkevchqk7sfq -ResourceType Microsoft.Compute/virtualMachines

# Get information about virtual machines in managed resource group
Get-AzVM -ResourceGroupName DemoApp6zkevchqk7sfq | ForEach{ $_.Name, $_.storageProfile.osDisk.osType, $_.hardwareProfile.vmSize }

## Resize virtual machines in managed resource group
$vm = Get-AzVM -ResourceGroupName DemoApp6zkevchqk7sfq -VMName demoVM
$vm.HardwareProfile.VmSize = "Standard_D2_v2"
Update-AzVM -VM $vm -ResourceGroupName DemoApp6zkevchqk7sfq

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令部署托管应用程序。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [Get-AzManagedApplication](https://docs.microsoft.com/powershell/module/az.resources/get-azmanagedapplication) | 列出托管应用程序。 提供要重点关注结果的资源组名称。 |
| [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | 列出资源。 提供要重点关注结果的资源组和资源类型。 |
| [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) | 更新虚拟机的大小。 |

## <a name="next-steps"></a>后续步骤

* 有关托管应用程序的简介，请参阅 [Azure 托管应用程序概述](../overview.md)。
* 有关 PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/get-started-azureps)。

<!-- Update_Description: new article about managed application powershell sample get managed group resize vm -->
<!--NEW.date: 01/20/2020-->