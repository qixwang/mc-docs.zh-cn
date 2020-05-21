---
title: 使用 CLI 获取维护通知
description: 使用 Azure CLI 查看在 Azure 中运行的虚拟机的维护通知并启动自助维护。
author: rockboyfor
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
origin.date: 11/19/2019
ms.date: 04/27/2020
ms.author: v-yeche
ms.openlocfilehash: ceb7b9e834542b138c2c8f79ac8c2f275ae9d8a3
ms.sourcegitcommit: 2d8950c6c255361eb6c66406988e25c69cf4e0f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83392330"
---
# <a name="handling-planned-maintenance-notifications-using-the-azure-cli"></a>使用 Azure CLI 处理计划内维护通知

**本文适用于同时运行 Linux 和 Windows 的虚拟机。**

可以使用 CLI 查看何时安排 VM 进行[维护](maintenance-notifications.md)。 可通过 [az vm get-instance-view](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-get-instance-view) 获得计划内维护信息。

仅当有计划内维护时，才会返回维护信息。 

```azurecli
az vm get-instance-view -n myVM -g myResourceGroup --query instanceView.maintenanceRedeployStatus
```

## <a name="start-maintenance"></a>启动维护

如果 `IsCustomerInitiatedMaintenanceAllowed` 设置为 true，以下调用会在 VM 上启动维护。

```azurecli
az vm perform-maintenance -g myResourceGroup -n myVM 
```

## <a name="classic-deployments"></a>经典部署

[!INCLUDE [classic-vm-deprecation](../../includes/classic-vm-deprecation.md)]

如果你仍有通过使用经典部署模型部署的旧 VM，则可以使用 Azure 经典 CLI 查询 VM，并启动维护。

通过键入以下内容确保在正确模式下使用经典 VM：

```
azure config mode asm
```

若要获取名为“myVM”的 VM 的维护状态，请键入：

```
azure vm show myVM 
``` 

若要在名为 myVM 的经典 VM 的 myService 服务和 myDeployment 部署中启动维护，请键入：

```
azure compute virtual-machine initiate-maintenance --service-name myService --name myDeployment --virtual-machine-name myVM
```

## <a name="next-steps"></a>后续步骤

还可以使用 [Azure PowerShell](maintenance-notifications-powershell.md) 或[门户](maintenance-notifications-portal.md)处理计划内维护。

<!-- Update_Description: update meta properties, wording update, update link -->