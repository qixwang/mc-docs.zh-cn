---
title: include 文件
description: include 文件
services: billing
author: rothja
ms.service: billing
ms.topic: include
origin.date: 05/18/2018
ms.date: 09/26/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 193817f4f35df2f4c0e31666e31d5de108291a18
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75500407"
---
| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 每个[订阅](https://www.azure.cn/pricing)的 vCPU 数量 <sup>1</sup> |20 个 |10,000 |
| [协同管理员数](/billing/billing-add-change-azure-subscription-administrator) |200 |200 |
| 每个订阅在每个区域中的[存储帐户数](../articles/storage/common/storage-quickstart-create-account.md)<sup>2</sup> |200 |250 |
| 每个订阅的[云服务数](../articles/cloud-services/cloud-services-choose-me.md) |20 个 |200 |
| 每个订阅的[本地网络数](https://msdn.microsoft.com/library/jj157100.aspx) |10 个 |500 |
| 每个订阅的 SQL 数据库服务器数 |6 |200 |
| 每个订阅的 DNS 服务器 |9 |100 |
| 每个订阅的保留的 IP |20 个 |100 |
| 每个订阅的托管服务证书数 |199 |199 |
| 每个订阅的[地缘组数](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) |256 |256 |


<sup>1</sup>特小实例作为一个 vCPU 至 vCPU 限制计数，即使使用了部分 CPU 核心。

<sup>2</sup>此存储帐户限制包括标准和高级存储帐户。 如果在某特定区域中需要的存储帐户多于 200 个，请通过 [Azure 支持](https://www.azure.cn/support/faq/)提出请求。 Azure 存储团队将评审业务案例，对于特定区域最多可以批准 250 个存储帐户。 

<!-- ms.date: 09/26/2018 -->
