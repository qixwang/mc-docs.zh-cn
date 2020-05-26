---
title: Azure 安全中心支持的平台 | Azure
description: 本文档列出了 Azure 安全中心支持的平台。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2020
ms.author: v-tawe
origin.date: 08/29/2019
ms.openlocfilehash: 835bedec901b0692d2d868cd26cddd030bba57bf
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83422904"
---
# <a name="supported-platforms"></a>支持的平台 

## <a name="virtual-machines--servers"></a>虚拟机/服务器 <a name="vm-server"></a>

安全中心支持不同类型的混合环境上的虚拟机/服务器：

* 仅限 Azure
* Azure 和本地
* Azure 和其他云
* Azure、其他云和本地

对于在 Azure 订阅上激活的 Azure 环境，Azure 安全中心会自动发现在该订阅中部署的 IaaS 资源。

> [!NOTE]
> 若要接收完整的安全功能集，则必须安装 [Log Analytics 代理](../azure-monitor/platform/agents-overview.md#log-analytics-agent)（由 Azure 安全中心使用）并对其进行[正确配置以将数据发送到 Azure 安全中心](security-center-enable-data-collection.md#manual-agent)。

以下部分列出了支持的服务器操作系统，Azure 安全中心使用的 [Log Analytics 代理](../azure-monitor/platform/agents-overview.md#log-analytics-agent)可以在其上运行。

### <a name="windows-server-operating-systems"></a>Windows Server 操作系统 <a name="os-windows"></a>

|操作系统|受 Azure 安全中心支持|支持与 Microsoft Defender ATP 进行集成|
|:---|:-:|:-:|
|Windows Server 2019|✔|X|
|Windows Server 2016|✔|✔|
|Windows Server 2012 R2|✔|✔|
|Windows Server 2008 R2|✔|✔|

若要详细了解上面列出的 Windows 操作系统的支持功能，请参阅[虚拟机/服务器支持的功能](security-center-services.md#vm-server-features)。

### <a name="windows-operating-systems"></a>Windows 操作系统 <a name="os-windows (non-server)"></a>

Azure 安全中心与 Azure 服务集成，监视和保护基于 Windows 的虚拟机。

### <a name="linux-operating-systems"></a>Linux 操作系统 <a name="os-linux"></a>

64 位

* CentOS 6 和 7
* Amazon Linux 2017.09
* Oracle Linux 6 和 Oracle Linux 7
* Red Hat Enterprise Linux Server 6 和 7
* Debian GNU/Linux 8 和 9
* Ubuntu Linux 14.04 LTS、16.04 LTS 和 18.04 LTS
* SUSE Linux Enterprise Server 12

32 位
* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Debian GNU/Linux 8 和 9
* Ubuntu Linux 14.04 LTS 和 16.04 LTS

> [!NOTE]
> 由于受支持的 Linux 操作系统的列表会不断变化，因此若你愿意，请单击[此处](https://github.com/microsoft/OMS-Agent-for-Linux#supported-linux-operating-systems)查看最新的受支持版本列表，以防本主题在上次发布后发生了更改。

若要详细了解上面列出的 Linux 操作系统的支持功能，请参阅[虚拟机/服务器支持的功能](security-center-services.md#vm-server-features)。

### <a name="managed-virtual-machine-services"></a>托管的虚拟机服务 <a name="virtual-machine"></a>

虚拟机也会作为某些 Azure 托管服务（比如 Azure Kubernetes (AKS)、Azure Databricks 等）的一部分在客户订阅中创建。 这些虚拟机还可以由 Azure 安全中心发现，并且 Log Analytics 代理可根据上面列出的受支持的 [Windows/Linux 操作系统](#os-windows)进行安装和配置。

### <a name="cloud-services"></a>云服务 <a name="cloud-services"></a>

此外，云服务中运行的虚拟机也受支持。 仅监视云服务 Web 和在生产槽运行的辅助角色。 若要了解有关云服务的详细信息，请参阅[云服务概述](../cloud-services/cloud-services-choose-me.md)。

还支持对驻留在 Azure Stack 中的虚拟机进行保护。 若要详细了解安全中心与 Azure Stack 的集成，请参阅[将 Azure Stack 虚拟机加入安全中心](https://docs.azure.cn/security-center/quick-onboard-azure-stack)。

## <a name="next-steps"></a>后续步骤

- 了解[安全中心如何收集数据以及 Log Analytics 代理](security-center-enable-data-collection.md)。
- 了解[安全中心如何管理和保护数据](security-center-data-security.md)。
- 了解如何[规划并理解设计注意事项，以便采用 Azure 安全中心](security-center-planning-and-operations-guide.md)。
- 了解[适用于不同云环境的功能](security-center-services.md)。
- 详细了解 [Azure 安全中心中适用于 Windows 和 Linux 计算机的威胁防护](threat-protection.md#windows-machines)。
