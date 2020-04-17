---
title: 获取托管资源组并调整 VM 大小 - Azure CLI
description: 提供 Azure CLI 示例脚本，该脚本获取 Azure 托管应用程序的托管资源组。 该脚本重设 VM 大小。
author: rockboyfor
ms.devlang: azurecli
ms.topic: sample
origin.date: 10/25/2017
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: f5895e83e968dd5ae3b392a595ef6e22e13381b7
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76170711"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>使用 Azure CLI 获取托管资源组中的资源并重设 VM 大小

此脚本从托管资源组中检索资源，并重设该资源组中 VM 的大小。

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Get managed applications from known resource group
az managedapp list --query "[?contains(resourceGroup,'DemoApp')]"

# Get ID of managed resource group
az managedapp list --query "[?contains(resourceGroup,'DemoApp')].{ managedResourceGroup:managedResourceGroupId }"

# Get virtual machines in the managed resource group
az resource list -g DemoApp6zkevchqk7sfq --query "[?contains(type,'Microsoft.Compute/virtualMachines')]"

# Get information about virtual machines in managed resource group
az vm list -g DemoApp6zkevchqk7sfq --query "[].{VMName:name,OSType:storageProfile.osDisk.osType,VMSize:hardwareProfile.vmSize}"

## Resize virtual machines in managed resource group
az vm resize --size Standard_D2_v2 --ids $(az vm list -g DemoApp6zkevchqk7sfq --query "[].id" -o tsv)
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令部署托管应用程序。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az managedapp list](https://docs.microsoft.com/cli/azure/managedapp?view=azure-cli-latest#az-managedapp-list) | 列出托管应用程序。 提供要重点关注结果的查询值。 |
| [az resource list](https://docs.azure.cn/cli/resource?view=azure-cli-latest#az-resource-list) | 列出资源。 提供要重点关注结果的资源组和查询值。 |
| [az vm resize](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-resize) | 更新虚拟机的大小。 |

## <a name="next-steps"></a>后续步骤

* 有关托管应用程序的简介，请参阅 [Azure 托管应用程序概述](../overview.md)。
* 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

<!-- Update_Description: new article about managed application cli sample get managed group resize vm -->
<!--NEW.date: 01/20/2020-->