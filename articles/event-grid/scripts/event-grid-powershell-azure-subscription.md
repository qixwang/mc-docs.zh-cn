---
title: Azure PowerShell - 订阅 Azure 订阅
description: 本文提供了一个示例 Azure PowerShell 脚本，演示如何订阅 Azure 订阅的事件网格事件。
services: event-grid
documentationcenter: na
author: lingliw
manager: digimobile
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 01/23/2020
ms.date: 3/16/2020
ms.author: v-lingwu
ms.openlocfilehash: 861bdcdd5dfe1e5db8065dd32e59d7eee30238c0
ms.sourcegitcommit: 7995ca87e9e10388948f714f94c61d66880f3bb3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2020
ms.locfileid: "79452479"
---
# <a name="subscribe-to-events-for-an-azure-subscription-with-powershell"></a>使用 PowerShell 订阅 Azure 订阅的事件

此脚本创建 Azure 订阅事件的事件网格订阅。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script---stable"></a>示例脚本 - 稳定版

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

<!--[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-azure-subscription/subscribe-to-azure-subscription.ps1 "Subscribe to Azure subscription")]-->

## <a name="sample-script---preview-module"></a>示例脚本 - 预览模块

此预览示例脚本需要事件网格模块。 若要安装，请运行 `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

<!--[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-azure-subscription-preview/subscribe-to-azure-subscription-preview.ps1 "Subscribe to Azure subscription")]-->

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建事件订阅。 表中的每条命令链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzEventGridSubscription](https://docs.microsoft.com/powershell/module/az.eventgrid/new-azeventgridsubscription) | 创建事件网格订阅。 |

## <a name="next-steps"></a>后续步骤

* 有关托管应用程序的简介，请参阅 [Azure 托管应用程序概述](../overview.md)。
* 有关 PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](https://docs.microsoft.com/powershell/azure/get-started-azureps)。
