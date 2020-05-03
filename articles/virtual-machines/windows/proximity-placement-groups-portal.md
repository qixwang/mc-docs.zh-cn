---
title: 使用门户创建邻近放置组
description: 了解如何使用 Azure 门户创建邻近放置组。
services: virtual-machines
author: rockboyfor
manager: digimobile
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
origin.date: 10/30/2019
ms.date: 04/30/2020
ms.author: v-yeche
ms.openlocfilehash: db7e851ddfdaa2deff409b30c63a435bbf12f88e
ms.sourcegitcommit: 275203d0f144aa809792f2c570fd64797f405c80
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82125796"
---
<!--Verified successfully-->
# <a name="create-a-proximity-placement-group-using-the-portal"></a>使用门户创建邻近放置组

若要让 VM 尽可能靠近，将延迟尽可能降至最低，应将 VM 部署到一个[邻近放置组](co-location.md#proximity-placement-groups)中。

邻近放置组是一种逻辑分组，用于确保 Azure 计算资源在物理上彼此靠近。 邻近放置组用于要求低延迟的工作负荷。

## <a name="create-the-proximity-placement-group"></a>创建邻近放置组

1. 在搜索中键入“邻近放置组”  。
1. 在搜索结果中的“服务”  下，选择“邻近放置组”  。
1. 在“邻近放置组”  页面中，选择“添加”  。
1. 在“基本信息”  选项卡中的“项目详细信息”  下，确保选择了正确的订阅。
1. 在“资源组”  中选择“新建”  来创建新的资源组，或者从下拉列表中选择现有资源组。
1. 在“区域”  中，选择要在其中创建邻近放置组的位置。
1. 在“邻近放置组名称”  中键入一个名称，然后选择“查看 + 创建”  。
1. 在通过验证后，选择“创建”  以创建邻近放置组。

    ![创建邻近放置组的屏幕截图](./media/ppg/ppg.png)

## <a name="create-a-vm"></a>创建 VM

1. 在门户中创建 VM 时，转到“高级”  选项卡。 
1. 在“邻近放置组”  部分，选择正确的邻近放置组。 

    ![在门户中新建 VM 时“邻近放置组”部分的屏幕截图](./media/ppg/vm-ppg.png)

1. 在完成所有其他必需选择后，选择“查看 + 创建”  。
1. 通过验证后，选择“创建”  ，在邻近放置组中部署 VM。

## <a name="next-steps"></a>后续步骤

也可使用 [Azure PowerShell](proximity-placement-groups.md) 创建邻近放置组。

<!-- Update_Description: new article about proximity placement groups portal -->
<!--NEW.date: 04/30/2020-->