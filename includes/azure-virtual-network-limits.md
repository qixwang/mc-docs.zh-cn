---
title: include 文件
description: include 文件
services: networking
author: rockboyfor
ms.service: networking
ms.topic: include
origin.date: 12/09/2019
ms.date: 01/13/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 12e78397cf2b9cc1b86639ca23a4387f87760f0e
ms.sourcegitcommit: bc5f8b4f8ccd7c723f64055825508d1dfcc2162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2020
ms.locfileid: "75865150"
---
<a name="azure-resource-manager-virtual-networking-limits"></a> 网络限制 - Azure 资源管理器 以下限制仅适用于通过每个订阅的每个区域的 Azure 资源管理器进行管理的网络资源  。 了解如何[针对订阅限制查看当前资源使用情况](../articles/networking/check-usage-against-limits.md)。

> [!NOTE]
> 我们最近将所有默认限制提高到了最大限制。 如果没有最大限制列，则资源没有可调整的限制。 如果过去已通过客户支持提高了这些上限，因此在以下表中看不到更新的限制，可[免费提交联机客户支持请求](../articles/azure-resource-manager/resource-manager-quota-errors.md)

| 资源 | 默认/最大限制 | 
| --- | --- |
| 虚拟网络 |1,000 |
| 每个虚拟网络的子网数 |3,000 |
| 每个虚拟网络的虚拟网络对等互连数 |500 |
| 每个虚拟网络的 DNS 服务器数 |20 个 |
| 每个虚拟网络的专用 IP 地址数 |65,536 |
| 每个网络接口的专用 IP 地址数 |256 |
| 每个虚拟机的专用 IP 地址数 |256 |
| [虚拟机或角色实例的单 NIC 并发 TCP 或 UDP 流数](../articles/virtual-network/virtual-machine-network-throughput.md#flow-limits-and-recommendations) |500,000 |
| 网络接口卡数 |65,536 |
| 网络安全组 |5,000 |
| 每个 NSG 的 NSG 规则数 |1,000 |
| 应用程序安全组 |3,000 |
| 用户定义路由表数 |200 |
| 每个路由表的用户定义的路由数 |400 |
| 每个 Azure VPN 网关的点到站点根证书数 |20 个 |
| 虚拟网络 TAP |100 |
| 每个虚拟网络 TAP 的网络接口 TAP 配置 |100 |

<!-- Not Available on |             IP addresses and ranges specified for source or destination in a security rule              |-->
<!-- Not Available on |                        Application security groups per IP configuration, per NIC                        |-->
<!-- Not Available on |                            IP configurations per application security group                             |-->
<!-- Not Available on | Application security groups that can be specified within all security rules of a network security group |-->

<a name="publicip-address"></a>
#### <a name="public-ip-address-limits"></a>公共 IP 地址限制
| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 公共 IP 地址数 - 动态 | 基本版为 1,000。 |请联系支持人员。 |
| 公共 IP 地址数 - 静态 | 基本版为 1,000。 |请联系支持人员。 |
| 公共 IP 地址数 - 静态 | 标准版为 1000。|请联系支持人员。 |
| 公共 IP 前缀长度 | /28 | 请联系支持人员。 |

<a name="load-balancer"></a>
#### <a name="load-balancer-limits"></a>负载均衡器限制
以下限制仅适用于通过每个订阅的每个区域的 Azure Resource Manager 进行管理的网络资源。 了解如何[针对订阅限制查看当前资源使用情况](../articles/networking/check-usage-against-limits.md)。

| 资源 | 默认/最大限制 |
| --- | --- |
| 负载均衡器 | 1,000 | 
| 每个资源的规则数，基本 | 250 |
| 前端 IP 配置，基本 | 200 |
| 后端池，基本 | 100，单个可用性集 |

<!-- Not Avaiable on Rules per resource, Standard  -->
<!-- Not Avaiable on Rules per IP configuration -->
<!-- Not Avaiable on Frontend IP configurations, Standard -->
<!-- Not Avaiable on | Backend pool, Standard | 1000, single VNet | contact support | -->
<!-- Not Avaiable on | Backend resources per Load Balancer, Standard * | 150 | 150 |-->
<!-- Not Avaiable on | HA Ports, Standard | 1 per internal frontend | - | -->

<!--MOONCAKE: Not Available on Backend resources per Load Balancer, Standard * | 150 |-->
<!--Not Available on <sup>1</sup>The limit is up to 150 resources, in any combination of standalone virtual machine resources, availability set resources, and virtual machine scale-set resources.-->

<a name="virtual-networking-limits-classic"></a>
#### <a name="the-following-limits-apply-only-for-networking-resources-managed-through-the-classic-deployment-model-per-subscription-learn-how-to-view-your-current-resource-usage-against-your-subscription-limitsarticlesnetworkingcheck-usage-against-limitsmd"></a>以下限制仅适用于每个订阅通过**经典**部署模型托管的网络资源。 了解如何[针对订阅限制查看当前资源使用情况](../articles/networking/check-usage-against-limits.md)。

| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 虚拟网络 |100 |100 |
| 本地网络站点 |20 个 |请联系支持人员。 |
| 每个虚拟网络的 DNS 服务器数 |20 个 |20 个 |
| 每个虚拟网络的专用 IP 地址数 |4,096 |4,096 |
| 虚拟机或角色实例的单 NIC 并发 TCP 或 UDP 流数 |如果 NIC 至少有两个，则为 500,000（至多 1,000,000）。 |如果 NIC 至少有两个，则为 500,000（至多 1,000,000）。 |
| 网络安全组 (NSG) |200 |200 |
| 每个 NSG 的 NSG 规则数 |1,000 |1,000 |
| 用户定义路由表数 |200 |200 |
| 每个路由表的用户定义的路由数 |400 |400 |
| 公共 IP 地址 (动态) |500 |500 |
| 保留的公共 IP 地址 |500 |500 |
| 每个部署的公共 VIP |5 |联系支持人员 |
| 每个部署的专用 VIP（内部负载均衡） |1 |1 |
| 终结点访问控制列表 (ACL) |50 |50 |
