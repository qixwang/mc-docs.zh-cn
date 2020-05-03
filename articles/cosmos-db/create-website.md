---
title: 使用模板部署 Web 应用 - Azure Cosmos DB
description: 了解如何使用 Azure Resource Manager 模板部署 Azure Cosmos DB 帐户、Azure 应用服务 Web 应用以及示例 Web 应用程序。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/11/2019
ms.date: 04/27/2020
ms.author: v-yeche
ms.openlocfilehash: 78f52861d0b9a32770b577751bbd3bd8ffa78a8d
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134943"
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>使用 Azure Resource Manager 模板部署 Azure Cosmos DB 和 Azure 应用服务 Web 应用
本教程说明如何使用 Azure 资源管理器模板来部署和集成 [Azure Cosmos DB](https://www.azure.cn/home/features/cosmos-db/)、[Azure 应用服务](/app-service-web/app-service-changes-existing-services)、Web 应用以及示例 Web 应用程序。

使用 Azure Resource Manager 模板，可以轻松自动化 Azure 资源的部署和配置。  本教程演示如何部署 Web 应用程序，以及自动配置 Azure Cosmos DB 帐户的连接信息。

完成本教程后，你能够回答以下问题：  

* 如何使用 Azure Resource Manager 模板来部署和集成 Azure Cosmos DB 帐户与 Azure 应用服务中的 Web 应用？
* 如何使用 Azure Resource Manager 模板来部署和集成 Azure Cosmos DB 帐户、应用服务 Web 应用中的 Web 应用以及 Webdeploy 应用程序？

<a name="Prerequisites"></a>

## <a name="prerequisites"></a>先决条件
> [!TIP]
> 虽然本教程不会假设先前有使用 Azure 资源管理器模板或 JSON 的经验，但是，如果想修改引用的模板或部署选项，则需要有其中每个领域的知识。
> 
> 

在按照本教程中的说明操作之前，请确保已有 Azure 订阅。 Azure 是基于订阅的平台。  有关获取订阅的详细信息，请参阅[购买选项](https://www.azure.cn/pricing/overview/)、[会员套餐](https://www.azure.cn/support/legal/offer-rate-plans/)或[试用](https://www.azure.cn/pricing/1rmb-trial/)。

<a name="CreateDB"></a>
## <a name="step-1-download-the-template-files"></a>步骤 1：下载模板文件
让我们从下载本教程所需的模板文件开始。

1. 将[创建 Azure Cosmos DB 帐户、Web 应用和部署演示应用程序示例](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json)模板下载到本地文件夹（例如 C:\Azure Cosmos DBTemplates）。 此模板会部署 Azure Cosmos DB 帐户、应用服务 Web 应用和 Web 应用程序。  它还会自动配置 Web 应用程序，以连接到 Azure Cosmos DB 帐户。
2. 将[创建 Azure Cosmos DB 帐户和 Web 应用示例](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json)模板下载到本地文件夹（例如 C:\Azure Cosmos DBTemplates）。 此模板将部署 Azure Cosmos DB 帐户、应用服务 Web 应用，并修改站点的应用程序设置以便轻松地显示 Azure Cosmos DB 连接信息，但不包含 Web 应用程序。  

<a name="Build"></a>

## <a name="step-2-deploy-the-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>步骤 2：部署 Azure Cosmos DB 帐户、应用服务 Web 应用和演示应用程序示例
现在让我们来部署第一个模板。

> [!TIP]
> 该模板不会验证在以下模板中输入的 Web 应用名称和 Azure Cosmos DB 帐户名称是否：a) 有效和 b) 可用。  强烈建议在提交部署之前，先确认你打算提供的名称的可用性。
> 
> 

1. 登录到 [Azure 门户](https://portal.azure.cn)，单击“新建”并搜索“模板部署”。
    ![模板部署 UI 的屏幕截图](./media/create-website/TemplateDeployment1.png)
2. 选择模板部署项目，然后单击“创建”  ![模板部署 UI 的屏幕截图](./media/create-website/TemplateDeployment2.png)
3. 单击“编辑模板”  ，粘贴 DocDBWebsiteTodo.json 模板文件的内容，并单击“保存”  。
    
    <!--Customized on Mooncake-->
    
    将 Location 的 allowedValues 属性更改为 ["China East","China East 2","China North","China North 2"]。
    
    ![模板部署 UI 的屏幕截图](./media/create-website/TemplateDeployment3.png)

4. 单击“编辑参数”  ，为每个必需参数提供值，并单击“确定”  。  参数如下：

    1. SITENAME：指定应用服务 Web 应用名称，并用来构造用于访问 Web 应用的 URL（例如，如果指定“mydemodocdbwebapp”，则用于访问 Web 应用的 URL 为 `mydemodocdbwebapp.chinacloudsites.cn`）。
    2. HOSTINGPLANNAME：指定要创建的应用服务托管计划的名称。
    3. LOCATION：指定要在其中创建 Azure Cosmos DB 和 Web 应用资源的 Azure 位置。
    4. DATABASEACCOUNTNAME：指定要创建的 Azure Cosmos DB 帐户的名称。   

        ![模板部署 UI 的屏幕截图](./media/create-website/TemplateDeployment4.png)
        
5. 选择现有的资源组或提供名称以创建新的资源组，并选择资源组的位置。

    ![模板部署 UI 的屏幕截图](./media/create-website/TemplateDeployment5.png)
6. 依次单击“查看法律条款”  、“购买”  和“创建”  以开始部署。   选择“固定到仪表板”，让生成的部署轻松显示在 Azure 门户的主页上。
    
    ![模板部署 UI 的屏幕截图](./media/create-website/TemplateDeployment6.png)
    
7. 部署完成后，会打开“资源组”窗格。
    
    ![“资源组”窗格的屏幕截图](./media/create-website/TemplateDeployment7.png)
    
8. 若要使用应用程序，请导航到 Web 应用 URL（上述示例中的 URL 是 `http://mydemodocdbwebapp.chinacloudsites.cn`）。  会看到下列 Web 应用程序：

    ![示例待办事项应用程序](./media/create-website/image2.png)
    
9. 继续在 Web 应用中创建几个任务，并返回到 Azure 门户中的“资源组”窗格。 单击“资源”列表中的“Azure Cosmos DB 帐户”资源，并单击“数据资源管理器”  。
10. 运行默认查询“SELECT * FROM c”，并检查结果。  请注意，查询已检索在上面的步骤 7 中创建的待办事项的 JSON 表示形式。  任意尝试查询；例如，尝试运行 SELECT * FROM c WHERE c.isComplete = true，以返回所有标记为完成的待办事项。
11. 随意浏览 Azure Cosmos DB 门户体验，或修改示例待办事项应用程序。  准备好时，让我们来部署另一个模板。

<a name="Build"></a> 

## <a name="step-3-deploy-the-document-account-and-web-app-sample"></a>步骤 3：部署文档帐户和 Web 应用示例
现在让我们来部署第二个模板。  此模板可用于演示如何将帐户终结点和主密钥等 Azure Cosmos DB 连接信息插入 Web 应用，作为应用程序设置或自定义连接字符串。 例如，你或许有想要使用 Azure Cosmos DB 帐户部署的 Web 应用程序，以及在部署期间自动填充的连接信息。

> [!TIP]
> 该模板不会验证下面输入的 Web 应用名称和 Azure Cosmos DB 帐户名称是否：a) 有效以及 b) 可用。  强烈建议在提交部署之前，先确认你打算提供的名称的可用性。
> 
> 

1. 在 [Azure 门户](https://portal.azure.cn)中，单击“新建”并搜索“模板部署”。
    ![模板部署 UI 的屏幕截图](./media/create-website/TemplateDeployment1.png)
2. 选择模板部署项目，然后单击“创建”  ![模板部署 UI 的屏幕截图](./media/create-website/TemplateDeployment2.png)
3. 单击“编辑模板”  ，粘贴 DocDBWebSite.json 模板文件的内容，并单击“保存”  。
    
    <!--Customized on Mooncake-->
    
    将 Location 的 allowedValues 属性更改为 ["China East","China East 2","China North","China North 2"]。
    
    ![模板部署 UI 的屏幕截图](./media/create-website/TemplateDeployment3.png)
    
4. 单击“编辑参数”  ，为每个必需参数提供值，并单击“确定”  。  参数如下：

   1. SITENAME：指定应用服务 Web 应用名称，该名称用来构造用于访问 Web 应用的 URL（例如，如果指定“mydemodocdbwebapp”，则用于访问 Web 应用的 URL 是 mydemodocdbwebapp.chinacloudsites.cn）。
   2. HOSTINGPLANNAME：指定要创建的应用服务托管计划的名称。
   3. LOCATION：指定要在其中创建 Azure Cosmos DB 和 Web 应用资源的 Azure 位置。
   4. DATABASEACCOUNTNAME：指定要创建的 Azure Cosmos DB 帐户的名称。   

        ![模板部署 UI 的屏幕截图](./media/create-website/TemplateDeployment4.png)
        
5. 选择现有的资源组或提供名称以创建新的资源组，并选择资源组的位置。

    ![模板部署 UI 的屏幕截图](./media/create-website/TemplateDeployment5.png)
    
6. 依次单击“查看法律条款”  、“购买”  和“创建”  以开始部署。   选择“固定到仪表板”，让生成的部署轻松显示在 Azure 门户的主页上。
    
    ![模板部署 UI 的屏幕截图](./media/create-website/TemplateDeployment6.png)
    
7. 部署完成后，会打开“资源组”窗格。

    ![“资源组”窗格的屏幕截图](./media/create-website/TemplateDeployment7.png)
    
8. 单击“资源”列表中的“Web 应用”资源，然后单击“应用程序设置” 

    ![资源组的屏幕截图](./media/create-website/TemplateDeployment9.png)  
    
9. 注意出现的 Azure Cosmos DB 终结点和每个 Azure Cosmos DB 主密钥的应用程序设置。

    ![应用程序设置的屏幕截图](./media/create-website/TemplateDeployment10.png)  
    
10. 继续随意浏览 Azure 门户，或按照其中一个 Azure Cosmos DB [示例](/cosmos-db/sql-api-dotnet-samples)来创建自己的 Azure Cosmos DB 应用程序。

    <!--CORRECT ON [samples](/cosmos-db/sql-api-dotnet-samples)-->
    
<a name="NextSteps"></a>

## <a name="next-steps"></a>后续步骤
祝贺！ 已使用 Azure Resource Manager 模板部署了 Azure Cosmos DB、应用服务 Web 应用以及示例 Web 应用程序。

* 若要了解有关 Azure Cosmos DB 的详细信息，请单击[此处](https://www.azure.cn/home/features/cosmos-db/)。
* 若要了解有关 Azure 应用服务 Web 应用的详细信息，请单击[此处](https://docs.azure.cn/app-service/)。
* 若要了解有关 Azure Resource Manager 模板的详细信息，请单击[此处](https://msdn.microsoft.com/library/azure/dn790549.aspx)。

## <a name="whats-changed"></a>发生的更改
* 有关从网站更改应用服务的指南，请参阅：[Azure 应用服务及其对现有 Azure 服务的影响](/app-service-web/app-service-changes-existing-services)

<!-- Not Available on [Try App Service](https://go.microsoft.com/fwlink/?LinkId=523751)-->

<!-- Update_Description: update meta properties, wording update, update link -->
