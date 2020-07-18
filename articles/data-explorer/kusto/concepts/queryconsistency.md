---
title: 查询一致性 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的查询一致性。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 01/20/2019
ms.date: 07/01/2020
ms.openlocfilehash: 70f30821cede2687f419cadda12ff6820a152917
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226174"
---
# <a name="query-consistency"></a>查询一致性

Kusto 支持两种查询一致性模型：强一致性查询和弱一致性查询。

强一致性查询（默认）具有“读取我的更改”保证。 如果你发送控制命令并收到“命令已成功完成”的确认，那么可以保证紧随其后的任何查询都会观察到该命令的结果。

必须由客户端显式启用的弱一致性查询没有该保证。 进行查询的客户端可能会在更改与反映这些更改的查询之间观察到一些延迟（通常为 1-2 分钟）。

弱一致性查询的优点在于，它可以减少处理数据库更改的群集节点上的负载。 通常，我们建议你首先尝试强一致性模型。 仅在必要时切换到弱一致性查询。

若要切换到弱一致性查询，请在进行 [REST API 调用](../api/rest/request.md)时设置 `queryconsistency` 属性。 .NET 客户端的用户还可以在 [Kusto 连接字符串](../api/connection-strings/kusto.md)中设置它，也可以在[客户端请求属性](../api/netfx/request-properties.md)中将其设置为标志。
