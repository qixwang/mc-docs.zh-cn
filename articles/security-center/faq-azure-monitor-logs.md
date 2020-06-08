---
title: Azure 安全中心常见问题解答 - 有关现有 Log Analytics 代理的问题
description: 此常见问题解答为已在使用 Log Analytics 代理并考虑使用 Azure 安全中心（一种可帮助你预防、检测和响应威胁的产品）的客户解答问题。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2020
ms.author: v-tawe
origin.date: 02/25/2020
ms.openlocfilehash: 68dc253fe36e1a26dce5c49f5b29c86bc6daf013
ms.sourcegitcommit: cbaa1aef101f67bd094f6ad0b4be274bbc2d2537
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84126744"
---
# <a name="faq-for-customers-already-using-azure-monitor-logs"></a>针对已在使用 Azure Monitor 日志的客户的常见问题解答<a name="existingloganalyticscust"></a>

## <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>安全中心是否会覆盖 VM 和工作区之间的任何现有连接？

如果 VM 已将 Log Analytics 代理作为 Azure 扩展进行安装，则安全中心不会覆盖现有工作区连接。 相反，安全中心会使用现有工作区。 如果在 VM 向其报告的工作区上安装了“Security”或“SecurityCenterFree”解决方案，则该 VM 将受保护。 

将在“数据收集”屏幕中选择的工作区上安装一个安全中心解决方案（如果尚不存在），而该解决方案只会应用到相关的 VM。 添加解决方案时，默认情况下会自动将它部署到连接到 Log Analytics 工作区的所有 Windows 和 Linux 代理。 <!-- not available -->

> [!TIP]
> 如果 Log Analytics 代理直接安装到 VM 上（而不是作为 Azure 扩展进行安装），那么安全中心不会安装 Log Analytics 代理，并且安全监视也会受限。

## <a name="does-security-center-install-solutions-on-my-existing-log-analytics-workspaces-what-are-the-billing-implications"></a>安全中心是否在现有 Log Analytics 工作区上安装解决方案？ 计费会产生什么影响？
安全中心确定 VM 已连接到所创建的工作区时，会根据定价层启用此工作区上的解决方案。 <!--  not available -->
- **免费层** - 安全中心在工作区中安装“SecurityCenterFree”解决方案。 免费层不会产生费用。
- **标准层** - 安全中心在工作区中安装“Security”解决方案。

   ![默认工作区上的解决方案](./media/security-center-platform-migration-faq/solutions.png)

<!-- not available-->

## <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>工作区已存在安全解决方案。 计费会产生什么影响？
“安全性与审核”解决方案用于为 Azure VM 启用安全中心标准层功能。 如果已在工作区上安装“安全性与审核”解决方案，则安全中心会使用现有解决方案。 计费方面没有任何更改。