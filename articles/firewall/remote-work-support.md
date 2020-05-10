---
title: Azure 防火墙远程工作支持
description: 本文介绍 Azure 防火墙如何支持远程工作人员需求。
services: firewall
author: rockboyfor
ms.service: firewall
ms.topic: conceptual
origin.date: 03/25/2020
ms.date: 05/06/2020
ms.author: v-yeche
ms.openlocfilehash: c6f62dd9fc7cd4624240d606768820cf1f685b19
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83002064"
---
# <a name="azure-firewall-remote-work-support"></a>Azure 防火墙远程工作支持

Azure 防火墙是一种基于云的托管网络安全服务，可保护 Azure 虚拟网络资源。 它是一个服务形式的完全有状态防火墙，具有内置的高可用性和不受限制的云可伸缩性。 

## <a name="firewall-rules"></a>防火墙规则

可以使用 Azure 防火墙，通过 Azure 防火墙 [DNAT 规则](rule-processing.md)来保护虚拟桌面基础结构 (VDI) 对 Azure 虚拟网络的入站 RDP 访问。 Windows 虚拟桌面 (WVD) 不需要你打开对虚拟网络的任何入站访问。 但是，你必须允许在虚拟网络中运行的 WVD 虚拟机的一组出站网络连接。

<!--Not Avaiable on [What is Windows Virtual Desktop?](../virtual-desktop/overview.md#requirements)-->

可以使用 Azure 防火墙应用程序规则配置此出站访问。 有关详细信息，请参阅[教程：使用 Azure 门户部署和配置 Azure 防火墙](tutorial-firewall-deploy-portal.md)。

<!--Not Available on ## Next steps-->

<!--Not Available on [Windows Virtual Desktop](/virtual-desktop/)-->

<!-- Update_Description: new article about remote work support -->
<!--NEW.date: 04/06/2020-->