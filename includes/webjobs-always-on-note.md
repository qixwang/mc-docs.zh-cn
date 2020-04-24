---
title: include 文件
description: include 文件
services: app-service
author: ggailey777
ms.service: app-service
ms.topic: include
origin.date: 02/19/2019
ms.date: 03/16/2020
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: 92b69a54a384f19d0486831de4e29835408fba94
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79546910"
---
> [!NOTE]
> Web 应用可在进入非活动状态 20 分钟后超时。 只有向实际 Web 应用发出的请求才会重置计时器。 在 Azure 门户中查看应用的配置或向高级工具站点 (`https://<app_name>.scm.chinacloudsites.cn`) 发出请求不会重置计时器。 如果应用运行连续性或计划的（计时器触发器）WebJobs，可启用 **Always On** 来确保 WebJobs 可靠运行。 此功能仅在基本、标准和高级[定价层](https://www.azure.cn/pricing/details/app-service/)中提供。
