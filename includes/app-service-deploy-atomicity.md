---
title: include 文件
description: include 文件
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/07/2020
origin.date: 02/17/2020
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: 076a763feda549406f8f452dea6efce77c5961ed
ms.sourcegitcommit: ee2a3063185cd4c5dc24901366dbb726119d045d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2020
ms.locfileid: "76979349"
---
## <a name="what-happens-to-my-app-during-deployment"></a>部署期间我的应用会发生什么情况？

所有官方支持的部署方法都会更改应用的 `/home/site/wwwroot` 文件夹中的文件。 这些文件用于运行你的应用。 因此，部署可能会因为文件锁定而失败。 应用在部署期间的行为也可能无法预测，因为并非所有文件都同时更新。 这对于面向客户的应用来说是不合需要的。 可通过多种不同方式避免这些问题：

- [直接从 ZIP 包运行应用](../articles/app-service/deploy-run-package.md)，而不将其解压缩。
- 部署期间停止应用或对应用启用脱机模式。 有关详细信息，请参阅[处理部署过程中锁定的文件](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment)。
- 部署到[过渡槽](../articles/app-service/deploy-staging-slots.md)且启用了[自动交换](../articles/app-service/deploy-staging-slots.md#configure-auto-swap)。 
