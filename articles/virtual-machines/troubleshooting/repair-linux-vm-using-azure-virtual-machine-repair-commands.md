---
title: 使用 Azure 虚拟机修复命令修复 Linux VM | Azure
description: 本文详细介绍如何使用 Azure 虚拟机修复命令将磁盘连接到另一个 Linux VM 来修复所有错误，然后重新生成原始 VM。
services: virtual-machines-linux
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
origin.date: 09/10/2019
ms.date: 11/11/2019
ms.author: v-yeche
ms.openlocfilehash: d14120c72d392993140ceb9ecd1dc6dd67bd7950
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73730733"
---
# <a name="repair-a-linux-vm-by-using-the-azure-virtual-machine-repair-commands"></a>使用 Azure 虚拟机修复命令修复 Linux VM

如果 Linux 虚拟机 (VM) 在 Azure 中遇到启动或磁盘错误，可能需要对磁盘本身执行缓解操作。 一个常见示例是应用程序更新失败，使 VM 无法成功启动。 本文详细介绍如何使用 Azure 虚拟机修复命令将磁盘连接到另一个 Linux VM 来修复所有错误，然后重新生成原始 VM。

> [!IMPORTANT]
> 本文中的脚本仅适用于使用 [Azure 资源管理器](/azure-resource-manager/resource-group-overview)的 VM。

## <a name="repair-process-overview"></a>修复过程概述

现在可以使用 Azure 虚拟机修复命令更改 VM 的 OS 磁盘，而不再需要删除并重新创建 VM。

请按照下列步骤排查 VM 问题：

1. 启动 Azure 本地 Shell
2. 运行 az extension add/update
3. 运行 az vm repair create
4. 执行缓解步骤
5. 运行 az vm repair restore

有关其他文档和说明，请参阅 [az vm repair](https://docs.azure.cn/cli/ext/vm-repair/vm/repair?view=azure-cli-latest#az-vm-repair)。

## <a name="repair-process-example"></a>修复过程示例

1. 启动 Azure 本地 Shell

    <!--Not Available on The Azure Cloud Shell-->
    <!--Not Available on select **Try it** from the upper-right corner of a code block.-->
    <!--Not Available on Select **Copy** to copy the blocks of code, then paste the code into the local Shell, and select **Enter** to run it.-->

    如果希望在本地安装并使用 CLI，则本快速入门需要 Azure CLI version 2.0.30 或更高版本。 运行 ``az --version`` 即可查找版本。 如果需要安装或升级 Azure CLI，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

2. 如果是首次使用 `az vm repair` 命令，请添加 vm-repair CLI 扩展。

    ```azurecli
    az extension add -n vm-repair
    ```

    如果以前使用过 `az vm repair` 命令，请将任何更新应用于 vm-repair 扩展。

    ```azurecli
    az extension update -n vm-repair
    ```

3. 运行 `az vm repair create`。 此命令将为无法运行的 VM 创建 OS 磁盘的副本，创建修复 VM 并附加磁盘。

    ```azurecli
    az vm repair create -g MyResourceGroup -n myVM --repair-username username --repair-password password!234 --verbose
    ```

4. 在创建的修复 VM 上执行任何所需的缓解步骤，然后继续执行步骤 5。

5. 运行 `az vm repair restore`。 此命令会将已修复的 OS 磁盘与 VM 的原始 OS 磁盘交换。

    ```azurecli
    az vm repair restore -g MyResourceGroup -n MyVM --verbose
    ```

## <a name="verify-and-enable-boot-diagnostics"></a>验证和启用启动诊断

以下示例在名为 ``myResourceGroup`` 的资源组中名为 ``myVMDeployed`` 的 VM 上启用诊断扩展：

Azure CLI

```azurecli
az vm boot-diagnostics enable --name myVMDeployed --resource-group myResourceGroup --storage https://mystor.blob.core.chinacloudapi.cn/
```

## <a name="next-steps"></a>后续步骤

* 如果在连接到 VM 时遇到问题，请参阅[对 Azure 虚拟机的 RDP 连接进行故障排除](/virtual-machines/troubleshooting/troubleshoot-rdp-connection)。
* 如果在访问 VM 上运行的应用程序时遇到问题，请参阅[排查 Azure 中虚拟机上的应用程序连接问题](/virtual-machines/troubleshooting/troubleshoot-app-connection)。
* 有关资源组的详细信息，请参阅 [Azure 资源管理器概述](/azure-resource-manager/resource-group-overview)。

<!--Update_Description: new articles on repair linux vm using repair commands -->
<!--New.date: 11/11/2019-->