---
title: Azure 中继的网络安全性
description: 本文介绍如何配置专用终结点的访问权限
ms.topic: conceptual
origin.date: 06/23/2020
ms.date: 07/27/2020
ms.testscope: yes|no
ms.testdate: 07/27/2020Null
ms.author: v-yeche
author: rockboyfor
ms.openlocfilehash: af1c426ef0bb6def7a9a4a550a3ef85d09e20eb6
ms.sourcegitcommit: 091c672fa448b556f4c2c3979e006102d423e9d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87162858"
---
<!--Verified successfully-->
# <a name="network-security-for-azure-relay"></a>Azure 中继的网络安全性 
本文介绍如何将以下安全功能与 Azure 中继配合使用： 

- IP 防火墙规则（预览版）

<!--Not Available on - Private endpoints (preview)-->

> [!NOTE]
> Azure 中继不支持网络服务终结点。 

## <a name="ip-firewall"></a>IP 防火墙 
默认情况下，只要请求附带有效的身份验证和授权，就可以从 Internet 访问中继命名空间。 有了 IP 防火墙，就可以使用 [CIDR（无类别域间路由）](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)表示法将其进一步限制为仅一组 IPv4 地址或 IPv4 地址范围。

在仅应从某些知名站点访问 Azure 中继的情况下，此功能很有用。 可以通过防火墙规则来配置规则，以便接受来自特定 IPv4 地址的流量。 例如，如果将中继与 [Azure Express Route](/expressroute/expressroute-faqs#supported-services) 配合使用，则可创建防火墙规则，仅允许来自本地基础结构 IP 地址的流量。 

IP 防火墙规则应用于中继命名空间级别。 因此，这些规则适用于通过任何受支持协议从客户端发出的所有连接。 如果某 IP 地址与中继命名空间的允许 IP 规则不匹配，系统会拒绝来自该地址的任何连接尝试并将其标记为“未经授权”。 响应不会提及 IP 规则。 IP 筛选器规则将按顺序应用，与 IP 地址匹配的第一个规则决定了将执行接受操作还是执行拒绝操作。

有关详细信息，请参阅[如何为中继命名空间配置 IP 防火墙](ip-firewall-virtual-networks.md)

<!--Not Available on ## Private endpoints-->

## <a name="next-steps"></a>后续步骤
请参阅以下文章：

- [如何配置 IP 防火墙](ip-firewall-virtual-networks.md)

<!--Not Available on - [How to configure private endpoints](private-link-service.md)-->

<!-- Update_Description: new article about network security -->
<!--NEW.date: 07/27/2020-->