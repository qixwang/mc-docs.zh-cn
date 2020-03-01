---
title: 对 Azure Stack Hub 应用原始设备制造商 (OEM) 更新
description: 了解如何对 Azure Stack Hub 应用原始设备制造商 (OEM) 更新。
author: WenJason
ms.topic: article
origin.date: 1/22/2020
ms.date: 02/24/2020
ms.author: v-jay
ms.lastreviewed: 09/10/2019
ms.reviewer: ppacent
ms.openlocfilehash: 3add0b325a3e65f774d26b61fe157b0a1bda969d
ms.sourcegitcommit: afe972418a883551e36ede8deae32ba6528fb8dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77540262"
---
# <a name="azure-stack-hub-servicing-policy"></a>Azure Stack Hub 服务策略

本文介绍了 Azure Stack Hub 集成系统的服务策略、使系统保持受支持状态所必须执行的操作以及如何获得支持。

## <a name="keep-your-system-under-support"></a>保持系统受支持

若要继续获得支持，必须使 Azure Stack Hub 上的更新保持最新。

要使 Azure Stack Hub 系统保持受支持状态，实例必须运行最新发布的更新版本或运行前面两个更新版本之一。

修补程序不属于主要更新版本。 如果 Azure Stack Hub 系统落后两个以上的更新  ，则将其视为不合规。 必须至少更新到最低支持版本才能获得支持。

例如，如果最新发布的更新版本为 1904，在此之前的两个更新包为版本 1903 和 1902，则 1902 和 1903 仍受支持， 但 1901 不受支持。 即使最近一到两个月没有发布任何版本，此策略也有效。 例如，如果最新版本为 1807，但没有版本 1806，则此前的两个更新包（1805 和 1804）仍受支持。

Microsoft 软件更新包是非累积性的，其先决条件是需要前一个更新包或修补程序。 如果决定延后一个或多个更新，如果想要获取最新版本，请考虑整体运行时。

## <a name="get-support"></a>获取支持

Azure Stack Hub 遵循与 Azure 相同的支持流程。 企业客户可以[创建 Azure 支持请求](https://support.azure.cn/zh-cn/support/support-azure/)。 如果你是云解决方案提供商 (CSP) 的客户，请联系 CSP 获得支持。 有关详细信息，请参阅 [Azure 支持常见问题解答](https://azure.cn/support/faq/)。

## <a name="next-steps"></a>后续步骤

[在 Azure Stack Hub 中管理更新](azure-stack-updates.md)
