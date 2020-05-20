---
title: 在 Azure Stack HCI 中删除卷
description: 如何使用 Windows 管理中心和 PowerShell 删除 Azure Stack HCI 中的卷。
author: WenJason
ms.author: v-jay
ms.topic: article
origin.date: 03/17/2020
ms.date: 05/18/2020
ms.openlocfilehash: 2545d8604f69fedd6986e019d966952d88919749
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83422436"
---
# <a name="deleting-volumes-in-azure-stack-hci"></a>在 Azure Stack HCI 中删除卷

> 适用于：Windows Server 2019

本主题说明如何使用 Windows Admin Center 在[存储空间直通](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)群集上删除卷。

## <a name="use-windows-admin-center-to-delete-a-volume"></a>使用 Windows Admin Center 删除卷

1. 在 Windows Admin Center 中连接到存储空间直通群集，然后在“工具”窗格中选择“卷”。 
2. 在“卷”页上选择“清单”选项卡，然后选择要删除的卷。
3. 在卷详细信息页的顶部，选择“删除”。
4. 在“确认”对话框中，选中复选框以确认要删除该卷，然后选择“删除”。

## <a name="delete-volumes-using-powershell"></a>使用 PowerShell 删除卷

使用 Remove-VirtualDisk cmdlet 删除存储空间直通中的卷。 此 cmdlet 用于删除 VirtualDisk 对象，并将其使用的空间返回给公开了 VirtualDisk 对象的存储池。

首先，在管理电脑上启动 PowerShell，并运行带有 CimSession 参数（即存储空间直通群集或服务器节点的名称，例如 clustername.microsoft.com）的 Get-VirtualDisk cmdlet： 

```PowerShell
Get-VirtualDisk -CimSession clustername.microsoft.com
```

这将返回 -FriendlyName 参数的可能值列表，这些值对应于群集上的卷名。

### <a name="example"></a>示例

若要删除名为“Volume1”的镜像卷，请在 PowerShell 中运行以下命令：

```PowerShell
Remove-VirtualDisk -FriendlyName "Volume1"
```

系统会要求你确认是否要执行该操作并清除该卷包含的所有数据。 选择“Y”或“N”。

   > [!WARNING]
   > 此操作不可恢复。 此示例会永久删除 VirtualDisk 卷对象。

## <a name="next-steps"></a>后续步骤

如需其他重要的存储管理任务的分步说明，另请参阅：

- [在存储空间直通中规划卷](../concepts/plan-volumes.md)
- [在存储空间直通中创建卷](create-volumes.md)
- [在存储空间直通中扩展卷](extend-volumes.md)