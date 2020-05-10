---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/04/2020
ms.date: 04/06/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 1ae47b1b55197fe17964a78225bd7ad978fb43ef
ms.sourcegitcommit: 95efd248f5ee3701f671dbd5cfe0aec9c9959a24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82507692"
---
1. 在 [Azure 门户](https://portal.azure.cn)菜单中，选择“创建资源”  。 

   ![在 Azure 门户中创建资源](./media/vpn-gateway-add-gw-rm-portal-include/azure-portal-create-resource.png)
2. 在“在市场中搜索”字段中，键入“虚拟网关”。  在搜索返回的结果中找到“虚拟网关”，并选择该条目  。 在“虚拟网关”页上，选择“创建”   。 这会打开“创建虚拟网关”页  。
3. 在“基本信息”选项卡上，填写虚拟网关的值。 

   ![“创建虚拟网关”页字段](./media/vpn-gateway-add-gw-rm-portal-include/gateway1.png "“创建虚拟网关”页字段")

   ![“创建虚拟网关”页字段](./media/vpn-gateway-add-gw-rm-portal-include/gateway2.png "“创建虚拟网关”页字段")

   - **名称**：为网关命名。 为网关命名与为网关子网命名不同。 它是要创建的网关对象的名称。
   - **网关类型**：选择“VPN”。  VPN 网关使用虚拟网络网关类型“VPN”  。
   - **VPN 类型**：选择为你的配置指定的 VPN 类型。 大多数配置需要''基于路由'' VPN 类型。
   - **SKU**：从下拉列表中选择网关 SKU。 下拉列表中列出的 SKU 取决于选择的 VPN 类型。 有关网关 SKU 的详细信息，请参阅[网关 SKU](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku)。
   - **虚拟网络**：选择要将此网关添加到的虚拟网络。

   **公共 IP 地址**：此设置指定与 VPN 网关关联的公共 IP 地址对象。 创建 VPN 网关后，会将公共 IP 地址动态分配给此对象。 公共 IP 地址只在删除或重新创建网关时更改。 该地址不会因为 VPN 网关大小调整、重置或其他内部维护/升级而更改。

     - **公共 IP 地址**：让“新建”  保持选中状态。
     - **公共 IP 地址名称**：在文本框中，键入公共 IP 地址实例的名称。
     - **分配**：VPN 网关仅支持“动态”。

   **主动-主动模式**：仅当要创建主动-主动网关配置时，才选择“启用主动-主动模式”  。 否则，请将此设置保留未选择状态。

   让“配置 BGP ASN”保留  取消选中状态，除非你的配置特别需要此设置。 如果确实需要此设置，则默认 ASN 为 65515，但可以更改此值。
4. 选择“查看 + 创建”  ，运行验证。 验证通过后，选择“创建”  以部署 VPN 网关。 网关可能需要长达 45 分钟才能完全创建和部署。 可以在网关的“概述”页上查看部署状态。

创建网关后，可以通过在门户中查看虚拟网络，来查看已分配给网关的 IP 地址。 网关显示为连接的设备。
