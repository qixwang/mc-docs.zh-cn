---
title: include 文件
description: include 文件
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: include
origin.date: 10/07/2019
ms.date: 06/15/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: afc076f413a64d5469142e1a7a98b3a378990a2e
ms.sourcegitcommit: 8dae792aefbe44e8388f961b813e3da6564423ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84654875"
---
使用自定义 IPsec 策略时，请记住以下要求：

* **IKE** - 对于 IKE，你可以从“IKE 加密”中选择任何参数、从“IKE 完整性”中选择任何参数，以及从“DH 组”中选择任何参数。
* **IPsec** - 对于 IPsec，你可以从“IPsec 加密”中选择任何参数，还可以再从“IPsec 完整性”中选择任何参数，还可以再选择“PFS”。 如果“IPsec 加密”或“IPsec 完整性”的任一参数是 GCM，则这两个设置的参数必须都是 GCM。

>[!NOTE]
> 对于自定义 IPsec 策略，不存在响应方和发起方的概念（与默认 IPsec 策略不同）。 两端（本地和 Azure VPN 网关）将对“IKE 阶段 1”和“IKE 阶段 2”使用相同的设置。 IKEv1 协议和 IKEv2 协议均受支持。 不支持仅将 Azure 作为响应方。
>

**可用的设置和参数**

| 设置 | parameters |
|--- |--- |
| IKE 加密 | GCMAES256、GCMAES128、AES256、AES128 |
| IKE 完整性 | SHA384、SHA256 |
| DH 组 | ECP384、ECP256、DHGroup24、DHGroup14 |
| IPsec 加密 | GCMAES256、GCMAES128、AES256、AES128、None |
| IPsec 完整性 | GCMAES256、GCMAES128、SHA256 |
| PFS 组 | ECP384、ECP256、PFS24、PFS14、None |

<!-- Update_Description: new article about virtual wan ipsec custom include -->
<!--NEW.date: 03/02/2020-->