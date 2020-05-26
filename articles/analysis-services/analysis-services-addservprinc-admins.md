---
title: 将服务主体添加到 Azure Analysis Services 管理员角色 | Azure
description: 了解如何将自动化服务主体添加到 Azure Analysis Services 服务器管理员角色
author: rockboyfor
ms.service: azure-analysis-services
ms.topic: conceptual
origin.date: 03/30/2020
ms.date: 04/30/2020
ms.author: v-yeche
ms.reviewer: minewiskan
ms.custom: fasttrack-edit
ms.openlocfilehash: d078861548404c972230a11c935334b0cb04d2ad
ms.sourcegitcommit: 2d8950c6c255361eb6c66406988e25c69cf4e0f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83392362"
---
# <a name="add-a-service-principal-to-the-server-administrator-role"></a>将服务主体添加到服务器管理员角色 

 若要自动执行无人参与的 PowerShell 任务，服务主体在托管的 Analysis Services 服务器上必须具备“服务器管理员”权限。 本文介绍如何将服务主体添加到 Azure AS 服务器上的服务器管理员角色。 可以使用 SQL Server Management Studio 或资源管理器模板来执行此操作。

## <a name="before-you-begin"></a>准备阶段
在完成此项任务之前，必须有一个在 Azure Active Directory 中注册的服务主体。

[创建服务主体 - Azure 门户](../active-directory/develop/howto-create-service-principal-portal.md)   
[创建服务主体 - PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio

可以使用 SQL Server Management Studio (SSMS) 配置服务器管理员。 若要完成此项任务，在 Azure AS 服务器上必须具备[服务器管理员](analysis-services-server-admins.md)权限。 

1. 在 SSMS 中连接至 Azure AS 服务器。
2. 在“服务器属性” > “安全性”中，单击“添加”  。
3. 在“选择用户或组”中，按名称搜索注册的应用，选中以后单击“添加” 。

    ![搜索服务主体帐户](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. 验证服务主体帐户 ID，然后单击“确定”。

    ![搜索服务主体帐户](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)

## <a name="using-a-resource-manager-template"></a>使用资源管理器模板

还可以通过使用 Azure 资源管理器模板部署 Analysis Services 服务器来配置服务器管理员。 运行部署的身份必须在 [Azure 基于角色的访问控制 (RBAC)](../role-based-access-control/overview.md) 中属于资源的“参与者”角色。

> [!IMPORTANT]
> 必须使用格式 `app:{service-principal-client-id}@{azure-ad-tenant-id}` 来添加服务主体。

以下资源管理器模板使用已添加到 Analysis Services 管理员角色中的指定服务主体部署 Analysis Services 服务器：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "analysisServicesServerName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "analysisServicesSkuName": {
            "type": "string"
        },
        "analysisServicesCapacity": {
            "type": "int"
        },
        "servicePrincipalClientId": {
            "type": "string"
        },
        "servicePrincipalTenantId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('analysisServicesServerName')]",
            "type": "Microsoft.AnalysisServices/servers",
            "apiVersion": "2017-08-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('analysisServicesSkuName')]",
                "capacity": "[parameters('analysisServicesCapacity')]"
            },
            "properties": {
                "asAdministrators": {
                    "members": [
                        "[concat('app:', parameters('servicePrincipalClientId'), '@', parameters('servicePrincipalTenantId'))]"
                    ]
                }
            }
        }
    ]
}
```

## <a name="using-managed-identities"></a>使用托管标识

还可以将托管标识添加到 Analysis Services 管理员列表。 例如，你可能有一个[具有系统分配的托管标识逻辑应用](../logic-apps/create-managed-service-identity.md)，并且希望授予它管理 Analysis Services 服务器的能力。

<!--EXISTS ON system-assigned managed identity IN LOGIC APPS-->

在 Azure 门户和 API 的大多数部分中，托管标识是使用其服务主体对象 ID 标识的。 但是，Analysis Services 要求使用其客户端 ID 进行标识。 若要获取服务主体的客户端 ID，可以使用 Azure CLI：

```bash
az ad sp show --id <ManagedIdentityServicePrincipalObjectId> --query appId -o tsv
```

也可以使用 PowerShell：

```powershell
(Get-AzureADServicePrincipal -ObjectId <ManagedIdentityServicePrincipalObjectId>).AppId
```

然后，可以将此客户端 ID 与租户 ID 结合使用，以将托管标识添加到 Analysis Services 管理员列表中，如上所述。

## <a name="related-information"></a>相关信息

* [下载 SQL Server PowerShell 模块](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [下载 SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

<!-- Update_Description: update meta properties, wording update, update link -->