---
title: Azure Service Fabric - 部署支持托管标识的新 Azure Service Fabric 群集
description: 本文介绍如何创建启用了托管标识的新 Service Fabric 群集
ms.topic: article
origin.date: 12/09/2019
ms.author: v-yeche
ms.date: 01/06/2020
ms.openlocfilehash: 25db1ad576e1e7f0386573abfcaad7e536eebde9
ms.sourcegitcommit: 671ff43b99572ca295968769278ee726954f1016
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2020
ms.locfileid: "76508762"
---
<!--Verify successfully with New Deployment-->

# <a name="create-a-new-azure-service-fabric-cluster-with-managed-identity-support-preview"></a>创建支持托管标识的新 Azure Service Fabric 群集（预览）

若要访问 Azure Service Fabric 应用程序的托管标识功能，必须先在群集上启用托管标识令牌服务。 此服务负责使用 Service Fabric 应用程序的托管标识对这些应用程序进行身份验证，以及代表它们获取访问令牌。 启用此服务以后，即可在 Service Fabric Explorer 中左侧窗格的“系统”部分  下看到它，它在其他系统服务旁边以 **fabric:/System/ManagedIdentityTokenService** 名称运行。

> [!NOTE]
> 若要启用**托管标识令牌服务**，必须使用 Service Fabric 运行时 6.5.658.9590 或更高版本。  

## <a name="enable-the-managed-identity-token-service"></a>启用托管标识令牌服务 
若要在创建群集时启用托管标识令牌服务，可以在 Azure 资源管理器模板中使用以下代码片段：

```json
"fabricSettings": [
    {
        "name": "ManagedIdentityTokenService",
        "parameters": [
            {
                "name": "IsEnabled",
                "value": "true"
            }
        ]
    }
]
```

## <a name="errors"></a>错误

如果部署失败并显示此消息，则表示群集不在所需的 Service Fabric 版本上（支持的最低运行时为 6.5 CU2）：

```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```

## <a name="related-articles"></a>相关文章
* 查看 Azure Service Fabric 中的[托管标识支持](./concepts-managed-identity.md)

* [在现有 Azure Service Fabric 群集中启用托管标识支持](./configure-existing-cluster-enable-managed-identity-token-service.md)

## <a name="next-steps"></a>后续步骤

<!--Not Avaialble on * [Deploy an Azure Service Fabric application with a system-assigned managed identity](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)-->
<!--Not Avaialble on * [Deploy an Azure Service Fabric application with a user-assigned managed identity](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)-->
<!--Not Avaialble on * [Leverage the managed identity of a Service Fabric application from service code](./how-to-managed-identity-service-fabric-app-code.md)-->

* [向 Azure Service Fabric 应用程序授予对其他 Azure 资源的访问权限](./how-to-grant-access-other-resources.md)

<!-- Update_Description: update meta properties, wording update, update link -->