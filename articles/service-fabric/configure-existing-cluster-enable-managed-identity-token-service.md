---
title: Azure Service Fabric - 将现有的 Azure Service Fabric 群集配置为启用托管标识支持
description: 本文介绍如何将现有的 Azure Service Fabric 群集配置为启用托管标识支持
ms.topic: article
origin.date: 12/09/2019
ms.author: v-yeche
ms.date: 01/06/2020
ms.openlocfilehash: b62ca9e623b00e4f9033e1768fea80bc349475a5
ms.sourcegitcommit: 671ff43b99572ca295968769278ee726954f1016
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2020
ms.locfileid: "76508761"
---
<!--Pending for Verify-->
<!--Verify successfully with New Deployment-->

# <a name="configure-an-existing-azure-service-fabric-cluster-to-enable-managed-identity-support-preview"></a>将现有的 Azure Service Fabric 群集配置为启用托管标识支持（预览版）
若要访问 Azure Service Fabric 应用程序的托管标识功能，必须先在群集上启用**托管标识令牌服务**。 此服务负责使用 Service Fabric 应用程序的托管标识对这些应用程序进行身份验证，以及代表它们获取访问令牌。 启用此服务以后，即可在 Service Fabric Explorer 中左侧窗格的“系统”部分  看到它，它以 **fabric:/System/ManagedIdentityTokenService** 名称运行。

> [!NOTE]
> 若要启用**托管标识令牌服务**，必须使用 Service Fabric 运行时 6.5.658.9590 或更高版本。  
> 
> 可以在 Azure 门户中查找 Service Fabric 版群集，方法是：打开群集资源，然后在“基本信息”部分查找“Service Fabric 版本”属性。  
> 
> 如果群集处于“手动”升级模式，则需先将其升级到  6.5.658.9590 或更高版本。

## <a name="enable-the-managed-identity-token-service-in-an-existing-cluster"></a>在现有群集中启用托管标识令牌服务
若要在现有群集中启用托管标识令牌服务，需启动群集升级并指定两项更改：启用托管标识令牌服务，以及请求重启每个节点。 为此，请在 Azure 资源管理器模板中添加下述两个代码片段：

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

若要让更改生效，还需更改升级策略，指定在升级进展到群集时，在每个节点上以强制方式重启 Service Fabric 运行时。 此重启确保新启用的系统服务在每个节点上启动并运行。 在下面的代码片段中，`forceRestart` 是基本设置；请对余下设置使用现有值。  

```json
"upgradeDescription": {
    "forceRestart": true,
    "healthCheckRetryTimeout": "00:45:00",
    "healthCheckStableDuration": "00:05:00",
    "healthCheckWaitDuration": "00:05:00",
    "upgradeDomainTimeout": "02:00:00",
    "upgradeReplicaSetCheckTimeout": "1.00:00:00",
    "upgradeTimeout": "12:00:00"
}
```

> [!NOTE]
> 成功完成升级以后，请勿忘记回退 `forceRestart` 设置，尽量减少后续升级的影响。 

## <a name="errors-and-troubleshooting"></a>错误和故障排除

如果部署失败并出现以下错误，则表明群集运行时所在的 Service Fabric 的版本不够高：

```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```

## <a name="next-steps"></a>后续步骤

<!--Not Avaiable on * [Deploy an Azure Service Fabric application with a system-assigned managed identity](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)-->
<!--Not Avaiable on * [Deploy an Azure Service Fabric application with a user-assigned managed identity](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)-->
<!--Not Avaiable on * [Leverage the managed identity of a Service Fabric application from service code](./how-to-managed-identity-service-fabric-app-code.md)-->

* [向 Azure Service Fabric 应用程序授予对其他 Azure 资源的访问权限](./how-to-grant-access-other-resources.md)

<!--Update_Description: new articles on configure existing cluster enalbed managed identity token service-->
<!--new.date: 09/02/2019-->