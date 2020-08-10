---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 08/10/2020
ms.testscope: no
ms.testdate: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 7f6b6fe7645577689d871aaa87205374b284bf95
ms.sourcegitcommit: ac70b12de243a9949bf86b81b2576e595e55b2a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919335"
---
在此步骤中，使用同一网络上运行的客户端应用程序测试可用性组侦听器。

客户端连接具有以下要求：

* 对于与侦听器的客户端连接，其源计算机所驻留的云服务必须不同于托管 AlwaysOn 可用性副本的云服务。
* 如果 Always On 副本位于不同子网中，客户端必须在连接字符串中指定 *MultisubnetFailover=True*。 此条件会导致尝试并行连接到各个子网中的副本。 此方案包括跨区域 AlwaysOn 可用性组部署。

例如，从同一个 Azure 虚拟网络的一个虚拟机（而不是托管副本的虚拟机）连接到侦听器。 完成此测试一个简单的方法是尝试将 SQL Server Management Studio 连接到可用性组侦听器。 另一种简单方法是运行 [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx)，如下所示：

```console
sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15
```

> [!NOTE]
> 如果 EndpointPort 值为 *1433*，则不需在调用时指定它。 前面的调用还假定客户端计算机加入了同一个域，并且调用方已被授予使用 Windows 身份验证访问数据库的权限。
> 
> 

在测试侦听器时，请务必对可用性组进行故障转移，以确保客户端可以在故障转移之间连接到侦听器。

<!-- Update_Description: update meta properties -->