---
title: Microsoft Azure Stack Hub 帮助和支持概述
description: 获取 Microsoft Azure Stack Hub 的支持。
author: WenJason
ms.topic: article
origin.date: 07/24/2019
ms.date: 02/24/2020
ms.author: v-jay
ms.reviewer: prchint
ms.lastreviewed: 07/24/2019
ms.openlocfilehash: f0ec9b4c5eb9418d95e1dcb8d2f536bd66c4621e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77540993"
---
# <a name="azure-stack-hub-help-and-support"></a>Azure Stack Hub 帮助和支持

Azure Stack Hub 门户中的“帮助 + 支持”  提供的资源可帮助操作员详细了解 Azure Stack Hub、查看其支持选项，以及获得专家帮助。 从 1907 版本开始，操作员还可以使用“帮助 + 支持”收集诊断日志以进行故障排除。  

## <a name="help-resources"></a>帮助资源 

操作员还可以通过“帮助 + 支持”  详细了解 Azure Stack Hub、查看其支持选项，以及获得专家帮助。 

### <a name="things-to-try-first"></a>首先要尝试的操作

“帮助 + 支持”的顶部提供了可先试用项目的链接，例如研读新概念、了解计费的工作原理，或查看有哪些可用的支持选项。  

![自助支持](media/azure-stack-help-and-support/get-support-tiles.png)

- **文档** [Azure Stack Hub 操作员文档](index.yml)包含多项介绍如何提供 Azure Stack Hub 服务（例如虚拟机、SQL 数据库、Web 应用等）的概念、操作指南主题和教程。 

- **了解计费**。 获取有关[用量和计费](azure-stack-billing-and-chargeback.md)的提示。

- **支持选项**。 Azure Stack Hub 操作员有多种可供选择的 [Azure 支持选项](/azure-stack/operator/azure-stack-manage-basics#where-to-get-support)，用于应对各种企业的需求。 

### <a name="get-expert-help"></a>获取专家帮助 

对于集成系统，Azure 和我们的原始设备制造商 (OEM) 硬件合作伙伴之间已经建立了协作的问题升级和解决流程。

如果存在云服务问题，请通过 Azure 客户支持服务 (CSS) 寻求支持。 可以单击管理员门户右上角的“帮助”（问号），然后单击“帮助 + 支持”打开“帮助 + 支持概述”，并提交新的支持请求。    创建支持请求时，系统会预先选择 Azure Stack Hub 服务。 我们强烈建议客户使用此体验来提交票证，而不要使用公共 Azure 门户。 

如果存在部署问题、修补和更新问题、硬件（包括现场可更换部件）问题，以及任何硬件品牌软件（例如在硬件生命周期主机上运行的软件）问题，请首先联系 OEM 硬件供应商。 对于其他问题，请联系 Azure CSS。

![获取集成系统的专家帮助](media/azure-stack-help-and-support/get-support-integrated.png)

对于 ASDK，可以在 [Azure Stack Hub MSDN 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home?sort=relevancedesc&brandIgnore=True&searchTerm=Azure+Stack)中提出与支持相关的问题。 

可以单击管理员门户右上角的“帮助”  （问号），然后单击“帮助 + 支持”  打开“帮助 + 支持概述”  ，其中包含指向论坛的链接。 我们会定期监视 MSDN 论坛。  
由于开发工具包是一个评估环境，因此我们不会通过 Azure CSS 提供官方支持。

![获取 ASDK 的专家帮助](media/azure-stack-help-and-support/get-support-asdk.png)

还可以转到 MSDN 论坛来探讨问题，或参加在线培训以提升自己的技能。 

![获取专家帮助](media/azure-stack-help-and-support/get-support-cards.png)

### <a name="get-up-to-speed-with-azure-stack-hub"></a>快速熟悉 Azure Stack Hub

这套教程是根据运行的是 ASDK 或集成系统而自定义的，可帮助你快速熟悉环境。 

![获取支持教程](media/azure-stack-help-and-support/get-support-tutorials.png)

## <a name="diagnostic-log-collection"></a>诊断日志收集

从 1907 版开始，在“帮助和支持”  中提供了两种新方法来收集日志：

- **自动收集**：如果启用，日志收集将由特定的运行状况警报触发 
- **立即收集日志**：可以从过去七天选择 1-4 小时滑动窗口

![诊断日志收集选项的屏幕截图](media/azure-stack-automatic-log-collection/azure-stack-log-collection-overview.png)

集成系统可以与 Azure 客户支持服务 (CSS) 共享诊断日志。 由于 Azure Stack 开发工具包 (ASDK) 是一个评估环境，因此它不受 CSS 支持。 有关详细信息，请参阅 [Azure Stack 诊断日志收集概述](azure-stack-diagnostic-log-collection-overview.md)。



## <a name="help-and-support-for-earlier-releases-azure-stack-hub-pre-1905"></a>旧版 Azure Stack Hub（1905 之前）的帮助和支持

旧版 Azure Stack Hub 也有“帮助 + 支持”链接，此链接重定向到 [Azure Stack Hub 操作员文档](/azure-stack/operator/)。 

![获取支持教程](media/azure-stack-help-and-support/get-support-previous.png)

如果存在云服务问题，请通过 Azure 客户支持服务 (CSS) 寻求支持。 可以单击管理员门户右上角的“帮助”（问号），单击“帮助和支持”，然后单击“新建支持请求”以直接向 CSS 提交新的支持请求。   

对于集成系统，Azure 和我们的 OEM 合作伙伴之间已经建立了协作的问题升级和解决流程。 如果存在云服务问题，请通过 Azure CSS 寻求支持。 

如果存在部署问题、修补和更新问题、硬件（包括现场可更换部件）问题，以及任何硬件品牌软件（例如在硬件生命周期主机上运行的软件）问题，请首先联系 OEM 硬件供应商。 对于其他问题，请联系 Azure CSS。

对于开发工具包，可以在 [Azure Stack Hub MSDN 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home?sort=relevancedesc&brandIgnore=True&searchTerm=Azure+Stack)中提出与支持相关的问题。 可以单击管理员门户右上角的“帮助”（问号），然后单击“新建支持请求”，从 Azure Stack Hub 社区的专家处获取帮助。  
由于开发工具包是一个评估环境，因此我们不会通过 Azure CSS 提供官方支持。

## <a name="next-steps"></a>后续步骤

- 了解如何[排查 Azure Stack Hub 的问题](azure-stack-troubleshooting.md)
