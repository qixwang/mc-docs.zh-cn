---
title: Azure CLI 示例 - 为域名创建 DNS 区域和记录
description: 此 Azure CLI 脚本示例演示如何为域名创建 DNS 区域和记录
services: dns
author: WenJason
ms.service: dns
ms.topic: sample
origin.date: 09/20/2019
ms.date: 11/04/2019
ms.author: v-jay
ms.openlocfilehash: 38996de5bf9e818c4cd79eb222f47468b793f2bb
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73416288"
---
# <a name="azure-cli-script-example-create-a-dns-zone-and-record"></a>Azure CLI 脚本示例：创建 DNS 区域和记录

此 Azure CLI 脚本示例为域名创建 DNS 区域和记录。 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Create a resource group.
az group create \
  -n myResourceGroup \
  -l chinaeast

# Create a DNS zone. Substitute zone name "contoso.com" with the values for your own.

az network dns zone create \
  -g MyResourceGroup \
  -n contoso.com

# Create a DNS record. Substitute zone name "contoso.com" and IP address "1.2.3.4* with the values for your own.

az network dns record-set a add-record \
  --g MyResourceGroup \
  --z contoso.com \
  --n www \
  --a 1.2.3.4

# Get a list the DNS records in your zone
az network dns record-set list \
  -g MyResourceGroup \ 
  -z contoso.com
```

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令以删除资源组、DNS 区域和所有相关资源。

```azurecli
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟机、可用性集、负载均衡器和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az network dns zone create](/cli/network/dns/zone#az-network-dns-zone-create) | 创建 Azure DNS 区域。 |
| [az network dns record-set a add-record](/cli/network/dns/record-set) | 将 *A* 记录添加到 DNS 区域。 |
| [az network dns record-set list](/cli/network/dns/record-set) | 列出 DNS 区域中的所有 *A* 记录集。 |
| [az group delete](/cli/vm/extension#az-vm-extension-set) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/)。

