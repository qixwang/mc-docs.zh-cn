---
title: 如何在 Azure 安全中心合规性仪表板中更新为动态合规性监视 | Azure
description: 更新合规性包（预览版）
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2020
ms.author: v-tawe
origin.date: 11/04/2019
ms.openlocfilehash: 21d13a8051e8683a040cdbbc69128e29a8121ed4
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83423068"
---
# <a name="update-to-dynamic-compliance-packages-in-your-regulatory-compliance-dashboard-preview"></a>在合规性仪表板中更新为动态合规性包（预览版）

Azure 安全中心会不断地将资源的配置与行业标准、法规和基准中的要求进行比较。 根据你满足特定的合规性控制和要求的方式，合规性仪表板提供有关合规性状态的见解。

用于跟踪合规性状态的一个标准是 [Azure CIS 1.1.0](https://www.cisecurity.org/benchmark/azure/)（更正式地说法是“CIS Microsoft Azure 基础基准检验版本 1.1.0”）。 

最初出现在合规性仪表板中的 Azure CIS 表示形式依赖于安全中心随附的一组静态规则。

通过“动态合规性包（预览版）”功能，安全中心会随时间的推移自动提高行业标准的覆盖范围。 合规性包实质上是在 Azure Policy 中定义的计划。 可以将它们分配给所选的作用域（订阅、管理组等）。 若要查看在仪表板中映射为评估的合规性数据，请从安全策略中将合规性包添加到管理组或订阅中。 添加合规性包可有效地将合规性计划分配给所选的作用域。 通过这种方式，你可以在仪表板中跟踪新发布的法规计划，将其作为合规性标准。 当 Microsoft 发布新的计划内容（映射到标准中的更多控制的新策略）时，其他内容会自动显示在仪表板中。

Azure CIS 基准 - Azure CIS 1.1.0（新版）- 的动态合规性包通过以下方式在原始静态版本的基础上做了改进：

* 包括更多策略
* 添加新覆盖范围时自动更新 

更新为如下所述的新动态包。

## <a name="adding-a-dynamic-compliance-package"></a>添加动态合规性包

以下步骤说明如何添加动态包，监视你对 Azure CIS 基准 1.1.0 版的遵从情况。   

### <a name="update-to-the-azure-cis-110-new-dynamic-compliance-package"></a>更新为 Azure CIS 1.1.0（新版）动态合规性包 

1. 打开“安全策略”页。 此页面显示管理组数、订阅数和工作区数以及管理组结构。

1. 选择要为其管理合规性状态的订阅或管理组。 建议选择标准适用的最高作用域，以便为所有嵌套资源聚合和跟踪合规性数据。 

1. 在行业与法规标准（预览版）部分中，你将看到 Azure CIS 1.1.0 可以针对新内容进行更新。 单击“立即更新”。 

1. （可选）单击“添加更多标准”以打开“添加合规性标准”页 。 在该页中，可手动搜索 Azure CIS 1.1.0（新版）和动态包以获取其他合规性标准，如 NIST SP 800-53 R4、SWIFT CSP CSCF-v2020、UKO 和 UK NHS 以及加拿大 PBMM    。
    
    > [!TIP]
    > 只有作为所有者或策略参与者的用户具有添加合规性标准所必需的权限。 

    ![将法规包添加到 Azure 安全中心的合规性仪表板](./media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-additional-standards.png)


1. 从安全中心的边栏中，选择“合规性”以打开合规性仪表板。 
    * Azure CIS 1.1.0（新版）现在显示在行业与法规标准的列表中。 
    * Azure CIS 1.1.0 合规性的原始静态视图也将保留在其中。 将来可能会自动删除它。

    > [!NOTE]
    > 新添加的标准可能需要几个小时才能显示在合规性仪表板中。


    [![显示旧版和新版 Azure CIS 的合规性仪表板](media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-cis-old-and-new.png)](media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-cis-old-and-new.png#lightbox)


## <a name="next-steps"></a>后续步骤

在本文中，我们已了解到：

* 如何将你的合规性仪表板中显示的标准升级到新的动态包
* 如何添加合规性包以监视你对其他标准的遵从情况。 

其他相关材料，请参阅以下文章： 

- [安全中心合规性仪表板](security-center-compliance-dashboard.md)
- [使用安全策略](tutorial-security-policy.md)
- [管理 Azure 安全中心安全建议](security-center-recommendations.md) - 了解如何使用 Azure 安全中心的建议来保护 Azure 资源。