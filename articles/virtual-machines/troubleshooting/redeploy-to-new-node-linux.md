---
title: 将 Linux 虚拟机重新部署到新的 Azure 节点 | Azure
description: 如何通过在 Azure 中重新部署 Linux 虚拟机来解决 SSH 连接问题。
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 10/31/2018
ms.date: 04/27/2020
ms.author: v-yeche
ms.openlocfilehash: 6506bb2f5d319e1c604f2a2e6767ab7a418d8c85
ms.sourcegitcommit: b469d275694fb86bbe37a21227e24019043b9e88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82596101"
---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>将 Linux 虚拟机重新部署到新的 Azure 节点
如果在对 SSH 或应用程序访问 Azure 中 Linux 虚拟机 (VM) 进行故障排除时遇到困难，重新部署 VM 可能会有帮助。 重新部署 VM 时，将 VM 移到 Azure 基础结构中的新节点，并重新提供支持。 所有配置选项和关联资源均保留。 本文介绍如何使用 Azure CLI 或 Azure 门户重新部署 VM。

> [!NOTE]
> 重新部署 VM 后，临时磁盘将丢失，与虚拟网络接口关联的动态 IP 地址将更新。 

<a name="azure-cli-20"></a>
## <a name="use-the-azure-cli"></a>使用 Azure CLI
安装最新的 [Azure CLI](https://docs.azure.cn/cli/install-az-cli2?view=azure-cli-latest) 并使用 [az login](https://docs.azure.cn/cli/reference-index?view=azure-cli-latest#az-login) 登录到 Azure 帐户。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

使用 [az vm redeploy](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-redeploy) 重新部署 VM。 以下示例在名为 *myResourceGroup* 的资源组中重新部署名为 *myVM* 的 VM：

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

<a name="azure-cli-10"></a>
## <a name="use-the-azure-classic-cli"></a>使用 Azure 经典 CLI

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

安装[最新的 Azure 经典 CLI](../../cli-install-nodejs.md) 并登录到 Azure 帐户。 请确保处于资源管理器模式 (`azure config mode arm`)。

以下示例在名为 *myResourceGroup* 的资源组中重新部署名为 *myVM* 的 VM：

```console
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

<a name="using-azure-portal"></a>
[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>后续步骤
如果在连接 VM 时遇到问题，可以在 [SSH 连接故障排除](troubleshoot-ssh-connection.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)或[详细的 SSH 故障排除步骤](detailed-troubleshoot-ssh-connection.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到特定的帮助。 如果无法访问在 VM 上运行的应用程序，还可以阅读[应用程序故障排除问题](troubleshoot-app-connection.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)。

<!-- Update_Description: update meta properties, wording update, update link -->