---
title: 创建托管应用程序定义 - Azure CLI
description: 提供在订阅中创建托管应用程序定义的 Azure CLI 脚本示例。
author: rockboyfor
ms.devlang: azurecli
ms.topic: sample
origin.date: 10/25/2017
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 7c32445c7a445198bdfb185878126314cc86a50f
ms.sourcegitcommit: 8de025ca11b62e06ba3762b5d15cc577e0c0f15d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2020
ms.locfileid: "76170713"
---
# <a name="create-a-managed-application-definition-with-azure-cli"></a>使用 Azure CLI 创建托管应用程序定义

此脚本会将托管应用程序定义发布到服务目录。 

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Create resource group
az group create --name appDefinitionGroup --location chinaeast

# Get Azure Active Directory group to manage the application
groupid=$(az ad group show --group appManagers --query objectId --output tsv)

# Get role
roleid=$(az role definition list --name Owner --query [].name --output tsv)

# Create the definition for a managed application
az managedapp definition create \
  --name "ManagedStorage" \
  --location "chinaeast" \
  --resource-group appDefinitionGroup \
  --lock-level ReadOnly \
  --display-name "Managed Storage Account" \
  --description "Managed Azure Storage Account" \
  --authorizations "$groupid:$roleid" \
  --package-file-uri "https://raw.githubusercontent.com/Azure/azure-managedapp-samples/master/Managed%20Application%20Sample%20Packages/201-managed-storage-account/managedstorage.zip"

```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建托管应用程序定义。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az managedapp definition create](https://docs.microsoft.com/cli/azure/managedapp/definition?view=azure-cli-latest#az-managedapp-definition-create) | 创建托管应用程序定义。 提供包含所需文件的包。 |

## <a name="next-steps"></a>后续步骤

* 有关托管应用程序的简介，请参阅 [Azure 托管应用程序概述](../overview.md)。
* 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

<!-- Update_Description: new article about managed application cli sample create definition -->
<!--NEW.date: 01/20/2020-->