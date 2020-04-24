---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 44634a7221dd26cab228a0b297b32655e625fcfc
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63822446"
---
1. 在故障转移群集管理器中，展开“角色”，然后突出显示可用性组。   

2. 在“资源”选项卡上，右键单击侦听程序名称，然后单击“属性”。  

3. 选择“依赖项”选项卡。  如果列出了多个资源，请验证 IP 地址具有 OR 而不是 AND 依赖项。  

4. 单击 **“确定”** 。

5. 右键单击侦听程序名称，然后单击“联机”。 

6. 侦听程序处于联机状态后，请在“资源”选项卡上右键单击可用性组，然后单击“属性”。  

    ![配置可用性组资源](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)

7. 在侦听程序名称资源（而不是 IP 地址资源名称）上创建依赖项，然后单击“确定”。 

    ![在侦听程序名称上添加依赖项](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)

8. 启动 SQL Server Management Studio，然后连接到主副本。

9. 转到“AlwaysOn 高可用性”   > “可用性组”   >  **\<AvailabilityGroupName\>**  >   “可用性组侦听程序”。  
    此时会显示在故障转移群集管理器中创建的侦听程序名称。

10. 右键单击侦听程序名称，然后单击“属性”。 

11. 在“端口”框中，通过使用先前使用过的 $EndpointPort 为可用性组侦听程序指定端口号（在本教程中，默认值是 1433），然后单击“确定”。  

<!-- Update_Description: update meta properties -->