---
title: 在 Azure Stack 中排查更新问题 | Microsoft Docs
description: 作为 Azure Stack 操作员，了解如何解决更新问题，以便 Azure Stack 可以尽快恢复生产状态。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 09/23/2019
ms.date: 01/13/2020
ms.author: v-jay
ms.lastreviewed: 09/23/2019
ms.reviewer: ppacent
ms.openlocfilehash: 9bb3029fa6b5d63ecdf72d8b9fb05d27fc33f0b9
ms.sourcegitcommit: 166549d64bbe28b28819d6046c93ee041f1d3bd7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75737734"
---
# <a name="best-practices-for-troubleshooting-azure-stack-hub-patch-and-update-issues"></a>排查 Azure Stack Hub 修补程序和更新问题的最佳做法

本文概述了用于排查 Azure Stack 修补程序和更新问题以及修复常见修补程序和更新问题的最佳做法。


Azure Stack Hub 修补程序和更新过程旨在让操作员以一致且简单的方式应用更新包。 虽然不常见，但在修补和更新期间可能会出现问题。 如果在修补和更新过程中遇到问题，建议执行以下步骤：

0. **先决条件**：请确保已遵循[更新活动清单](release-notes-checklist.md)，并[配置“日志自动收集”](azure-stack-configure-automatic-diagnostic-log-collection.md)。
1. 按照在更新失败时创建的失败警报中的补救步骤进行操作。
2. 查看[常见的 Azure Stack Hub 修补程序和更新问题](/azure-stack/operator/azure-stack-updates-troubleshoot#Common-Azure-Stack-Hub-patch-and-update-issues)，并在问题已列出的情况下采取建议的措施。
3. 如果无法解决上述步骤的问题，请创建 [Azure Stack 支持票证](azure-stack-help-and-support-overview.md)。 请确保已针对发生问题的时间跨度[收集日志](/azure-stack/operator/azure-stack-configure-on-demand-diagnostic-log-collection)。

## <a name="common-azure-stack-hub-patch-and-update-issues"></a>常见 Azure Stack Hub 修补程序和更新问题

*适用于：Azure Stack Hub 集成系统*

### <a name="preparationfailed"></a>PreparationFailed

**适用于**：此问题适用于所有支持的版本。

**原因：** 尝试安装 Azure Stack 更新时，更新状态可能会失败并将状态更改为 `PreparationFailed`。 对于连接到 Internet 的系统，这通常表明由于 Internet 连接不稳定，无法正确下载更新包。 

**补救措施**：可以通过再次单击“立即安装”  来解决此问题。 如果此问题仍然存在，建议按照[安装更新](azure-stack-apply-updates.md?#install-updates-and-monitor-progress)部分的说明手动上传更新包。

**发生率**：通用

## <a name="next-steps"></a>后续步骤

- [更新 Azure Stack](azure-stack-updates.md)  
- [Microsoft Azure Stack 帮助和支持](azure-stack-help-and-support-overview.md)
