---
title: 设置用于将本地物理服务器灾难恢复到 Azure 的目标环境 | Azure
description: 本文介绍如何使用 Azure Site Recovery 设置目标 Azure 环境以进行物理服务器的灾难恢复。
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: conceptual
origin.date: 11/27/2018
ms.date: 04/22/2019
ms.author: v-yeche
ms.openlocfilehash: 07bc5a8f890d7459698e97aafdaba899042cbc17
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63825609"
---
# <a name="prepare-target-vmware-to-azure"></a>准备目标（VMware 到 Azure）

本文介绍如何准备 Azure 环境以将运行 Windows 或 Linux 的物理服务器 (x64) 复制到 Azure。

## <a name="prerequisites"></a>必备条件

本文假设：
- 已创建恢复服务保管库，用于保护物理服务器。 可通过 [Azure 门户](https://portal.azure.cn "Azure 门户")创建恢复服务保管库。
- 已[设置本地环境](physical-azure-disaster-recovery.md)，用于将物理服务器复制到 Azure。

## <a name="prepare-target"></a>准备目标

完成“**步骤 1: 选择保护目标**”和“**步骤 2: 准备源**”后，会转到“**步骤 3: 目标**”

![准备目标](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. 订阅：  从下拉菜单中，选择要将物理服务器复制到的“订阅”。
2. **部署模型：** 选择部署模型（经典或 Resource Manager）

根据选择的部署模型运行验证，确保要将物理服务器复制并故障转移到的目标订阅中至少具有一个兼容的存储帐户和虚拟网络。

成功完成验证后，单击“确定”转到下一步。

如果你没有兼容的资源管理器存储帐户或虚拟网络，则可单击页面顶部的“+ 存储帐户”  或“+ 网络”  按钮来创建一个。

## <a name="next-steps"></a>后续步骤
[配置复制设置](vmware-azure-set-up-replication.md)。

<!--Update_Description: update meta properties -->
