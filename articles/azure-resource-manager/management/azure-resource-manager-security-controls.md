---
title: 安全控件
description: 用于评估 Azure 资源管理器服务的内置安全控制的清单。
ms.topic: conceptual
origin.date: 09/04/2019
ms.author: v-yeche
ms.date: 01/06/2020
ms.openlocfilehash: 663683ed8e34499633ba1837dbe1a26678ff4879
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75631406"
---
<!--Verify successfully-->
# <a name="security-controls-for-azure-resource-manager"></a>Azure 资源管理器的安全控制

本文介绍 Azure 资源管理器中内置的安全控制。

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="data-protection"></a>数据保护

| 安全控制 | Yes/No | 注释 |
|---|---|--|
| 服务器端静态加密：Azure 托管的密钥 | 是 |  |
| 传输中加密（例如 ExpressRoute 加密、VNet 中加密，以及 VNet-VNet 加密）| 是 | HTTPS/TLS。 |
| 服务器端静态加密：客户管理的密钥 (BYOK) | 不适用 | Azure 资源管理器不存储客户内容，仅存储控制数据。 |
| 列级加密（Azure 数据服务）| 是 | |
| 加密的 API 调用| 是 | |

## <a name="network"></a>网络

| 安全控制 | Yes/No | 注释 |
|---|---|--|
| 服务终结点支持| 否 | |
| VNet 注入支持| 是 | |
| 网络隔离和防火墙支持| 否 |  |
| 强制隧道支持| 否 |  |

## <a name="monitoring--logging"></a>监视和日志记录

| 安全控制 | Yes/No | 注释|
|---|---|--|
| Azure 监视支持（Log Analytics、App Insights 等）| 否 | |
| 控制和管理平面日志记录和审核| 是 | 活动日志公开对资源执行的所有写入操作（PUT、POST、DELETE）；请参阅[查看活动日志来审核对资源的操作](view-activity-logs.md)。 |
| 数据平面日志记录和审核| 不适用 | |

## <a name="identity"></a>标识

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 身份验证| 是 | 基于 [Azure Active Directory](/active-directory)。|
| 授权| 是 | |

## <a name="configuration-management"></a>配置管理

| 安全控制 | Yes/No | 注释|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 |  |

<!--Not Available on ## Next steps-->
<!--Not Available on [built-in security controls across Azure services](../security/fundamentals/security-controls.md)-->

<!-- Update_Description: update meta properties, wording update, update link -->