---
title: 复制并粘贴到虚拟机以及从中进行复制和粘贴 - Azure Bastion
description: 本文介绍如何使用 Bastion 复制并粘贴到 Azure VM 以及从中进行复制和粘贴。
services: bastion
author: rockboyfor
ms.service: bastion
ms.topic: how-to
origin.date: 05/04/2020
ms.date: 07/27/2020
ms.testscope: yes
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: 09a18b10004a00d1633d30859e738d43f159d51b
ms.sourcegitcommit: 4d9846bb03ac24bd98b0c9a781bb8912ff6d2f61
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86926978"
---
<!--RELEASE BEFORE CONFIRMATION-->
# <a name="copy-and-paste-to-a-virtual-machine-azure-bastion"></a>复制并粘贴到虚拟机：Azure Bastion

本文可帮助你在使用 Azure Bastion 时将内容复制并粘贴到虚拟机，以及从中进行复制和粘贴。 使用 VM 之前，请确保已按照[创建 Bastion 主机](bastion-create-host-portal.md)的步骤进行操作。 然后，连接到要通过 [RDP](bastion-connect-vm-rdp.md) 或 [SSH](bastion-connect-vm-ssh.md) 使用的 VM。

对于支持高级剪贴板 API 访问的浏览器，可以使用在本地设备上的应用程序之间进行复制和粘贴的相同方式在本地设备与远程会话之间复制和粘贴文本。 对于其他浏览器，可以使用 Bastion 剪贴板访问工具面板。

>[!NOTE]
>目前仅支持文本复制/粘贴。
>

  ![允许剪贴板](./media/bastion-vm-manage/allow.png)

仅支持文本复制/粘贴。 对于直接复制和粘贴，浏览器可能会在 Bastion 会话进行初始化时提示你它需要访问剪贴板。 允许网页访问剪贴板。

<a name="to"></a>
## <a name="copy-to-a-remote-session"></a>复制到远程会话

使用 [Azure 门户](https://portal.azure.cn)连接到虚拟机后，请完成以下步骤：

1. 将本地设备中的文本/内容复制到本地剪贴板。
1. 在远程会话期间，通过选择两个箭头启动 Bastion 剪贴板访问工具面板。 这些箭头位于会话的左中位置。

    ![工具面板](./media/bastion-vm-manage/left.png)

    ![剪贴板](./media/bastion-vm-manage/clipboard.png)
1. 通常，已复制的文本会自动显示在 Bastion 复制粘贴面板上。 如果文本不在那里，则将文本粘贴到面板上的文本区域中。
1. 文本位于文本区域中后，可以将其粘贴到远程会话。

    ![粘贴](./media/bastion-vm-manage/local.png)

<a name="from"></a>
## <a name="copy-from-a-remote-session"></a>从远程会话复制

使用 [Azure 门户](https://portal.azure.cn)连接到虚拟机后，请完成以下步骤：

1. 将远程会话中的文本/内容复制到远程剪贴板（使用 Ctrl-C）。

    ![工具面板](./media/bastion-vm-manage/remote.png)
1. 在远程会话期间，通过选择两个箭头启动 Bastion 剪贴板访问工具面板。 这些箭头位于会话的左中位置。

    ![剪贴板](./media/bastion-vm-manage/clipboard2.png)
1. 通常，已复制的文本会自动显示在 Bastion 复制粘贴面板上。 如果文本不在那里，则将文本粘贴到面板上的文本区域中。
1. 文本位于文本区域中后，可以将其粘贴到本地设备。

    ![粘贴](./media/bastion-vm-manage/local2.png)

## <a name="next-steps"></a>后续步骤

阅读 [Bastion 常见问题解答](bastion-faq.md)。

<!-- Update_Description: new article about bastion vm copy paste -->
<!--NEW.date: 07/27/2020-->