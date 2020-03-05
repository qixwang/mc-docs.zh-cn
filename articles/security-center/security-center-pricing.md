---
title: Azure 安全中心层级的定价
description: Azure 安全中心具有两种层级 - 免费层和标准层。 此页面显示如何从“免费层”升级到“标准层”。
services: security-center
documentationcenter: na
author: lingliw
manager: rkarlin
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 12/13/2019
ms.date: 1/9/2020
ms.author: v-lingwu
ms.openlocfilehash: a9df5a2e8c58a61c241658f148e1b712dfc38168
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155005"
---
# <a name="upgrade-to-standard-tier-for-enhanced-security"></a>升级到标准层以增强安全性
Azure 安全中心为在 Azure、本地和其他云中运行的工作负载提供统一的安全管理。 它可以提供针对混合云工作负载的可见性和可控性、可减小在威胁下的曝光面的积极防御功能以及有助于随时响应快速演变的网络攻击的智能检测功能。

## <a name="pricing-tiers"></a>定价层
安全中心分两个层提供：

- 你第一次在 Azure 门户中访问 Azure 安全中心仪表板后，或者通过 API 以编程方式启用后，“免费”层就会在所有 Azure 订阅上启用。  免费层提供安全策略、持续的安全评估和切实可行的安全建议来帮助你保护 Azure 资源。
- **标准**层将免费层的功能扩展到私有云和其他公有云中运行的工作负荷，从而为各种混合云工作负荷提供统一的安全管理。 标准层还增加了高级威胁检测功能，它使用内置行为分析和机器学习识别攻击和零时差攻击，并使用访问和应用程序控件减小在网络攻击和恶意软件下的曝光面，此外还有更多其他操作。 可以免费试用标准层。 安全中心标准支持 Azure 资源，包括 VM、虚拟机规模集、应用服务、SQL Server 和存储帐户。 如果你使用 Azure 安全中心标准，则可以根据资源类型选择不再支持。 

大多数针对 VM 的免费层安全评估以及许多标准层安全警报都需要安装 Microsoft Monitoring Agent (MMA) 功能。 在安全中心启用自动预配即可为 Azure VM 自动部署代理。

## <a name="try-standard-free-for-30-days"></a>免费试用标准层 30 天
标准层在前 30 天免费。 30 天后，如果选择继续使用服务，会自动开始收取使用费用。

可将整个 Azure 订阅升级到标准层，这样此订阅中的所有资源均将继承此层。

若要获取标准层，请执行以下操作：

1. 在“安全中心”主菜单上选择“定价和设置”  。 
2. 选择要升级到标准的订阅。
3. 选择“定价层”。 
4. 选择“标准层”  以进行升级。
5. 单击“保存”  。

[![安全中心定价](media/security-center-pricing/pricing-tier-page.png)](media/security-center-pricing/pricing-tier-page.png#lightbox)

> [!NOTE]
> 若要启用所有的安全中心功能，必须将标准定价层应用到包含适用虚拟机的订阅。 配置工作区定价不会为 Azure 资源启用实时 VM 访问、自适应应用程序控件和网络检测功能。
>

## <a name="why-upgrade-to-standard"></a>为什么要升级到标准？
安全中心为混合云工作负荷提供了增强的安全性，其中包括：

-  混合安全 – 在所有本地和云工作负载上获得统一的安全视图。 应用安全策略并持续评估混合云工作负载的安全性，确保符合安全标准。 收集、搜索并分析来自多个源（包括防火墙和其他合作伙伴解决方案）的安全数据。
-  高级威胁检测 - 使用高级分析和 Microsoft Intelligent Security Graph，获得针对不断演变的网络攻击的优势。 利用内置行为分析和机器学习来识别攻击和零时差攻击。 监视网络、计算机和云服务是否出现有即将来袭的攻击和攻破后活动。 使用交互工具和上下文威胁智能简化调查。
-  虚拟机的漏洞扫描 - 轻松地向所有虚拟机部署一个一个扫描程序，提供行业内的出色漏洞管理解决方案。 直接在安全中心内查看、调查和修复扫描到的漏洞。 
- **访问和应用程序控件** - 通过应用适合特定工作负载且由机器学习提供支持的允许列表建议，阻止恶意软件和其他不需要的应用程序。 实时减小网络受攻击面，控制对 Azure VM 上的管理端口的访问。 这会显著减小在暴力和其他网络攻击下的曝光面。
- **容器安全功能** - 在容器化环境中进行漏洞管理和实时威胁检测的好处。 启用容器注册表资源时，启用所有功能可能需要长达 12 小时的时间。


<!--Image references-->
[1]: ./media/security-center-pricing/get-standard.png
