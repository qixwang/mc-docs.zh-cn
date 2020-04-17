---
author: rockboyfor
ms.service: virtual-machines-sql
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: c0124329ffc150d93444aa5a53e4b20ce2081199
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63846445"
---
1. 创建并运行 Azure 虚拟机以后，单击 Azure 门户中的“虚拟机”图标即可查看 VM。

1. 单击对应于新建 VM 的省略号 **...**

1. 单击“连接”  。

   ![在门户中连接到 VM](./media/virtual-machines-sql-server-remote-desktop-connect/azure-virtual-machine-connect.png)

1. 打开浏览器为 VM 下载的  RDP 文件。

1. “远程桌面连接”会通知你，无法识别此远程连接的发布者。 单击“ **连接** ”以继续。

1. 在“Windows 安全性”对话框中，单击“使用其他帐户”。   可能需要单击“更多选项”才能看到此内容。  指定在创建 VM 时配置的用户名和密码。 必须在用户名之前添加反斜杠。

   ![远程桌面身份验证](./media/virtual-machines-sql-server-remote-desktop-connect/remote-desktop-connect.png)

1. 单击“确定”  进行连接。

<!--Update_Description: wording update, update link-->