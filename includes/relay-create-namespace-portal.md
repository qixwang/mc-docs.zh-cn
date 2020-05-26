---
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 05/19/2020
ms.author: v-tawe
origin.date: 11/09/2018
ms.openlocfilehash: 8c4f4e27f63f327f4905d3141479cbd7e9c168bc
ms.sourcegitcommit: cada23b6400453ff9c08cfb08393e635e2fddac1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2020
ms.locfileid: "83734512"
---
1. 登录到 [Azure 门户][Azure portal]。
1. 选择“创建资源”。 然后，选择“集成” > “中继”。 如果未在列表中看到“中继”，请选择右上角的“查看全部”。
1. 选择“创建”，然后在“名称”字段中输入命名空间名称。 Azure 门户会检查该名称是否可用。
1. 选择要在其中创建命名空间的 Azure 订阅。
1. 对于[资源组](/azure-resource-manager/management/manage-resource-groups-portal.md)，选择一个要在其中放置命名空间的现有资源组，或创建一个新资源组。  
1. 选择应托管该命名空间的国家或地区。
   
    ![创建命名空间][create-namespace]
    
1. 选择“创建” 。 系统会创建并启用命名空间。 几分钟后，系统为用户的帐户预配资源。

### <a name="get-management-credentials"></a>获取管理凭据

1. 选择“所有资源”，然后选择新建的命名空间名称。
1. 选择“共享访问策略”。  
1. 在“共享访问策略”下，选择“RootManageSharedAccessKey”。
1. 在“SAS 策略:RootManageSharedAccessKey”下，选择“主连接字符串”旁边的“复制”按钮。 此操作会将连接字符串复制到剪贴板，供以后使用。 将此值粘贴到记事本或其他某个临时位置。
1. 重复上述步骤，将**主密钥**的值复制和粘贴到临时位置，供以后使用。  

    ![连接字符串][connection-string]

 

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace-vs2019.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string-vs2019.png
[Azure portal]: https://portal.azure.cn
