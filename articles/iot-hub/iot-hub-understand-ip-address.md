---
title: 了解 IoT 中心的 IP 地址 | Microsoft Docs
description: 了解如何查询 IoT 中心的 IP 地址及其属性。 IoT 中心的 IP 地址在某些情况下（例如灾难恢复或区域故障转移）可能会更改。
author: philmea
ms.author: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
origin.date: 11/21/2019
ms.date: 12/23/2019
ms.openlocfilehash: 47f9a665a334ee2f2ece58be78d7e3cc89aaa4ec
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75335204"
---
# <a name="iot-hub-ip-addresses"></a>IoT 中心 IP 地址

IoT 中心的 IP 地址前缀在 *AzureIoTHub* [服务标记](../virtual-network/service-tags-overview.md)下定期发布。 若要确保正常运营，IoT 设备必须以出站方式连接到在 *AzureIoTHub* 服务标记下列出的地址前缀。 IoT 应用程序服务还需以出站方式连接到在 *EventHub* 服务标记下列出的地址前缀。


## <a name="best-practices"></a>最佳实践

* IoT 中心的 IP 地址前缀可能会更改。 这些更改在生效之前通过服务标记定期发布。 因此，必须制定流程来定期检索并使用最新的服务标记。 该流程可以通过[服务标记发现 API](../virtual-network/service-tags-overview.md#service-tags-in-on-premises) 自动完成。
* 请使用 *AzureIoTHub.[区域名称]* 标记来确定特定区域中的 IoT 中心终结点使用的 IP 前缀。 如果考虑到数据中心灾难恢复或[区域性故障转移](iot-hub-ha-dr.md)，请确保还允许连接到 IoT 中心的异地配对区域的 IP 前缀。


## <a name="support-for-ipv6"></a>对 IPv6 的支持 

目前 IoT 中心不支持 IPv6。