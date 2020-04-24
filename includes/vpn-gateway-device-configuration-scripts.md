---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 01/09/2019
ms.date: 01/21/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: bbcd478da5952102a567f6a4116797d1cb96f01b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "68951403"
---
| **供应商** | **设备系列** | **固件版本** |
| --- | --- | --- |
|Cisco | ISR| IOS 15.1（预览版）|
|Cisco | ASA | ASA ( * ) RouteBased（IKEv2- 无 BGP），对于低于 9.8 版的 ASA |
|Cisco | ASA | ASA RouteBased（IKEv2 - 无 BGP），对于 ASA 9.8+ |
|Juniper | SRX_GA | 12.x|
|Juniper | SSG_GA | ScreenOS 6.2.x|
|Juniper | JSeries_GA | JunOS 12.x|
|Juniper | SRX | JunOS 12.x RouteBased BGP |
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased VTI|
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased BGP|

> [!NOTE]
> ( * ) 必需：NarrowAzureTrafficSelectors（启用 UsePolicyBasedTrafficSelectors 选项）和 CustomAzurePolicies (IKE/IPsec)
>
