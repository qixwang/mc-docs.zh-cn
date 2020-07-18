---
title: Kusto Ping REST API - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的 Kusto Ping REST API。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/23/2018
ms.date: 07/01/2020
ms.openlocfilehash: bc41127def3a1c2c1f2cf4f91fe132ae1dd90a66
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226276"
---
# <a name="kusto-ping-rest-api"></a>Kusto Ping REST API

提供 ping REST API 是为了允许网络设备（例如负载均衡器）验证群集的无状态前端组件的简单的网络响应能力。 将 GET 谓词应用于此终结点时，群集将通过返回“200 正常”HTTP 响应做出响应。 REST API 未经身份验证（调用方不需要发送 HTTP `Authorization` 标头）。

- 路径：`/v1/rest/ping`
- 谓词：`GET`
- 操作：使用 `200 OK` 消息进行响应
- 参数：无