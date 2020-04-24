---
title: Azure 负载均衡器的安全控制
description: 用于评估负载均衡器的安全控制清单
services: load-balancer
author: WenJason
manager: digimobile
ms.service: load-balancer
ms.topic: conceptual
origin.date: 09/04/2019
ms.date: 12/09/2019
ms.author: v-jay
ms.openlocfilehash: cf4c7133a54aa26fb1aba51296ad7184657098ea
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74884084"
---
# <a name="security-controls-for-azure-load-balancer"></a>Azure 负载均衡器的安全控制

本文介绍 Azure 负载均衡器中内置的安全控制。

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>网络

| 安全控制 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 不适用 | |
| VNet 注入支持| 不适用 | |
| 网络隔离和防火墙支持| 空值 |  |
| 强制隧道支持| 不适用 | |

## <a name="identity"></a>标识

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 身份验证| 空值 |  |
| 授权| 不适用 |  |

## <a name="data-protection"></a>数据保护

| 安全控制 | Yes/No | 注释 |
|---|---|--|
| 服务器端静态加密：Microsoft 管理的密钥 | 空值 | |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 不适用 | |
| 服务器端静态加密：客户管理的密钥 (BYOK) | 不适用 | |
| 列级加密（Azure 数据服务）| 不适用 | |
| 加密的 API 调用| 是 | 通过 [Azure 资源管理器](../azure-resource-manager/index.yml)。 |

## <a name="configuration-management"></a>配置管理

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 空值 |  | 

