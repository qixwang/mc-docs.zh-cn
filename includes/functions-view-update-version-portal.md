---
title: include 文件
description: include 文件
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/05/2020
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 51546f5db483d0515442ac030d618c6153ed7277
ms.sourcegitcommit: f1a76ee3242698123a3d77f44c860db040b48f70
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574663"
---
使用以下步骤查看并更新函数应用当前使用的运行时版本。

1. 在 [Azure 门户](https://portal.azure.cn)中，浏览到你的函数应用。

1. 在“设置”下，选择“配置”**** ****。 在“函数运行设置”选项卡中，定位“运行时版本”**** ****。 请注意特定的运行时版本。 在以下示例中，版本设置为 `~3`。

    :::image type="content" source="./media/functions-view-update-version-portal/functions-view-runtime-version.png" alt-text="查看运行时版本。" border="true":::

1. 若要将函数应用固定到 1.x 版运行时，请在“运行时版本”下选择“~1”。**** **** 当应用中存在函数时，此开关被禁用。

1. 如果更改了运行时版本，请返回到“概览”**** 选项卡并选择“重启”**** 以重启应用。  函数应用重新开始在 1.x 版运行时上运行，并且在创建函数时将使用 1.x 版模板。

    :::image type="content" source="./media/functions-view-update-version-portal/functions-restart-function-app.png" alt-text="重新启动你的函数应用。" border="true":::

