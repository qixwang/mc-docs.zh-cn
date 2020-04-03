---
title: Azure CLI 脚本示例 - 重置帐户凭据 | Microsoft Docs
description: 使用 Azure CLI 脚本重置帐户凭据和恢复 app.config 设置。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
origin.date: 08/20/2019
ms.date: 04/06/2020
ms.author: v-jay
ms.openlocfilehash: ef402969131d6acaca600049e9fcdc6c3f9a80cf
ms.sourcegitcommit: fe9ed98aaee287a21648f866bb77cb6888f75b0c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80625717"
---
# <a name="azure-cli-example-reset-the-account-credentials"></a>Azure CLI 示例：重置帐户凭据

本文中的 Azure CLI 脚本演示如何重置帐户凭据和恢复 app.config 设置。

## <a name="prerequisites"></a>先决条件

[创建媒体服务帐户](create-account-cli-how-to.md)。

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>示例脚本

```azurecli
# Update the following variables for your own settings:
$resourceGroup=amsResourceGroup
$amsAccountName=amsmediaaccountname

az ams account sp reset-credentials \
  --account-name $amsAccountName \
  --resource-group $resourceGroup
```

## <a name="next-steps"></a>后续步骤

* [az ams](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [重置凭据](https://docs.microsoft.com/cli/azure/ams/account/sp?view=azure-cli-latest#az-ams-account-sp-reset-credentials)
