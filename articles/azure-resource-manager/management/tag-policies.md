---
title: 用于标记资源的策略
description: 介绍可分配的用于确保标记符合性的 Azure 策略。
ms.topic: conceptual
origin.date: 03/20/2020
ms.date: 05/08/2020
ms.author: v-yeche
ms.openlocfilehash: 2decdc9ae64fe98c9d6889df1e3e3718adf53bab
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84199791"
---
<!--Verified successfully-->
# <a name="assign-policies-for-tag-compliance"></a>分配用于确保标记符合性的策略

使用 [Azure Policy](../../governance/policy/overview.md) 强制实施标记规则和约定。 通过创建策略，可以避免将没有组织所需标记的资源部署到订阅。 不要手动应用标记或搜索不符合的资源，请创建一个策略，用于在部署期间自动应用所需标记。 现在，还可以通过新的 [Modify](../../governance/policy/concepts/effects.md#modify) 效果和[修正任务](../../governance/policy/how-to/remediate-resources.md)将标记应用于现有资源。 以下部分展示标记策略示例。

## <a name="policies"></a>策略

[!INCLUDE [Tag policies](../../../includes/policy/samples/bycat/policies-tags.md)]

## <a name="next-steps"></a>后续步骤

* 若要了解如何标记资源，请参阅[使用标记来组织 Azure 资源](tag-resources.md)。
* 并非所有资源类型都支持标记。 若要确定是否可以将标记应用到资源类型，请参阅 [Azure 资源的标记支持](tag-support.md)。

<!-- Update_Description: new article about tag policies -->
<!--NEW.date: 04/30/2020-->