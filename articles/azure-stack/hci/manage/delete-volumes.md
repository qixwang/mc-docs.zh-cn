---
title: 在 Azure Stack HCI 中删除卷
description: 如何使用 Windows Admin Center 在 Azure Stack HCI 中删除卷。
author: WenJason
ms.author: v-jay
ms.topic: article
origin.date: 03/05/2020
ms.date: 03/23/2020
ms.openlocfilehash: cb748f7922cd73a0d2c0253722a98d66c2e0be79
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79547133"
---
# <a name="deleting-volumes-in-azure-stack-hci"></a>在 Azure Stack HCI 中删除卷

> 适用于：Windows Server 2019

本主题说明如何使用 Windows Admin Center 在[存储空间直通](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)群集上删除卷。

## <a name="use-windows-admin-center-to-delete-a-volume"></a>使用 Windows Admin Center 删除卷

1. 在 Windows Admin Center 中连接到存储空间直通群集，然后在“工具”窗格中选择“卷”。  
2. 在“卷”  页上选择“清单”  选项卡，然后选择要删除的卷。
3. 在卷详细信息页的顶部，选择“删除”。 
4. 在“确认”对话框中，选中复选框以确认要删除该卷，然后选择“删除”  。

## <a name="next-steps"></a>后续步骤

如需其他重要的存储管理任务的分步说明，另请参阅：

- [在存储空间直通中规划卷](https://docs.microsoft.com/windows-server/storage/storage-spaces/plan-volumes)
- [在存储空间直通中创建卷](https://docs.microsoft.com/windows-server/storage/storage-spaces/create-volumes)
- [在存储空间直通中扩展卷](https://docs.microsoft.com/windows-server/storage/storage-spaces/resize-volumes)