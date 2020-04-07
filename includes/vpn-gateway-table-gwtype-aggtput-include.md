---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 11/12/2019
ms.date: 04/06/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 15f485666a0addd00a1b4bfa4c0cb93663ce4082
ms.sourcegitcommit: 5fb45da006859215edc8211481f13174aa43dbeb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2020
ms.locfileid: "80634546"
---
|**SKU**   | **S2S/VNet 到 VNet<br>隧道** | **P2S<br> SSTP 连接** | **P2S<br> IKEv2 连接** | **聚合<br>吞吐量基准** | **BGP** |
|---         | ---        | ---       | ---            | ---       | ---       | ---|
|**基本**   | 最大 10 个    | 最大 128  | 不支持  | 100 Mbps  | 不支持|
|**VpnGw1**  | 最大 30   | 最大 128  | 最大 250       | 650 Mbps  | 支持 |
|**VpnGw2**  | 最大 30   | 最大 128  | 最大 500       | 1 Gbps    | 支持 |
|**VpnGw3**  | 最大 30   | 最大 128  | 最大 1000      | 1.25 Gbps | 支持 |
|            |            |           |                |           |           |     |
|**VpnGw2**  | 最大 30   | 最大 128  | 最大 500       | 1.25 Gbps | 支持 |
|**VpnGw3**  | 最大 30   | 最大 128  | 最大 1000      | 2.5 Gbps  | 支持 |
|**VpnGw4**  | 最大 30   | 最大 128  | 最大 1000      | 5 Gbps    | 支持 |
|**VpnGw5**  | 最大 30   | 最大 128  | 最大 1000      | 10 Gbps   | 支持 |

* 允许调整 VpnGw SKU 的大小，但基本 SKU 的大小调整除外。 基本 SKU 是旧版 SKU，并且具有功能限制。 若要从基本 SKU 移到其他 VpnGw SKU，必须删除基本 SKU VPN 网关，并使用所需 SKU 大小创建新网关。

* 这些连接限制是独立的。 例如，在 VpnGw1 SKU 上可以有 128 个 SSTP 连接，还可以有 250 个 IKEv2 连接。

* 可在 [定价](https://www.azure.cn/pricing/details/vpn-gateway) 页上找到定价信息。

* 可在 [SLA](https://www.azure.cn/support/sla/vpn-gateway/) 页上查看 SLA（服务级别协议）信息。

* 在单个隧道中，最多可以达到 1 Gbps 的吞吐量。 上表中的聚合吞吐量基准基于对通过单个网关聚合的多个隧道的测量。 适用于 VPN 网关的聚合吞吐量基准组合了 S2S 和 P2S。 **如果有大量的 P2S 连接，则可能会对 S2S 连接造成负面影响，因为存在吞吐量限制。** 受 Internet 流量情况和应用程序行为影响，无法保证聚合吞吐量基准。

为了帮助我们的客户了解使用不同算法的 SKU 的相对性能，我们使用市售 iPerf 和 CTSTraffic 工具来衡量性能。 下表列出了 VpnGw SKU 的性能测试结果。 可以看到，对 IPsec 加密和完整性使用 GCMAES256 算法时，可获得最佳性能。 对 IPsec 加密使用 AES256 以及对完整性使用 SHA256 时，可获得平均性能。 对 IPsec 加密使用 DES3 以及对完整性使用 SHA256 可获得最低性能。

|**SKU**   | **使用<br>的算法** | **观察到的<br>吞吐量** | **观察到的<br>每秒数据包数** |
|---       | ---                 | ---            | ---                    |
|**VpnGw1**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 650 Mbps<br>500 Mbps<br>120 Mbps   | 58,000<br>50,000<br>50,000|
|**VpnGw2**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1 Gbps<br>500 Mbps<br>120 Mbps | 90,000<br>80,000<br>55,000|
|**VpnGw3**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1.25 Gbps<br>550 Mbps<br>120 Mbps | 105,000<br>90,000<br>60,000|
