---
title: 使用 PowerShell 收集订阅中所有 VM 的详细信息
description: 使用 PowerShell 收集订阅中所有 VM 的详细信息
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: v-miegge
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
origin.date: 07/01/2019
ms.date: 07/06/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: c74f8eb6ed0a07fedddb17ed233c8683f07398b4
ms.sourcegitcommit: 89118b7c897e2d731b87e25641dc0c1bf32acbde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85945647"
---
# <a name="collect-details-about-all-vms-in-a-subscription-with-powershell"></a>使用 PowerShell 收集订阅中所有 VM 的详细信息

此脚本创建一个 csv，其中包含所提供订阅中 VM 的 VM 名称、资源组名称、区域、VM 大小、虚拟网络、子网、专用 IP 地址、OS 类型和公共 IP 地址。

如果你没有 [Azure 订阅](/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)，请在开始之前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="launch-azure-powershell"></a>启动 Azure PowerShell

在 PowerShell 控制台中以管理员权限运行以下示例脚本。

## <a name="sample-script"></a>示例脚本

```powershell
#Provide the subscription Id where the VMs reside
$subscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

#Provide the name of the csv file to be exported
$reportName = "myReport.csv"

Select-AzSubscription $subscriptionId
$report = @()
$vms = Get-AzVM
$publicIps = Get-AzPublicIpAddress 
$nics = Get-AzNetworkInterface | ?{ $_.VirtualMachine -NE $null} 
foreach ($nic in $nics) { 
    $info = "" | Select VmName, ResourceGroupName, Region, VmSize, VirturalNetwork, Subnet, PrivateIpAddress, OsType, PublicIPAddress 
    $vm = $vms | ? -Property Id -eq $nic.VirtualMachine.id 
    foreach($publicIp in $publicIps) { 
        if($nic.IpConfigurations.id -eq $publicIp.ipconfiguration.Id) {
            $info.PublicIPAddress = $publicIp.ipaddress
            } 
        } 
        $info.OsType = $vm.StorageProfile.OsDisk.OsType 
        $info.VMName = $vm.Name 
        $info.ResourceGroupName = $vm.ResourceGroupName 
        $info.Region = $vm.Location 
        $info.VmSize = $vm.HardwareProfile.VmSize
        $info.VirturalNetwork = $nic.IpConfigurations.subnet.Id.Split("/")[-3] 
        $info.Subnet = $nic.IpConfigurations.subnet.Id.Split("/")[-1] 
        $info.PrivateIpAddress = $nic.IpConfigurations.PrivateIpAddress 
        $report+=$info 
    } 
$report | ft VmName, ResourceGroupName, Region, VmSize, VirturalNetwork, Subnet, PrivateIpAddress, OsType, PublicIPAddress 
$report | Export-CSV "$home/$reportName"
```

## <a name="script-explanation"></a>脚本说明
此脚本使用以下命令创建订阅中 VM 详细信息的 csv 导出。 表中的每条命令均链接到特定于命令的文档。

|命令|注释|
|-|-|
|[Select-AzSubscription](https://docs.microsoft.com/powershell/module/Az.Accounts/Set-AzContext)|设置要在当前会话中使用的 cmdlet 的租户、订阅和环境。|
|[Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM)|获取虚拟机的属性。|
|[Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/Az.Network/Get-AzPublicIpAddress)|获取公共 IP 地址。|
|[Get-AzNetworkInterface](https://docs.microsoft.com/powershell/module/Az.Network/Get-AzNetworkInterface)|获取网络接口。|

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 模块的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/overview)。

可以在 [Azure Windows VM 文档](/virtual-machines/windows/powershell-samples?toc=/virtual-machines/windows/toc.json)中找到其他虚拟机 PowerShell 脚本示例。

<!-- Update_Description: update meta properties, wording update, update link -->