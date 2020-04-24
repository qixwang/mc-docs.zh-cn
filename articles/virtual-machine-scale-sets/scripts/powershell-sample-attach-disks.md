---
title: Azure PowerShell 示例 - 附加并使用数据磁盘 | Microsoft Docs
description: Azure PowerShell 示例
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 03/27/2018
ms.date: 05/16/2019
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: df52f409871dca11ac6888ea57c5ca39e54984ca
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "65917446"
---
# <a name="attach-and-use-data-disks-with-a-virtual-machine-scale-set-with-powershell"></a>使用 PowerShell 为虚拟机规模集附加并使用数据磁盘
此脚本创建虚拟机规模集并附加和准备数据磁盘。

如果没有 Azure 订阅，可在开始前创建一个 [试用帐户](https://www.azure.cn/pricing/1rmb-trial) 。

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>示例脚本
```powershell
# Provide your own secure password for use with the VM instances
$cred = Get-Credential

# Create a virtual machine scale set and supporting resources
# A resource group, virtual network, load balancer, and NAT rules are automatically
# created if they do not already exist
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myScaleSet" `
  -Location "ChinaNorth" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -Credential $cred `
  -UpgradePolicy "Automatic" `
  -DataDiskSizeGb 64,128

# Get scale set object
$vmss = Get-AzureRmVmss `
          -ResourceGroupName "myResourceGroup" `
          -VMScaleSetName "myScaleSet"

# Set the URI of a custom script that prepares any attached disks
$publicSettings = @{
  "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.ps1");
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File prepare_vm_disks.ps1"
}

# Use Custom Script Extension to prepare the attached data disks
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.8 `
  -Setting $publicSettings

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmss
```

## <a name="clean-up-deployment"></a>清理部署
运行以下命令可删除资源组、规模集和所有相关资源。

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明
此脚本使用以下命令创建部署。 表中的每一项均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [New-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss) | 创建虚拟机规模集和所有支持资源，包括虚拟网络、负载均衡器和 NAT 规则。 |
| [Get-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/get-azvmss) | 获取有关虚拟机规模集的信息。 |
| [Add-AzVmssExtension](https://docs.microsoft.com/powershell/module/az.compute/add-azvmssextension) | 添加自定义脚本的 VM 扩展以安装基本的 Web 应用程序。 |
| [Update-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/update-azvmss) | 更新虚拟机规模集模型以应用 VM 扩展。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组及其中包含的所有资源。 |

## <a name="next-steps"></a>后续步骤
有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure 虚拟机规模集文档](../powershell-samples.md)中找到其他虚拟机规模集 PowerShell 脚本示例。

<!-- Update_Description: wording update -->