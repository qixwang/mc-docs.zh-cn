---
title: Azure 防火墙 SNAT 专用 IP 地址范围
description: 可以配置 IP 地址专用范围，使防火墙不会将流量 SNAT 到这些 IP 地址。
services: firewall
author: rockboyfor
ms.service: firewall
ms.topic: article
origin.date: 03/20/2020
ms.date: 06/15/2020
ms.author: v-yeche
ms.openlocfilehash: 9e5d72ed662223c0e27873c07042a579a4c160d5
ms.sourcegitcommit: 285649db9b21169f3136729c041e4d04d323229a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2020
ms.locfileid: "84685449"
---
<!--Verified successfully-->
# <a name="azure-firewall-snat-private-ip-address-ranges"></a>Azure 防火墙 SNAT 专用 IP 地址范围

根据 [IANA RFC 1918](https://tools.ietf.org/html/rfc1918)，当目标 IP 地址处于专用 IP 地址范围内时，Azure 防火墙不会使用网络规则执行 SNAT。 无论目标 IP 地址如何，始终使用[透明代理](https://wikipedia.org/wiki/Proxy_server#Transparent_proxy)来应用应用程序规则。

如果组织对专用网络使用公共 IP 地址范围，Azure 防火墙会通过 SNAT 将流量发送到 AzureFirewallSubnet 中的某个防火墙专用 IP 地址。 但是，可以将 Azure 防火墙配置为不 SNAT 公共 IP 地址范围****。

## <a name="configure-snat-private-ip-address-ranges"></a>配置 SNAT 专用 IP 地址范围

可以使用 Azure PowerShell 来指定防火墙不会在其中进行 SNAT 的 IP 地址范围。

### <a name="new-firewall"></a>新建防火墙

对于新建防火墙，Azure PowerShell 命令如下：

`New-AzFirewall -Name $GatewayName -ResourceGroupName $RG -Location $Location -VirtualNetworkName $vnet.Name -PublicIpName $LBPip.Name -PrivateRange @("IANAPrivateRanges","IPRange1", "IPRange2")`

> [!NOTE]
> 将其他范围添加到 Azure 防火墙中时，IANAPrivateRanges 将扩展到 Azure 防火墙上的当前默认值。

有关详细信息，请参阅 [New-AzFirewall](https://docs.microsoft.com/powershell/module/az.network/new-azfirewall?view=azps-3.3.0)。

### <a name="existing-firewall"></a>现有防火墙

若要配置现有防火墙，请使用以下 Azure PowerShell 命令：

```azurepowershell
$azfw = Get-AzFirewall -ResourceGroupName "Firewall Resource Group name"
$azfw.PrivateRange = @("IANAPrivateRanges","IPRange1", "IPRange2")
Set-AzFirewall -AzureFirewall $azfw
```

### <a name="templates"></a>模板

可以将以下内容添加到 `additionalProperties` 部分：

```
"additionalProperties": {
                    "Network.SNAT.PrivateRanges": "IANAPrivateRanges , IPRange1, IPRange2"
                },
```

## <a name="next-steps"></a>后续步骤

- 了解如何[部署和配置 Azure 防火墙](tutorial-firewall-deploy-portal.md)。

<!-- Update_Description: new article about snat private range -->
<!--NEW.date: 06/15/2020-->