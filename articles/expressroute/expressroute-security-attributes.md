---
title: Azure ExpressRoute 的安全属性
description: 用于评估 Azure ExpressRoute 的安全属性的清单
services: expressroute
ms.service: expressroute
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
origin.date: 06/05/2019
ms.date: 09/09/2019
ms.author: v-yiso
ms.openlocfilehash: 66c2f2b048f5138a366f45232ed8d7b381ee2fc6
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70174281"
---
# <a name="security-attributes-for-azure-expressroute"></a>Azure ExpressRoute 的安全属性

本文介绍了 Azure ExpressRoute 中内置的安全属性。

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>预防

| 安全属性 | Yes/No | 说明 |
|---|---|--|
| 静态加密（例如服务器端加密、带客户托管密钥的服务器端加密，以及其他加密功能）|  空值 | ExpressRoute 不存储客户数据。 |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 否 | |
| 加密密钥处理（CMK、BYOK 等）| 空值 |  |
| 列级加密（Azure 数据服务）| 空值 | |
| 加密的 API 调用| 是 | 通过 [Azure 资源管理器](../azure-resource-manager/index.yml)和 HTTPS。 |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | Yes/No | 说明 |
|---|---|--|
| 服务终结点支持| 空值 |  |
| VNet 注入支持| 空值 | |
| 网络隔离和防火墙支持| 是 | 每个客户都包含在自己的路由域中，并通过隧道连接到自己的 VNet |
| 强制隧道支持| 空值 | 通过边界网关协议 (BGP)。 |

## <a name="detection"></a>检测

| 安全属性 | Yes/No | 说明|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 是 | 请参阅 [ExpressRoute 监视、指标和警报](expressroute-monitoring-metrics-alerts.md)。|

## <a name="identity-and-access-management"></a>标识和访问管理

| 安全属性 | Yes/No | 说明|
|---|---|--|
| Authentication| 是 | 用于 Microsoft 的网关 (GWM)（控制器）的服务帐户；用于开发和操作的实时 (JIT) 访问。 |
| 授权|  是 |用于 Microsoft 的网关 (GWM)（控制器）的服务帐户；用于开发和操作的实时 (JIT) 访问。 |


## <a name="audit-trail"></a>审核线索

| 安全属性 | Yes/No | 说明| 
|---|---|--|
| 控制和管理平面日志记录和审核| 是 |  |
| 数据平面日志记录和审核| 否 |   |

## <a name="configuration-management"></a>配置管理

| 安全属性 | Yes/No | 说明|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 | 通过网络资源提供程序 (NRP)。 |
