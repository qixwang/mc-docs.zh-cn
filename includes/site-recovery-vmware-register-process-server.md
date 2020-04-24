---
author: rockboyfor
ms.service: site-recovery
ms.topic: include
origin.date: 04/28/2019
ms.date: 06/10/2019
ms.author: v-yeche
ms.openlocfilehash: f85a47b4f68a8af55d83b85ebe376443d817d83d
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "66390707"
---
1. 与运行进程服务器的计算机建立远程桌面连接。 
2. 运行 cspsconfigtool.exe 以启动 Azure Site Recovery 进程服务器配置工具。
    - 首次登录进程服务器时，该工具会自动启动。
    - 如果它未自动打开，请单击它在桌面上的快捷方式。

3. 在“配置服务器 FQDN 或 IP”  中，指定用于注册进程服务器的配置服务器的名称或 IP 地址。
4. 在“配置服务器端口”  中，确保指定了 443。 这是配置服务器侦听请求的端口。
5. 在“连接密码”  中，指定你在设置配置服务器时指定的密码。 查找密码的步骤：
    -  在配置服务器上，导航到 Site Recovery 安装文件夹 *\home\svssystems\bin *\** 。 
    - 运行以下命令：**genpassphrase.exe.n**。 这将显示密码的位置，然后你可以将其记下。

6. 在“数据传输端口”  中，除非指定了自定义端口，否则请保留默认值。

7. 单击“保存”  保存设置，并注册进程服务器。

    ![注册进程服务器](./media/site-recovery-vmware-register-process-server/register-ps.png)