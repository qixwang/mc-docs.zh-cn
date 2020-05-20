---
title: 在 Azure 安全中心管理安全事件 | Azure
description: 本文档可帮助你使用 Azure 安全中心管理安全事件。
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 05/12/2020
ms.author: v-tawe
origin.date: 03/15/2020
ms.openlocfilehash: 1e10ed12c8d0bbbefc51b829aa806ddeaec91c13
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83422998"
---
# <a name="manage-security-incidents-in-azure-security-center"></a>在 Azure 安全中心管理安全事件

即使是最熟练的安全分析员，对安全警报进行分类和调查也可能很耗时，而且对于许多人来说，甚至都不知道从何处开始。 通过使用[分析](security-center-alerts-overview.md)来连接不同的[安全警报](security-center-managing-and-responding-alerts.md)之间的信息，安全中心可以提供攻击活动及其所有相关警报的单一视图，让你快速了解攻击者所采取的操作以及哪些资源受到了影响。

本主题介绍安全中心的事件，以及如何使用建议来修正其警报。

## <a name="what-is-a-security-incident"></a>什么是安全事件？

在安全中心，安全事件是对资源的所有警报汇总，与 [网络攻击链](alerts-reference.md#intentions) 模式保持一致。 事件显示在[安全警报](security-center-managing-and-responding-alerts.md)列表中。 单击事件可查看相关警报，这样就可以获得每个事件的更多信息。

## <a name="managing-security-incidents"></a>管理安全事件

1. 在安全中心仪表板中，单击“安全警报”磁贴。 此时会列出事件和警报。 请注意，安全事件描述相比其他警报具有不同的图标。

    ![查看安全事件](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. 若要查看详细信息，请单击某个事件。 “检测到的安全事件”边栏选项卡会显示更多详细信息。 可以通过“常规信息”部分深入了解触发安全警报的原因。 它显示的信息包括目标资源、源 IP 地址（如果适用）、警报是否仍然处于活动状态，以及修正方式建议。  

    ![在 Azure 安全中心响应安全事件](./media/security-center-managing-and-responding-alerts/security-center-alert-incident.png)

1. 若要获取每个警报的详细信息，请单击警报。 安全中心提供的修正建议因安全警报而异。

   > [!NOTE]
   > 同一个警报可以作为事件的一部分存在，也可以作为独立警报显示。

    ![警报详细信息](./media/security-center-incident/security-center-incident-alert.png)

1. 按照为每个警报提供的修正步骤进行操作。


## <a name="see-also"></a>另请参阅
在本文档中，已经学习了如何在安全中心使用安全事件功能。 如需相关信息，请参阅以下内容：

* [Azure 安全中心的威胁防护](threat-protection.md)
* [Azure 安全中心中的安全警报](security-center-alerts-overview.md)
* [管理安全警报](security-center-managing-and-responding-alerts.md)