---
author: rockboyfor
ms.service: virtual-machines-sql
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 6334efdf33a33ec7bdde0e4c24ed4e370757d363
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63823137"
---
### <a name="create-a-tcp-endpoint-for-the-virtual-machine"></a>为虚拟机创建 TCP 终结点
要从 Internet 访问 SQL Server，虚拟机必须具有终结点以侦听传入的 TCP 通信。 此 Azure 配置步骤将传入 TCP 端口通信定向到虚拟机可以访问的 TCP 端口。

> [!NOTE]
> 如果在同一云服务或虚拟网络中连接，则不需要创建一个公开访问的终结点。 在这种情况下，可以继续执行下一步。 有关详细信息，请参阅[连接方案](../articles/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-connect.md#connection-scenarios)。
> 
> 

1. 在 Azure 门户中，选择“虚拟机(经典)”。 
2. 然后选择 SQL Server 虚拟机。
3. 选择“终结点”，并单击终结点边栏选项卡顶部的“添加”按钮。  

    ![用于创建终结点的门户步骤](./media/virtual-machines-sql-server-connection-steps/portal-endpoint-creation.png)
4. 在“添加终结点”边栏选项卡上，提供“名称”，例如 SQLEndpoint。  
5. 选择“TCP”作为“协议”   。
6. 为“公用端口”指定端口号，如“57500”   。
7. 为“专用端口”，指定 SQL Server 侦听端口，默认为“1433”   。
8. 单击“确定”  创建终结点。

<!--Update_Description: wording update, update link-->