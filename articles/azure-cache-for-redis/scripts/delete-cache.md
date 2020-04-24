---
title: Azure CLI 脚本示例 - 删除 Azure Redis 缓存 | Microsoft Docs
description: Azure CLI 脚本示例 - 删除 Azure Redis 缓存
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
tags: azure-service-management
ms.assetid: 7beded7a-d2c9-43a6-b3b4-b8079c11de4a
ms.service: cache
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
origin.date: 08/30/2017
ms.date: 02/26/2019
ms.author: v-junlch
ms.openlocfilehash: 64fe4f9ba611fd13d6d29291257be01b4c3cdabc
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63853509"
---
# <a name="delete-an-azure-cache-for-redis"></a>删除 Azure Redis 缓存

在本方案中，了解如何删除 Azure Redis 缓存。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#/bin/bash

# Delete a Redis Cache named contosoCache from the Resource Group contosoGroup
az redis delete --name contosoCache --resource-group contosoGroup

```

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令删除 Azure Redis 缓存实例。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az redis 删除](/cli/redis) | 删除 Azure Redis 缓存实例。 |


## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

可以在 [Azure Redis 缓存文档](../cli-samples.md)中找到其他 Azure Redis 缓存 CLI 脚本示例。

<!-- Update_Description: update metedata properties -->