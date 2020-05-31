---
title: 将动态组与 Azure 自动化更新管理配合使用
description: 本文介绍动态组如何与 Azure 自动化更新管理配合工作。
services: automation
ms.subservice: update-management
origin.date: 11/20/2019
ms.date: 05/25/2020
ms.topic: conceptual
ms.openlocfilehash: baa4acf3d12925d5af15fdd7f59f76df1c438dc1
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801885"
---
# <a name="use-dynamic-groups-with-update-management"></a>将动态组与更新管理配合使用

更新管理允许将 Azure 或非 Azure VM 的动态组指定为更新部署的目标。 如果使用动态组，无需编辑部署即可更新计算机。

> [!NOTE]
> 动态组不适用于经典 VM。

可以从 Azure 门户中的“更新管理”为 Azure 或非 Azure 计算机定义动态组。 请参阅[管理多个 Azure 虚拟机的更新](manage-update-multi.md)。

动态组由 Azure 自动化在部署时评估的查询进行定义。 即使动态组查询检索大量的计算机，Azure 自动化一次也最多只能处理 1000 台计算机。 请参阅 [Azure 订阅和服务限制、配额和约束](/azure-resource-manager/management/azure-subscription-service-limits#update-management)。 

> [!NOTE]
> 如果你需要更新 1000 台以上的计算机，我们建议将更新划分到多个更新计划。 

## <a name="define-dynamic-groups-for-azure-machines"></a>为 Azure 计算机定义动态组

为 Azure 计算机定义动态组查询时，可以使用以下项来填充动态组：

* 订阅
* 资源组
* 位置
* Tags

![选择组](./media/automation-update-management/select-groups.png)

若要预览动态组查询的结果，请单击“预览”。 预览中会显示当前时间的组成员身份。 在此示例中，我们将搜索 **BackendServer** 组中具有 `Role` 标记的计算机。 如果为多台计算机添加了此标记，会将这些计算机添加到将来针对该组的任何部署中。

![预览组](./media/automation-update-management/preview-groups.png)

## <a name="define-dynamic-groups-for-non-azure-machines"></a>为非 Azure 计算机定义动态组

非 Azure 计算机的动态组使用保存的搜索（也称为计算机组）。 若要了解如何创建保存的搜索，请参阅[创建计算机组](../azure-monitor/platform/computer-groups.md#creating-a-computer-group)。 创建保存的搜索后，可以从 Azure 门户上“更新管理”中的已保存搜索列表中选择它。 单击“预览”可预览已保存的搜索中的计算机。

![选择组](./media/automation-update-management/select-groups-2.png)

## <a name="next-steps"></a>后续步骤

创建动态组后，可以[创建更新部署](automation-tutorial-update-management.md)。
