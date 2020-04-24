---
title: 使用远程工具排查 Azure VM 问题 | Azure
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
origin.date: 01/11/2018
ms.date: 11/11/2019
ms.author: v-yeche
ms.openlocfilehash: f88424262d8c29ef3d53b84e4fcf02c8bb693a25
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74116945"
---
# <a name="use-remote-tools-to-troubleshoot-azure-vm-issues"></a>使用远程工具排查 Azure VM 问题

排查 Azure 虚拟机 (VM) 的问题时，可以使用本文所述的远程工具而不是远程桌面协议 (RDP) 连接到 VM。

<!--Not Available on ## Serial Console-->

<!--Not Available on Use [Virtual Machine Serial Console](serial-console-windows.md)-->

## <a name="remote-cmd"></a>远程 CMD

下载 [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec)。 运行以下命令连接到 VM：

```cmd
psexec \\<computer> -u user -s cmd
```

<!--MOONCAKE: Correct on \\<computer> -u-->

>[!Note]
>* 必须在位于同一 VNET 中的计算机上运行该命令。
>* 可以使用 DIP 或主机名来替换 \<computer>。
>* -s 参数确保使用系统帐户（管理员权限）调用命令。
>* PsExec 使用 TCP 端口 135 和 445。 因此，需要在防火墙中打开这两个端口。

## <a name="run-commands"></a>运行命令

有关如何使用“运行命令”功能在 VM 上运行脚本的详细信息，请参阅[使用“运行命令”在 Windows VM 中运行 PowerShell 脚本](../windows/run-command.md)。

## <a name="customer-script-extension"></a>自定义脚本扩展

可以使用“自定义脚本扩展”功能在目标 VM 上运行自定义脚本。 若要使用此功能，必须符合以下条件：

* VM 已建立连接。

* 已在 VM 上安装 Azure 代理，并且该代理正在按预期方式运行。

* 未事先在 VM 上安装该扩展。

    该扩展仅在首次使用时才注入脚本。 如果以后再使用此功能，该扩展将识别到它已被用过，因此不会上传新脚本。

必须将脚本上传到存储帐户，并生成该帐户自身的容器。 然后，在已连接到 VM 的计算机上的 Azure PowerShell 中运行以下脚本。

### <a name="for-v1-vms"></a>对于 V1 VM

```powershell
#Setup the basic variables
$subscriptionID = "<<SUBSCRIPTION ID>>" 
$storageAccount = "<<STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for the blob in the storage
$vmName = "<<VM NAME>>" 
$vmCloudService = "<<CLOUD SERVICE>>" #Resource group/Cloud Service where the VM is hosted. I.E.: For "demo305.chinacloudapp.cn" the cloud service is going to be demo305

#Setup the Azure Powershell module and ensure the access to the subscription
Import-Module Azure
Add-AzureAccount -Environment AzureChinaCloud  #Ensure Login with account associated with subscription ID
Get-AzureSubscription -SubscriptionId $subscriptionID | Select-AzureSubscription

#Setup the access to the storage account and upload the script
$storageKey = (Get-AzureStorageKey -StorageAccountName $storageAccount).Primary
$context = New-AzureStorageContext -Environment AzureChinaCloud -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)<
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM
$vm = Get-AzureVM -ServiceName $vmCloudService -Name $vmName
Set-AzureVMCustomScriptExtension "CustomScriptExtension" -VM $vm -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName | Update-AzureVM
```

### <a name="for-v2-vms"></a>对于 V2 VM

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

```powershell
#Setup the basic variables
$subscriptionID = "<<SUBSCRIPTION ID>>"
$storageAccount = "<<STORAGE ACCOUNT>>"
$storageRG = "<<RESOURCE GROUP OF THE STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for blob in storage
$vmName = "<<VM NAME>>" 
$vmResourceGroup = "<<RESOURCE GROUP>>"
$vmLocation = "<<DATACENTER>>" 

#Setup the Azure Powershell module and ensure the access to the subscription
Import-Module Az
Connect-AzAccount -Environment AzureChinaCloud #Ensure Login with account associated with subscription ID
Get-AzSubscription -SubscriptionId $subscriptionID | Select-AzSubscription

#Setup the access to the storage account and upload the script 
$storageKey = (Get-AzStorageAccountKey -ResourceGroupName $storageRG -Name $storageAccount).Value[0]
$context = New-AzStorageContext -Environment AzureChinaCloud -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)
New-AzStorageContainer -Name $container -Permission Off -Context $context
Set-AzStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM
Set-AzVMCustomScriptExtension -Name "CustomScriptExtension" -ResourceGroupName $vmResourceGroup -VMName $vmName -Location $vmLocation -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName
```

<!--Not Available on ## Remote PowerShell-->
## <a name="remote-registry"></a>远程注册表

>[!Note]
>必须打开 TCP 端口 135 或 445 才能使用此选项。
>
>对于 ARM VM，必须在 NSG 中打开端口 5986。 有关详细信息，请参阅“安全组”。 
>
>对于 RDFE VM，必须有一个配备专用端口 5986 和公共端口的终结点。 还必须在 NSG 中打开该公共端口。

1. 在同一 VNET 中的另一个 VM 上，打开注册表编辑器 (regedit.exe)。

2. 选择“文件” **“连接网络注册表”。**  > 

    ![远程选项](./media/remote-tools-troubleshoot-azure-vm-issues/remote-registry.png) 

3. 在“输入要选择的对象名称”框中输入目标 VM 的**主机名**或**动态 IP**（首选），以找到该 VM。

    ![远程选项](./media/remote-tools-troubleshoot-azure-vm-issues/input-computer-name.png) 

4. 输入目标 VM 的凭据。

5. 进行任何必要的注册表更改。

## <a name="remote-services-console"></a>远程服务控制台

>[!Note]
>必须打开 TCP 端口 135 或 445 才能使用此选项。
>
>对于 ARM VM，必须在 NSG 中打开端口 5986。 有关详细信息，请参阅“安全组”。 
>
>对于 RDFE VM，必须有一个配备专用端口 5986 和公共端口的终结点。 然后，还必须在 NSG 中打开该公共端口。

1. 在同一 VNET 中的另一个 VM 上，打开 **Services.msc** 的实例。

2. 右键单击“服务(本地)”。 

3. 选择“连接到另一台计算机”。 

    ![远程服务](./media/remote-tools-troubleshoot-azure-vm-issues/remote-services.png)

4. 输入目标 VM 的动态 IP。

    ![输入 DIP](./media/remote-tools-troubleshoot-azure-vm-issues/input-ip-address.png)

5. 对服务进行任何必要的更改。

## <a name="next-steps"></a>后续步骤

[Enter-PSSession](https://technet.microsoft.com/library/hh849707.aspx)

[适用于 Windows 的自定义脚本扩展（使用经典部署模型）](../extensions/custom-script-classic.md)

PsExec 包含在 [PSTools Suite](https://download.sysinternals.com/files/PSTools.zip) 中。

有关 PSTools Suite 的详细信息，请参阅 [PSTools Suite](https://docs.microsoft.com/sysinternals/downloads/pstools)。

<!-- Update_Description: update meta properties, wording update, update link -->