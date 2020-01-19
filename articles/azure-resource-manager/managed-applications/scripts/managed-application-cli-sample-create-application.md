---
title: Azure CLI 脚本示例 - 部署托管应用程序
description: 提供 Azure CLI 示例脚本，该脚本将 Azure 托管应用程序定义部署到订阅。
author: rockboyfor
ms.devlang: azurecli
ms.topic: sample
origin.date: 10/25/2017
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: f9036bfb9ae746cf8e572d4beb0390298d18fa40
ms.sourcegitcommit: 8de025ca11b62e06ba3762b5d15cc577e0c0f15d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2020
ms.locfileid: "76170714"
---
# <a name="deploy-a-managed-application-for-service-catalog-with-azure-cli"></a>使用 Azure CLI 为服务目录部署托管应用程序

此脚本从服务目录部署托管应用程序定义。 

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Create resource group
az group create --name applicationGroup --location chinaeast 

# Get ID of managed application definition
appid=$(az managedapp definition show --name ManagedStorage --resource-group appDefinitionGroup --query id --output tsv)

# Get subscription ID
subid=$(az account show --query id --output tsv)

# Construct the ID of the managed resource group
managedGroupId=/subscriptions/$subid/resourceGroups/infrastructureGroup

# Create the managed application
az managedapp create \
  --name storageApp \
  --location "chinaeast" \
  --kind "Servicecatalog" \
  --resource-group applicationGroup \
  --managedapp-definition-id $appid \
  --managed-rg-id $managedGroupId \
  --parameters "{\"storageAccountNamePrefix\": {\"value\": \"demostorage\"}, \"storageAccountType\": {\"value\": \"Standard_LRS\"}}"

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令部署托管应用程序。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az managedapp create](https://docs.microsoft.com/cli/azure/managedapp?view=azure-cli-latest#az-managedapp-create) | 创建托管应用程序。 提供模板的定义 ID 和参数。 |

## <a name="next-steps"></a>后续步骤

* 有关托管应用程序的简介，请参阅 [Azure 托管应用程序概述](../overview.md)。
* 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

<!-- Update_Description: new article about managed application cli sample create application -->
<!--NEW.date: 01/20/2020-->