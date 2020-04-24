---
title: include 文件
description: include 文件
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/21/2018
ms.date: 08/13/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: f1b75a9ea9fa4f04e5f15fa393a8daa565f9c441
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "69578662"
---
### <a name="to-modify-the-local-network-gateway-ip-address---no-gateway-connection"></a><a name="gwipnoconnection"></a> 修改本地网络网关 IP 地址 - 无网关连接

请使用此示例修改没有网关连接的本地网关。 修改此值时，还可同时修改地址前缀。

1. 在“本地网络网关”资源的“设置”  部分中，单击“配置”  。
2. 在“IP 地址”  框中，修改 IP 地址。
3. 单击“保存”  保存这些设置。

### <a name="to-modify-the-local-network-gateway-ip-address---existing-gateway-connection"></a><a name="gwipwithconnection"></a>修改本地网络网关 IP 地址的具体步骤 - 现有网关连接

若要修改具有连接的本地网络网关，需先删除该连接。 删除连接后，可修改网关 IP 地址并重新创建一个新的连接。 此外可同时修改地址前缀。 这会导致 VPN 连接中断一段时间。 修改网关 IP 地址时，不需删除 VPN 网关。 只需删除连接。
 
#### <a name="1-remove-the-connection"></a>1.删除连接。

1. 在“本地网络网关”资源的“设置”  部分中，单击“连接”  。
2. 在连接的行上单击“...”  ，然后单击“删除”  。
3. 单击“保存”  以保存设置。

#### <a name="2-modify-the-ip-address"></a>2.修改 IP 地址。

此外可同时修改地址前缀。

1. 在“IP 地址”  框中，修改 IP 地址。
2. 单击“保存”  保存这些设置。

#### <a name="3-recreate-the-connection"></a>3.重新创建连接。

1. 导航到 VNet 的虚拟网络网关。 （不是本地网络网关。）
2. 在“虚拟网络网关”的“设置”  部分中，单击“连接”  。
3. 单击“+ 添加”  打开“添加连接”  边栏选项卡。
4. 重新创建连接。
5. 单击“确定”创建连接。 

<!-- ms.date: 08/13/2018 -->