---
title: include 文件
description: include 文件
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: include
origin.date: 11/04/2019
ms.date: 03/30/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: abaf38938e9a50c0d29ea97079a84987f91c4b3a
ms.sourcegitcommit: 4810b75d1e1db78d9747e99735468a6ab861be2d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80291297"
---
1. 选择“连接 VPN 站点”来打开“连接站点”页   。

    ![connect](./media/virtual-wan-tutorial-connect-vpn-site-include/connect.png "连接")

    完成以下字段：

    * 输入预共享密钥。 如果未输入密钥，Azure 会自动生成一个。
    * 选择“协议”和“IPsec 设置”。 请参阅 [默认/自定义 IPSec 详细信息] (https://docs.azure.cn/virtual-wan/virtual-wan-ipsec)
    * 选择适合“传播默认路由”的选项  。 “启用”选项允许虚拟中心将获知的默认路由传播到此连接  。 只有当虚拟 WAN 中心由于在中心部署防火墙而获知默认路由或另一个连接的站点已启用强制隧道时，此标志才会将默认路由传播到连接。 默认路由不源自虚拟 WAN 中心。

2. 选择“连接”  。
3. 几分钟后，该站点将显示连接和连接状态。

    ![status](./media/virtual-wan-tutorial-connect-vpn-site-include/status.png "状态")

    **连接状态：** 这是将 VPN 站点连接到 Azure 中心 VPN 网关的连接的 Azure 资源状态。 控制平面操作成功后，Azure VPN 网关和本地 VPN 设备将继续建立连接。

    **连接状态：** 这是中心和 VPN 站点中 Azure VPN 网关之间的实际连接（数据路径）状态。 可以显示以下任一状态：

    * **未知**：如果后端系统正在转换到另一状态，则通常会显示此状态。
    * **连接**：Azure VPN 网关正在尝试连接实际的本地 VPN 站点。
    * **已连接**：Azure VPN 网关和本地 VPN 站点之间已建立连接。
    * **断开连接**：如果（在本地或 Azure 中）出于任何原因连接断开，则会显示此状态。
4. 在中心 VPN 站点内，还可以根据情况执行以下操作： 

   * 编辑或删除 VPN 连接。
   * 在 Azure 门户中删除站点。
   * 使用站点旁的上下文 (…) 菜单下载特定于分支的配置以获取有关 Azure 端的详细信息。 如果要为中心内所有连接的站点下载配置，请在顶部菜单选择“下载 VPN 配置”  。

<!-- Update_Description: update meta properties, wording update, update link -->