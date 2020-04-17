---
title: Azure CLI 脚本示例 - 创建自定义主题 | Microsoft Docs
description: 本文提供了一个示例 Azure CLI 脚本，演示如何创建 Azure 事件网格自定义主题。
services: event-grid
documentationcenter: na
author: lingliw
ms.service: event-grid
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 01/23/2020
ms.date: 3/16/2020
ms.author: v-lingwu
ms.openlocfilehash: 284feb69aedfe7150289e87ba35c0879b5955863
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79452515"
---
# <a name="create-event-grid-custom-topic-with-azure-cli"></a>使用 Azure CLI 创建事件网格自定义主题

此脚本创建事件网格自定义主题。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

<!--[!code-azurecli[main](../../../cli_scripts/event-grid/create-custom-topic/create-custom-topic.sh "Create custom topic")]-->

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建自定义主题。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az eventgrid topic create](/cli/eventgrid/topic#az-eventgrid-topic-create) | 创建事件网格自定义主题。 |


## <a name="next-steps"></a>后续步骤

* 有关查询订阅的信息，请参阅[查询事件网格订阅](../query-event-subscriptions.md)。
* 有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure)。
