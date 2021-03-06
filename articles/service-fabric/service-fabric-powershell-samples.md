---
title: Azure PowerShell 示例 - Service Fabric
description: 了解如何使用 Powershell 创建和管理 Azure Service Fabric 群集、应用和服务。
ms.topic: sample
origin.date: 11/29/2018
ms.date: 02/24/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 2042ba41071bce4b0ae36797e45b97300fc0183b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77540987"
---
# <a name="azure-service-fabric-powershell-samples"></a>Azure Service Fabric PowerShell 示例

下表包含用于创建和管理 Service Fabric 群集、应用程序和服务的 PowerShell 脚本示例的链接。

[!INCLUDE [links to azure CLI and service fabric CLI](../../includes/service-fabric-powershell.md)]

| | |
|-|-|
| **创建群集** ||
| [创建群集 (Azure)](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)| 创建 Azure Service Fabric 群集。 |
| **管理群集、节点和基础结构** ||
| [添加应用程序证书](./scripts/service-fabric-powershell-add-application-certificate.md)| 创建密钥保管库的 X509 证书，并将其部署到群集中的虚拟机规模集。 |
| [更新群集 VM 上的 RDP 端口范围](./scripts/service-fabric-powershell-change-rdp-port-range.md)|更改已部署群集中群集节点 VM 上的 RDP 端口范围。|
| [更新群集节点 VM 的管理员用户和密码](./scripts/service-fabric-powershell-change-rdp-user-and-pw.md) | 更新群集节点 VM 的管理员用户名和密码。 |
| [打开负载均衡器的端口](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) | 在 Azure 负载均衡器中打开应用程序端口，以允许特定端口上的入站流量。 |
| [创建入站网络安全组规则](./scripts/service-fabric-powershell-add-nsg-rule.md) | 创建入站网络安全组规则，以允许入站流量发往特定端口上的群集。 |
| **管理应用程序** ||
| [部署应用程序](./scripts/service-fabric-powershell-deploy-application.md)| 将应用程序部署到群集。|
| [升级应用程序](./scripts/service-fabric-powershell-upgrade-application.md)| 升级应用程序。|
| [删除应用程序](./scripts/service-fabric-powershell-remove-application.md)| 从群集中删除应用程序。|

<!-- Update_Description: update meta properties, wording update -->