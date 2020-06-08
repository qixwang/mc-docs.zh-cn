---
title: Azure 安全中心中的安全分数
description: " 使用 Azure 安全中心中的安全评分确定安全建议的优先级。 "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 1/15/2019
ms.date: 09/22/2019
ms.author: v-lingwu
ms.openlocfilehash: fbf289a77761d9b6d9441aa54d0da2e553eb8cc3
ms.sourcegitcommit: cbaa1aef101f67bd094f6ad0b4be274bbc2d2537
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84126559"
---
# <a name="improve-your-secure-score-in-azure-security-center"></a>提高 Azure 安全中心的安全评分

> [!NOTE]
> 本文介绍以前版本的安全评分。 此安全评分体验仍然可以从 UI 获得，但将来会停用。 两种安全评分体验会并行存在，以实现更平稳的过渡。
>
> 有关新版安全评分的详细信息，请参阅[此文](secure-score-security-controls.md)。
>

在有很多服务提供安全权益的情况下，通常很难知道首先要采取哪些措施来保护并强化工作负荷。 安全评分会评审你的安全建议并确定其优先级，以便你知道要首先执行哪些建议。 这可帮助你找到最严重的安全漏洞，因此你可以确定调查优先级。 安全评分是一个可帮助你评估工作负荷安全状况的工具。

## <a name="secure-score-calculation"></a>计算安全评分

安全中心模仿安全分析师的工作，审查安全建议并应用高级算法来确定每项建议的重要性。
Azure 安全中心会不断检查活动建议并根据它们计算安全评分，建议的分数源自其严重性和对工作负荷安全性影响最大的安全最佳做法。

安全中心还提供**整体安全评分**。 

“总体安全评分”是所有建议分数的累计值。 可以查看订阅或管理组的总体安全评分，具体取决于所选择的内容。 分数将根据所选订阅和这些订阅上的活动建议而有所不同。

若要检查哪些建议对安全评分影响最大，可以在安全中心仪表板中查看最具影响力的前三个建议，或者可以使用“安全评分影响”列对“建议列表”边栏选项卡中的建议进行排序。

若要查看总体安全评分，请执行以下操作：

1. 在 Azure 仪表板中，单击“安全中心”，然后单击“安全评分” 。

2. 可以在顶部看见安全评分突出显示：
   - “总体安全评分”表示每个策略、每个所选订阅的分数
   - “安全评分(按类别)”显示哪些资源需要最多的关注
   - “安全评分影响的顶级建议”提供了一个建议列表，这些建议可以在你实施它们时最大程度提高安全评分。
 
   ![安全评分](./media/security-center-secure-score/secure-score-dashboard.png)

3. 在下表中可以看到每个订阅及其总体安全评分。

   > [!NOTE]
   > 每个订阅的安全评分总和不等于总体安全评分。 安全评分是根据每个建议的正常运行资源数与总资源数之间的比率计算的，而不是你的所有订阅的安全评分之和。 
   >
4. 单击“查看建议”可查看该订阅的建议，你可以修正该订阅以提高安全评分。
4. 在建议列表中，可在其中看到每个建议都有表示“安全评分影响”的一列。 此数字表示遵循建议能提高的总体安全评分。 例如，在下面的屏幕中，如果修正容器安全配置中的漏洞，安全评分将增加 35 分。

   ![安全评分](./media/security-center-secure-score/security-center-secure-score1.png)



## <a name="individual-secure-score"></a>单项安全评分

此外，若要查看单项安全评分，可以在单个建议边栏选项卡中找到这些分数。  

“建议安全评分”是根据正常运行的资源数与总资源数之间的比率计算的。 如果正常运行的资源数等于资源总数，则会获得建议的最高安全评分：50 分。 为了让安全评分接近最高分，请按建议修复运行不正常的资源。

“建议影响”表示在应用建议步骤时安全评分会提升多少。 例如，如果安全评分为 42 且“建议影响”为 +3，则执行建议中列出的步骤可以将安全评分提高到 45。

该建议显示不执行修正步骤时工作负荷所面临的威胁。

![单项建议安全评分](./media/security-center-secure-score/indiv-recommendation-secure-score.png)


## <a name="next-steps"></a>后续步骤
本文介绍了如何使用 Azure 安全中心的“安全评分”来改进安全状况。 若要了解有关安全中心的详细信息，请参阅：

* [Azure 安全中心常见问题解答](security-center-faq.md)-- 查找有关使用服务的常见问题。

