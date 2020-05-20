---
title: include 文件
description: include 文件
services: app-service
author: msangapu
ms.service: app-service
ms.topic: include
origin.date: 09/18/2018
ms.date: 09/18/2018
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: dd2cdd302576eecbac78385f6439470da0b47419
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71059591"
---
[!INCLUDE [resource group intro text](resource-group.md)]

在 Azure CLI 中，使用 [`az group create`](/cli/group?view=azure-cli-latest#az-group-create) 命令创建资源组。 以下示例在“中国北部”位置创建名为“myResourceGroup”的资源组。 要查看“免费”层中应用服务支持的所有位置，请运行 [`az appservice list-locations --sku FREE`](/cli/appservice?view=azure-cli-latest#az-appservice-list-locations) 命令。

```azurecli
az group create --name myResourceGroup --location "china north"
```

通常在附近的区域中创建资源组和资源。 

此命令完成后，JSON 输出会显示资源组属性。