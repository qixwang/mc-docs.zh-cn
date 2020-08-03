---
title: 基于 Azure 防火墙威胁智能的筛选
description: 可以为防火墙启用基于威胁智能的筛选，以提醒和拒绝来自/到达已知恶意 IP 地址和域的流量。
services: firewall
author: rockboyfor
ms.service: firewall
ms.topic: article
origin.date: 05/12/2020
ms.date: 08/03/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 00cd2d27ddc1276aefbe788ab633ba70766aca86
ms.sourcegitcommit: 362814dc7ac5b56cf0237b9016a67c35d8d72c32
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/31/2020
ms.locfileid: "87455582"
---
# <a name="azure-firewall-threat-intelligence-based-filtering"></a>基于 Azure 防火墙威胁智能的筛选

可以为防火墙启用基于威胁智能的筛选，以提醒和拒绝来自/到达已知恶意 IP 地址和域的流量。 IP 地址和域源自 Azure 威胁智能源。 [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) 支持 Azure 威胁情报，并已由多种服务（包括 Azure 安全中心）使用。<br />
<br />

:::image type="content" source="media/threat-intel/firewall-threat.png" alt-text="防火墙威胁智能" border="false":::

如果已启用基于威胁智能的筛选，则将在任何 NAT 规则、网络规则或应用程序规则之前处理相关规则。

可以选择仅在触发规则时记录警报，也可以选择警报和拒绝模式。

默认情况下，基于威胁智能的筛选在警报模式下启用。 门户界面在你所在的区域中推出之前，你无法关闭此功能或更改模式。

:::image type="content" source="media/threat-intel/threat-intel-ui.png" alt-text="基于威胁智能的筛选门户界面":::

## <a name="logs"></a>日志

以下日志摘录显示了一个触发的规则：

```
{
    "category": "AzureFirewallNetworkRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallThreatIntelLog",
    "properties": {
         "msg": "HTTP request from 10.0.0.5:54074 to somemaliciousdomain.com:80. Action: Alert. ThreatIntel: Bot Networks"
    }
}
```

## <a name="testing"></a>测试

- 出站测试 - 出站流量警报应该比较罕见，因为这意味着环境已泄露。 为了帮助测试出站警报是否正常工作，已创建一个触发警报的测试 FQDN。 使用 **testmaliciousdomain.chinaeast.cloudapp.chinacloudapi.cn** 进行出站测试。

- 入站测试 - 如果在防火墙上配置了 DNAT 规则，则预计可以看到传入流量的警报。 即使只允许在 DNAT 规则中使用特定源也是如此，否则流量会被拒绝。 Azure 防火墙不会在所有已知的端口扫描仪上发出警报；仅在已知也会参与恶意活动的扫描仪上发出警报。

## <a name="next-steps"></a>后续步骤

- 请参阅 [Azure 防火墙 Log Analytics 示例](log-analytics-samples.md)
- 了解如何[部署和配置 Azure 防火墙](tutorial-firewall-deploy-portal.md)
- 查看 [Microsoft 安全智能报告](https://www.microsoft.com/security/operations/security-intelligence-report)

<!-- Update_Description: update meta properties, wording update, update link -->