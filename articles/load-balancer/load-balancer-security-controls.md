---
title: Azure 负载均衡器的安全控制
description: 用于评估负载均衡器的安全控制清单
services: load-balancer
author: WenJason
manager: digimobile
ms.service: load-balancer
ms.topic: conceptual
origin.date: 09/04/2019
ms.date: 06/08/2020
ms.author: v-jay
ms.openlocfilehash: 88a2aa3383e16ea895500d0622a2b8f39e2d65bd
ms.sourcegitcommit: 9811bf312e0d037cb530eb16c8d85238fd276949
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84275408"
---
# <a name="security-controls-for-azure-load-balancer"></a>Azure 负载均衡器的安全控制

本文介绍 Azure 负载均衡器中内置的安全控制。

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>网络

| 安全控制 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 空值 | |
| VNet 注入支持| 空值 | |
| 网络隔离和防火墙支持| 空值 |  |
| 强制隧道支持| 空值 | |

## <a name="monitoring--logging"></a>监视和日志记录

| 安全控制 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | 请参阅[公共基本负载均衡器的 Azure Monitor 日志](load-balancer-monitor-log.md)。 |
| 控制和管理平面日志记录和审核| 是 | 请参阅[公共基本负载均衡器的 Azure Monitor 日志](load-balancer-monitor-log.md)。 |
| 数据平面日志记录和审核 | 空值 |  |

## <a name="identity"></a>标识

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 身份验证| 空值 |  |
| 授权| 空值 |  |

## <a name="data-protection"></a>数据保护

| 安全控制 | Yes/No | 注释 |
|---|---|--|
| 服务器端静态加密：Microsoft 管理的密钥 | 空值 | |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 空值 | |
| 服务器端静态加密：客户管理的密钥 (BYOK) | 空值 | |
| 列级加密（Azure 数据服务）| 空值 | |
| 加密的 API 调用| 是 | 通过 [Azure 资源管理器](../azure-resource-manager/index.yml)。 |

## <a name="configuration-management"></a>配置管理

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 空值 |  | 

