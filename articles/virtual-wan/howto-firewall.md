---
title: 在虚拟 WAN 中心安装 Azure 防火墙
titleSuffix: Azure Virtual WAN
description: 在虚拟 WAN 中心配置 Azure 防火墙的步骤
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: conceptual
origin.date: 05/21/2020
ms.date: 06/15/2020
ms.author: v-yeche
ms.openlocfilehash: b2b7c11626a0d4f955d1b1734df0474928bf2a5b
ms.sourcegitcommit: 8dae792aefbe44e8388f961b813e3da6564423ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84655025"
---
<!--NEED CONFIRM BEFORE RELASEMENT-->
<!--Schedule Release Date: 06/30/2020-->
# <a name="configure-azure-firewall-in-a-virtual-wan-hub"></a>在虚拟 WAN 中心配置 Azure 防火墙

安全中心是使用 Azure 防火墙的 Azure 虚拟 WAN 中心。 本文将指导你完成通过直接从 Azure 虚拟 WAN 门户页面安装 Azure 防火墙将虚拟 WAN 中心转换为安全中心的步骤。

## <a name="before-you-begin"></a>开始之前

本文中的步骤假设你已部署了具有一个或多个中心的虚拟 WAN。

若要创建新虚拟 WAN 和新中心，请使用以下文章中的步骤：

* [创建虚拟 WAN](virtual-wan-site-to-site-portal.md#openvwan)
* [创建中心](virtual-wan-site-to-site-portal.md#hub)

## <a name="view-virtual-hubs"></a>查看虚拟中心

虚拟 WAN 的“概述”页面显示了虚拟中心和安全中心的列表。 下图显示没有安全中心的虚拟 WAN。

[ ![概述](./media/howto-firewall/overview.png)](./media/howto-firewall/overview.png#lightbox)

## <a name="convert-to-secured-hub"></a>转换为安全中心

1. 在虚拟 WAN 的“概述”页面上，选择要转换为安全中心的中心。 在虚拟中心页面上，可看到两个选项，用于将 Azure 防火墙部署到此中心。 选择任一选项。

    [ ![安全性](./media/howto-firewall/security.png)](./media/howto-firewall/security.png#lightbox)

1. 选择其中一个选项之后，会看到“转换为安全中心”页面。 选择要转换的中心，然后选择页面底部的“下一步:Azure 防火墙”。

    [ ![选择中心](./media/howto-firewall/select-hub.png)](./media/howto-firewall/select-hub.png#lightbox)
1. 完成工作流之后，选择“确认”。

    [ ![确认](./media/howto-firewall/confirm.png)](./media/howto-firewall/confirm.png#lightbox)

1. 将中心转换为安全中心之后，可在虚拟 WAN“概述”页面上查看它。

    [ ![查看安全中心](./media/howto-firewall/secured-hub.png)](./media/howto-firewall/secured-hub.png#lightbox)

## <a name="view-hub-resources"></a>查看中心资源

从虚拟 WAN“概述”页面中，选择安全中心。 在中心页面上，可以查看所有虚拟中心资源，包括 Azure 防火墙。

[ ![查看中心资源](./media/howto-firewall/view-resources.png)](./media/howto-firewall/view-resources.png#lightbox)

若要从安全中心查看 Azure 防火墙设置，请在“安全性”下，选择“安全虚拟中心设置”。
[ ![查看中心设置](./media/howto-firewall/hub-settings.png)](./media/howto-firewall/hub-settings.png#lightbox)

## <a name="configure-additional-settings"></a>配置其他设置

若要为虚拟中心配置其他 Azure 防火墙设置，请选择“Azure 防火墙管理器”的链接。 

<!--Not Available on [Azure Firewall Manager](../firewall-manager/secure-cloud-network.md#create-a-firewall-policy-and-secure-your-hub)-->

[ ![其他设置](./media/howto-firewall/additional-settings.png)](./media/howto-firewall/additional-settings.png#lightbox)

若要返回到中心“概述”页面，可以通过单击路径向后导航，如下图中的箭头所示。

[ ![返回到概述](./media/howto-firewall/arrow.png)](./media/howto-firewall/arrow.png#lightbox)

## <a name="next-steps"></a>后续步骤

有关虚拟 WAN 的详细信息，请参阅[常见问题解答](virtual-wan-faq.md)。

<!-- Update_Description: new article about howto firewall -->
<!--NEW.date: 06/15/2020-->