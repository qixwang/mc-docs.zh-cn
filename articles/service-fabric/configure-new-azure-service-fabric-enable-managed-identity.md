---
title: 为新的 Service Fabric 群集配置托管标识支持
description: 下面介绍如何在新的 Azure Service Fabric 群集中启用托管标识支持
ms.topic: article
origin.date: 12/09/2019
ms.date: 06/08/2020
ms.author: v-yeche
ms.custom: sfrev
ms.openlocfilehash: c748fea09365c86baa2e384a335d9ff1b061c51d
ms.sourcegitcommit: 0e178672632f710019eae60cea6a45ac54bb53a1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84356172"
---
<!--Verify successfully with New Deployment-->

# <a name="configure-managed-identity-support-for-a-new-service-fabric-cluster"></a>为新的 Service Fabric 群集配置托管标识支持

若要在 Service Fabric 应用程序中使用 [Azure 资源托管标识](../active-directory/managed-identities-azure-resources/overview.md)，请首先在群集上启用“托管标识令牌服务”。 此服务负责使用 Service Fabric 应用程序的托管标识对这些应用程序进行身份验证，以及代表它们获取访问令牌。 启用此服务以后，即可在 Service Fabric Explorer 中左侧窗格的“系统”部分下看到它，它在其他系统服务旁边以 **fabric:/System/ManagedIdentityTokenService** 名称运行。

> [!NOTE]
> 若要启用**托管标识令牌服务**，必须使用 Service Fabric 运行时 6.5.658.9590 或更高版本。  

## <a name="enable-the-managed-identity-token-service"></a>启用托管标识令牌服务

若要在创建群集时启用托管标识令牌服务，请在群集的 Azure资源管理器模板中添加以下代码片段：

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