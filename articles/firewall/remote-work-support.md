---
title: Azure 防火墙远程工作支持
description: 本文介绍 Azure 防火墙如何支持远程工作人员需求。
services: firewall
author: rockboyfor
ms.service: firewall
ms.topic: conceptual
origin.date: 05/04/2020
ms.date: 06/15/2020
ms.author: v-yeche
ms.openlocfilehash: a81902d5ce7741fe0e477078d6a21fc7200c701a
ms.sourcegitcommit: 285649db9b21169f3136729c041e4d04d323229a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/11/2020
ms.locfileid: "84683901"
---
<!--Verified successfully-->
# <a name="azure-firewall-remote-work-support"></a>Azure 防火墙远程工作支持

Azure 防火墙是一种基于云的托管网络安全服务，可保护 Azure 虚拟网络资源。 它是一个服务形式的完全有状态防火墙，具有内置的高可用性和不受限制的云可伸缩性。

## <a name="virtual-desktop-infrastructure-vdi-deployment-support"></a>虚拟桌面基础结构 (VDI) 部署支持

在家工作策略要求许多 IT 组织应对容量、网络、安全性和治理方面的根本性变化。 在家工作时，员工不受与本地服务相关的分层安全策略的保护。 Azure 上的虚拟桌面基础结构 (VDI) 部署可以帮助组织快速应对这种变化的环境。 但是，你需要通过某种方法来保护进出这些 VDI 部署的入站/出站 Internet 访问。 可以使用 Azure 防火墙 [DNAT 规则](rule-processing.md)及其基于[威胁情报](threat-intel.md)的筛选功能来保护 VDI 部署。

<!--Not Available on ## Azure Windows Virtual Desktop support-->

<!--Not Available on [Use Azure Firewall to protect Window Virtual Desktop deployments](protect-windows-virtual-desktop.md)-->

<!--Not Available on ## Next steps-->

<!--Not Available on [Windows Virtual Desktop](/virtual-desktop/)-->

<!-- Update_Description: new article about remote work support -->
<!--NEW.date: 04/06/2020-->