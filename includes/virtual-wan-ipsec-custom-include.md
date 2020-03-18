---
title: include 文件
description: include 文件
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: include
origin.date: 10/07/2019
ms.date: 03/16/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 0cbfc94f9c5c13b3ce8e25cd4d4e0fcb05ffb34c
ms.sourcegitcommit: e94ed1c9eff4e88be2ca389909e60b14cc0d92f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084399"
---
使用自定义 IPsec 策略时，请记住以下要求：

* **IKE** - 对于 IKE，你可以从“IKE 加密”中选择任何参数、从“IKE 完整性”中选择任何参数，以及从“DH 组”中选择任何参数。
* **IPsec** - 对于 IPsec，你可以从“IPsec 加密”中选择任何参数，还可以从“IPsec 完整性”和“PFS 组”中选择任何参数。 如果“IPsec 加密”或“IPsec 完整性”的任一参数是 GCM，则这两个设置的参数必须都是 GCM。

>[!NOTE]
> 对于自定义 IPsec 策略，不存在响应方和发起方的概念（与默认 IPsec 策略不同）。 两端（本地和 Azure VPN 网关）将对“IKE 阶段 1”和“IKE 阶段 2”使用相同的设置。 IKEv1 协议和 IKEv2 协议均受支持。 不支持仅将 Azure 作为响应方。
>

**可用的设置和参数**

| 设置 | 参数 |
|--- |--- |
| IKE 加密 | AES256、AES192、AES128 |
| IKE 完整性 | SHA384、SHA256、SHA1 |
| DH 组 | DHGroup24、ECP384、ECP256、DHGroup14、DHGroup2048、DHGroup2 |
| IPsec 加密 | GCMAES256、GCMAES192、GCMAES128、AES256、AES192、AES128 |
| IPsec 完整性 | GCMASE256、GCMAES192、GCMAES128、SHA256、SHA1 |
| PFS 组 | PFS24、ECP384、ECP256、PFS2048、PFS2 |

<!-- Update_Description: new article about virtual wan ipsec custom include -->
<!--NEW.date: 03/02/2020-->