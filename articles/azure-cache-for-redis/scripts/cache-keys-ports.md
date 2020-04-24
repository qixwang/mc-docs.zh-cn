---
title: Azure CLI 脚本示例 - 获取 Azure Redis 缓存的主机名、端口和密钥 | Microsoft Docs
description: Azure CLI 脚本示例 - 获取 Azure Redis 缓存的主机名、端口和密钥
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
origin.date: 08/30/2017
ms.date: 02/26/2019
ms.author: v-junlch
ms.openlocfilehash: c4fa85a75b2995f332abae7fca656a2cc6b26875
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63856889"
---
# <a name="get-the-hostname-ports-and-keys-for-azure-cache-for-redis"></a>获取 Azure Redis 缓存的主机名、端口和密钥

在此方案中，将学习如何检索用于连接到 Azure Redis 缓存实例的主机名、端口和密钥。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#/bin/bash

# Retrieve the hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve the hostname and ports for an Azure Redis Cache instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve the keys for an Azure Redis Cache instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display the retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令检索 Azure Redis 缓存实例的主机名、密钥和端口。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az redis show](/cli/redis) | 检索 Azure Redis 缓存实例的详细信息。 |
| [az redis list-keys](/cli/redis) | 检索 Azure Redis 缓存实例的访问密钥。 |


## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

可以在 [Azure Redis 缓存文档](../cli-samples.md)中找到其他 Azure Redis 缓存 CLI 脚本示例。

<!-- Update_Description: update metedata properties -->