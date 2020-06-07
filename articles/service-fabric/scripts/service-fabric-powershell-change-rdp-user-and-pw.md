---
title: 在 PowerShell 中更新 RDP 用户名和密码
description: Azure PowerShell 脚本示例 - 更新特定节点类型的所有 Service Fabric 群集节点的 RDP 用户名和密码。
services: service-fabric
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
origin.date: 03/19/2018
ms.date: 06/08/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 94f356a50f63ca39be45b30f9c478e665bc1aa59
ms.sourcegitcommit: 0e178672632f710019eae60cea6a45ac54bb53a1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84356201"
---
# <a name="update-the-admin-username-and-password-of-the-vms-in-a-cluster"></a>更新群集中 VM 的管理员用户名和密码

Service Fabric 群集中的每个[节点类型](../service-fabric-cluster-nodetypes.md)是一个虚拟机规模集。 此示例脚本更新特定节点类型中群集虚拟机的管理员用户名和密码。  将 VMAccessAgent 扩展添加到规模集，因为管理员密码是不可修改的规模集属性。  用户名和密码更改将应用到规模集中的所有节点。 根据需要自定义参数。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

必要时，请使用 [Azure PowerShell 指南](https://docs.microsoft.com/powershell/azure/overview)中的说明安装 Azure PowerShell。 

## <a name="sample-script"></a>示例脚本

```powershell
Connect-AzAccount -Environment AzureChinaCloud
Get-AzSubscription
Set-AzContext -SubscriptionId 'yourSubscriptionID'

$nodeTypeName = 'nt1vm'
$resourceGroup = 'sfclustertutorialgroup'
$publicConfig = @{'UserName' = 'newuser'}
$privateConfig = @{'Password' = 'PasSwo0rd$#!'}
$extName = 'VMAccessAgent'
$publisher = 'Microsoft.Compute'
$node = Get-AzVmss -ResourceGroupName $resourceGroup -VMScaleSetName $nodeTypeName
$node = Add-AzVmssExtension -VirtualMachineScaleSet $node -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion '2.0' -AutoUpgradeMinorVersion $true

Update-AzVmss -ResourceGroupName $resourceGroup -Name $nodeTypeName -VirtualMachineScaleSet $node

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令：表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [Get-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/get-azvmss) | 获取群集节点类型（虚拟机规模集）的属性。   |
| [Add-AzVmssExtension](https://docs.microsoft.com/powershell/module/az.compute/add-azvmssextension)| 将扩展添加到虚拟机规模集。|
| [Update-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/update-azvmss)|将虚拟机规模集的状态更新为 VMSS 对象的状态。|

## <a name="duration"></a>Duration

对于单节点类型，例如，如果有五个节点，那么更改用户名或密码的持续时间为 45 到 60 分钟。 

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure PowerShell 示例](../service-fabric-powershell-samples.md)中找到 Azure Service Fabric 的其他 Azure Powershell 示例。

<!-- Update_Description: update meta properties, wording update, update link -->