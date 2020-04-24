---
title: include 文件
description: include 文件
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/21/2018
ms.date: 05/08/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 5701b741abac570cb2936e6f02079bb762242dd8
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "69578656"
---
### <a name="step-1-navigate-to-the-virtual-network-gateway"></a>步骤 1：导航到虚拟网络网关

1. 在 [Azure 门户](https://portal.azure.cn)中导航到“所有资源”。  
2. 若要打开“虚拟网关”页，请导航到要删除的虚拟网关，然后单击它。

### <a name="step-2-delete-connections"></a>步骤 2：删除连接

1. 在“虚拟网络网关”页中，单击“连接”以查看连接到该网关的所有连接  。
2. 在连接名称所在的行中，单击“...”  ，并从下拉列表中选择“删除”  。
3. 单击“是”  以确认要删除该连接。 如有多个连接，请删除每个连接。

### <a name="step-3-delete-the-virtual-network-gateway"></a>步骤 3：删除虚拟网络网关

请注意，除了 S2S 配置，如果还有此 VNet 的 P2S 配置，则删除虚拟网关将自动断开所有 P2S 客户端且不发出警告。

1. 在“虚拟网关”页中，单击“概览”  。
2. 在“概述”页上，单击“删除”以删除网关   。

<!-- ms.date: 05/08/2018 -->