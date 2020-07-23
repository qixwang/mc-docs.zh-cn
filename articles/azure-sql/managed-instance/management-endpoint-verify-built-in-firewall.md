---
title: 验证内置防火墙中的端口安全性
description: 了解如何验证 Azure SQL 托管实例中的内置防火墙保护。
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: sstein, carlrab
origin.date: 12/04/2018
ms.date: 07/13/2020
ms.openlocfilehash: 6bf02b3517343a26edc8a5872373f43c9f579a68
ms.sourcegitcommit: fa26665aab1899e35ef7b93ddc3e1631c009dd04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86228121"
---
# <a name="verify-the-azure-sql-managed-instance-built-in-firewall"></a>验证 Azure SQL 托管实例内置防火墙
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL 托管实例[强制入站安全规则](connectivity-architecture-overview.md#mandatory-inbound-security-rules)要求管理端口 9000、9003、1438、1440 和 1452 从保护 SQL 托管实例的网络安全组 (NSG) 上的任何源中开放。 虽然这些端口在 NSG 级别处于开放状态，但它们由内置防火墙在网络级别进行保护。

## <a name="verify-firewall"></a>验证防火墙

若要验证这些端口，请使用任何安全扫描程序工具来测试这些端口。 以下屏幕截图显示了如何使用这些工具之一。

![验证内置防火墙](./media/management-endpoint-verify-built-in-firewall/03_verify_firewall.png)

## <a name="next-steps"></a>后续步骤

有关 SQL 托管实例和连接的详细信息，请参阅 [Azure SQL 托管实例连接体系结构](connectivity-architecture-overview.md)。
