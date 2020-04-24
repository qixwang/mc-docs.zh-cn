---
title: Azure Service Fabric 应用程序资源模型
description: 本文概述如何使用 Azure 资源管理器管理 Azure Service Fabric 应用程序
ms.topic: conceptual
origin.date: 10/21/2019
ms.date: 02/24/2020
ms.author: v-yeche
ms.custom: sfrev
ms.openlocfilehash: c22e3342d7d3c8ae77223376efd333e5eaec87e5
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77540466"
---
<!--Verfiy sucessfully-->
# <a name="service-fabric-application-resource-model"></a>Service Fabric 应用程序资源模型

建议通过 Azure 资源管理器将 Service Fabric 应用程序部署到 Service Fabric 群集。 使用此方法可以在 JSON 中描述应用程序和服务，并将其部署到群集所在的同一资源管理器模板中。 与通过 PowerShell 或 Azure CLI 部署和管理应用程序相反，无需等待群集准备就绪。 只需执行一步操作，即可完成注册、预配和部署应用程序的整个过程。 这是在群集中管理应用程序生命周期的最佳做法。 有关详细信息，请参阅[最佳做法：基础结构即代码](/service-fabric/service-fabric-best-practices-infrastructure-as-code#azure-service-fabric-resources)。

在适当情况下，将应用程序作为资源管理器资源进行管理可改进：

* 审核线索：资源管理器审核所有操作，并记录详细的活动日志  ，有助于跟踪对这些应用程序和群集做出的任何更改。
* 基于角色的访问控制：可通过同一个资源管理器模板，管理对群集及其上部署的应用程序的访问。
* Azure 资源管理器（通过 Azure 门户）成为管理群集和关键应用程序部署的一站式平台。

## <a name="service-fabric-application-life-cycle-with-azure-resource-manager"></a>使用 Azure 资源管理器管理 Service Fabric 应用程序生命周期

本文档将介绍以下操作：

> [!div class="checklist"]
>
> * 使用 Azure 资源管理器部署应用程序资源
> * 使用 Azure 资源管理器升级应用程序资源
> * 删除应用程序资源

## <a name="deploy-application-resources-using-azure-resource-manager"></a>使用 Azure 资源管理器部署应用程序资源

若要使用 Azure 资源管理器应用程序资源模型部署应用程序及其服务，需要打包应用程序代码，上传该包，然后在 Azure 资源管理器模板中以应用程序资源的形式引用该包的位置。 有关详细信息，请查看[将应用程序打包](/service-fabric/service-fabric-package-apps#create-an-sfpkg)。

然后创建 Azure 资源管理器模板，使用应用程序详细信息更新参数文件，并将其部署到 Service Fabric 群集。 请参阅[此处](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/tree/master/ARM)的示例。

### <a name="create-a-storage-account"></a>创建存储帐户

从资源管理器模板部署应用程序需要使用一个存储帐户来暂存应用程序映像。 可以重复使用现有的存储帐户，或者创建新的存储帐户来暂存应用程序。 如果使用现有的存储帐户，则可以跳过此步骤。 

![创建存储帐户][CreateStorageAccount]

### <a name="configure-storage-account"></a>配置存储帐户

创建存储帐户后，需要创建一个可在其中暂存应用程序的 Blob 容器。 在 Azure 门户中，导航到用于存储应用程序的存储帐户。 选择“Blob”边栏选项卡，单击“添加容器”按钮。   可以通过将公共访问级别设置为“专用”来保护群集中的资源。 可以通过多种方式授予访问权限：

* [使用 Azure Active Directory 授予对 Blob 和队列的访问权限](../storage/common/storage-auth-aad-app.md)
* [在 Azure 门户中使用 RBAC 授予对 Azure Blob 和队列数据的访问权限](../storage/common/storage-auth-aad-rbac-portal.md)
* [使用共享访问签名 (SAS) 委派访问权限](https://docs.microsoft.com/rest/api/storageservices/delegate-access-with-shared-access-signature)

在此示例中，我们将继续对 Blob 使用匿名读取访问。

![创建 Blob][CreateBlob]

### <a name="stage-application-in-a-storage-account"></a>在存储帐户中暂存应用程序

在部署应用程序之前，必须先将其暂存在 Blob 存储中。 在本教程中，我们将手动创建应用程序包，不过也可以将此步骤自动化。  有关详细信息，请查看[将应用程序打包](/service-fabric/service-fabric-package-apps#create-an-sfpkg)。 下面的步骤将使用 [Voting 示例应用程序](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart)。

1. 在 Visual Studio 中右键单击 Voting 项目并选择“打包”。  
 
    ![将应用程序打包][PackageApplication]  
    
2. 打开刚刚创建的 **.\service-fabric-dotnet-quickstart\Voting\pkg\Debug** 目录，将内容压缩成名为 **Voting.zip** 的文件，并将 ApplicationManifest.xml 置于该 zip 文件的根目录。  
    
    ![Zip 应用程序][ZipApplication]  
    
3. 将该文件的扩展名从 .zip 重命名为 **.sfpkg**。
4. 在 Azure 门户上你的存储帐户的 **apps** 容器中，单击“上传”并上传 **Voting.sfpkg**。   
    
    ![上传应用包][UploadAppPkg]

该应用程序随即已暂存。 现在，我们可以开始创建 Azure 资源管理器模板来部署该应用程序。      

### <a name="create-the-azure-resource-manager-template"></a>创建 Azure 资源管理器模板

示例应用程序包含可用于部署应用程序的 [Azure 资源管理器模板](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/tree/master/ARM)。 模板文件名为 **UserApp.json** 和 **UserApp.Parameters.json**。

> [!NOTE]
> 必须使用群集名称更新 **UserApp.Parameters.json** 文件。
>
>

| 参数              | 说明                                 | 示例                                                      | 注释                                                     |
| ---------------------- | ------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| clusterName            | 要部署到的群集的名称 | sf-cluster123                                                |                                                              |
| application            | 应用程序的名称                 | Voting                                                       |
| applicationTypeName    | 应用程序的类型名称           | VotingType                                                   | 必须与 ApplicationManifest.xml 中的内容匹配                 |
| applicationTypeVersion | 应用程序类型的版本         | 1.0.0                                                        | 必须与 ApplicationManifest.xml 中的内容匹配                 |
| serviceName            | 服务的名称         | Voting~VotingWeb                                             | 必须采用 ApplicationName~ServiceType 格式            |
| serviceTypeName        | 服务的类型名称                | VotingWeb                                                    | 必须与 ServiceManifest.xml 中的内容匹配                 |
| appPackageUrl          | 应用程序的 Blob 存储 URL     | https://servicefabricapps.blob.core.chinacloudapi.cn/apps/Voting.sfpkg | Blob 存储中应用程序包的 URL（下面介绍了设置过程） |

```json
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

### <a name="deploy-the-application"></a>部署应用程序

若要部署该应用程序，请运行 New-AzResourceGroupDeployment 部署到包含你的群集的资源组。

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "sf-cluster-rg" -TemplateParameterFile ".\UserApp.Parameters.json" -TemplateFile ".\UserApp.json" -Verbose
```

## <a name="upgrade-service-fabric-application-using-azure-resource-manager"></a>使用 Azure 资源管理器升级 Service Fabric 应用程序

已部署到 Service Fabric 群集的应用程序将出于以下原因而升级：

1. 已将一个新服务添加到应用程序。 必须将服务定义添加到 service-manifest.xml 和 application-manifest.xml 文件中。 然后，若要反映应用程序的新版本，需在 [UserApp.parameters.json](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/blob/master/ARM/UserApp.Parameters.json) 中将应用程序类型版本从 1.0.0 更新为 1.0.1。

    ```json
    "applicationTypeVersion": {
        "value": "1.0.1"
    },
    "serviceName2": {
        "value": "Voting~VotingData"
    },
    "serviceTypeName2": {
        "value": "VotingDataType"
    }
    ```

2. 现有服务的新版本将添加到应用程序中。 这涉及到更改应用程序代码以及更新应用类型版本和名称。

    ```json
     "applicationTypeVersion": {
        "value": "1.0.1"
    },
    ```

## <a name="delete-application-resources"></a>删除应用程序资源

可通过以下步骤从群集中删除使用应用程序资源模型在 Azure 资源管理器中部署的应用程序

1) 使用 [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource?view=azps-2.5.0) 获取应用程序的资源 ID：

    ```powershell
    Get-AzResource  -Name <String> | f1
    ```

2) 使用 [Remove-AzResource](https://docs.microsoft.com/powershell/module/az.resources/remove-azresource?view=azps-2.5.0) 删除应用程序资源：

    ```powershell
    Remove-AzResource  -ResourceId <String> [-Force] [-ApiVersion <String>]
    ```

## <a name="next-steps"></a>后续步骤

获取有关应用程序资源模型的信息：

* [在 Service Fabric 中对应用程序建模](/service-fabric/service-fabric-application-model)
* [Service Fabric 应用程序和服务清单](/service-fabric/service-fabric-application-and-service-manifests)

## <a name="see-also"></a>另请参阅

* [最佳实践](/service-fabric/service-fabric-best-practices-infrastructure-as-code)
* [将应用程序和服务作为 Azure 资源进行管理](/service-fabric/service-fabric-best-practices-infrastructure-as-code)

<!--Image references-->

[CreateStorageAccount]: ./media/service-fabric-application-model/create-storage-account.png
[CreateBlob]: ./media/service-fabric-application-model/create-blob.png
[PackageApplication]: ./media/service-fabric-application-model/package-application.png
[ZipApplication]: ./media/service-fabric-application-model/zip-application.png
[UploadAppPkg]: ./media/service-fabric-application-model/upload-app-pkg.png

<!-- Update_Description: update meta properties, wording update, update link -->