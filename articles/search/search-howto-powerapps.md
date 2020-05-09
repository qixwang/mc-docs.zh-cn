---
title: 如何从 Power Apps 查询 Azure 认知搜索
titleSuffix: Azure Cognitive Search
description: 有关创建用于认知搜索的自定义连接器以及从 Power App 可视化查询结果的分步指南
author: luiscabrer
manager: eladz
ms.author: v-tawe
ms.service: cognitive-search
ms.devlang: rest-api
ms.topic: conceptual
origin.date: 03/25/2020
ms.date: 04/20/2020
ms.openlocfilehash: ee283413a536b22ff6cd0cd01334c42bdea714ae
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82588922"
---
# <a name="how-to-query-a-cognitive-search-index-from-power-apps"></a>如何从 Power Apps 查询认知搜索索引

本文档介绍了如何创建 Power Apps 自定义连接器，以便从搜索索引中检索搜索结果。 此外，它还介绍了如何发出搜索查询，以及如何从 Power App 将结果可视化。 

## <a name="prerequisites"></a>先决条件
*    能够创建自定义连接器的 Power Apps 帐户。
*    假设你已创建了一个 Azure 搜索索引。

## <a name="create-a-custom-connector-to-query-azure-search"></a>创建自定义连接器来查询 Azure 搜索

需要执行两个主要步骤才能创建可以显示 Azure 认知搜索结果的 PowerApp。 首先，让我们创建一个可以查询搜索索引的连接器。 在[下一部分](#visualize-results-from-the-custom-connector)中，我们将更新 Power Apps 应用程序，以将连接器返回的结果可视化。

1. 转到 [make.powerapps.com](http://make.powerapps.com) 并登录  。

1. 搜索“数据” > “自定义连接器”  
 
    :::image type="content" source="./media/search-howto-powerapps/1-2-custom-connector.png" alt-text="“自定义连接器”菜单" border="true":::

1. 单击“+ 新建自定义连接器”，然后选择“从空白开始创建”。  

    :::image type="content" source="./media/search-howto-powerapps/1-3-create-blank.png" alt-text="“从空白开始创建”菜单" border="true":::

1. 为自定义连接器命名 （例如 AzureSearchQuery），然后单击“继续”   。 此时会显示一个引导你创建新连接器的向导。

1. 在“常规”页中输入信息。

    - 图标背景色（例如 #007ee5）
    - 说明（例如“用于 Azure 认知搜索的连接器”）
    - 在“主机”中，需要输入搜索服务 URL（例如 `<yourservicename>.search.chinacloudapi.cn`）
    - 对于“基 URL”，只需输入“/”
    
    :::image type="content" source="./media/search-howto-powerapps/1-5-general-info.png" alt-text="“常规信息”对话框" border="true":::

1. 在“安全”页中，将“API 密钥”设置为“身份验证类型”，并将参数标签和参数名称字段都设置为“api-key”。    对于“参数位置”，请选择“标头”，如下所示。  
 
    :::image type="content" source="./media/search-howto-powerapps/1-6-authentication-type.png" alt-text="“身份验证类型”选项" border="true":::

1. 在“定义”页中，选择“+ 新建操作”以创建用于查询索引的操作。  输入值“查询”作为操作 ID 的摘要和名称。 输入说明，例如“查询搜索索引”。 
 
    :::image type="content" source="./media/search-howto-powerapps/1-7-new-action.png" alt-text="“新建操作”选项" border="true":::


1. 按“+ 从示例导入”按钮来定义参数和标头  。 接下来，你将定义查询请求。  

    * 选择谓词 `GET`
    * 对于“URL”，请输入你的搜索索引的示例查询，例如：
       
    >https://yoursearchservicename.search.chinacloudapi.cn/indexes/yourindexname/docs?search= *&api-version=2019-05-06-Preview
    

    Power Apps 将使用语法从查询中提取参数。  请注意，我们显式定义了搜索字段。 

    :::image type="content" source="./media/search-howto-powerapps/1-8-1-import-from-sample.png" alt-text="从示例导入" border="false":::

1.  单击“导入”以自动预填充“请求”对话框  。

    :::image type="content" source="./media/search-howto-powerapps/1-8-2-import-from-sample.png" alt-text="“从示例导入”对话框" border="false":::


1. 单击每个参数旁边的“...”  符号来完成参数元数据的设置。

    - 对于 search：  将 `*` 设置为默认值，将 required 设置为 false，将 visibility 设置为 none。      

    :::image type="content" source="./media/search-howto-powerapps/1-10-1-parameter-metadata-search.png" alt-text="搜索参数元数据" border="true":::

    - 对于 api-version：  将 `2019-05-06-Preview` 设置为默认值，将 visibility 设置为 internal，将 required 设置为 True。      

    :::image type="content" source="./media/search-howto-powerapps/1-10-2-parameter-metadata-version.png" alt-text="版本参数元数据" border="true":::

    - 同样，对于 api-key，请将其设置为 required，并将 visibility 设置为 internal。     输入搜索服务 API 密钥作为默认值  。
    
    进行这些更改后，切换到“Swagger 编辑器”视图。  在 parameters 节中，应会看到以下配置：    

    ```
          parameters:
          - {name: search, in: query, required: false, type: string, default: '*'}
          - {name: api-version, in: query, required: true, type: string, default: 2019-05-06-Preview,
            x-ms-visibility: internal}
          - {name: api-key, in: header, required: true, type: string, default: YOURKEYGOESHERE,
            x-ms-visibility: internal}
    ```

1. 在 Response 节中，单击“添加默认响应”  。 此操作至关重要，因为它会帮助 Power Apps 了解响应的架构  。 粘贴示例响应。

    > [!TIP] 
    > 可以输入的 JSON 响应存在字符限制，因此建议在粘贴 JSON 之前先将其简化。 响应的架构/格式非常重要。 示例响应中的实际值不太重要，可将其简化以减少字符数。
    

1.    单击屏幕右上角的“创建连接器”按钮，以便对其进行测试  。

1.  在“测试”页中单击“+ 新建连接”，然后输入你的搜索服务查询密钥作为 api-key 的值   。

    此步骤可能会使你离开向导并转到“连接”页。 可以返回到“自定义连接编辑器”来实际测试连接。 转到“自定义连接器”> 选择新建的连接器 >“...”   > “查看属性” > “编辑” > “4.    测试”以返回到测试页。

1.    现在，单击“测试操作”以确保能够从索引中获取结果  。 如果测试成功，应会看到 200 状态，并且在响应正文中，应会看到描述搜索结果的 JSON。




## <a name="visualize-results-from-the-custom-connector"></a>可视化自定义连接器返回的结果
本教程的目的并非介绍如何使用 Power App 创建精致的用户体验，因此 UI 布局极其简单。 让我们创建一个带有搜索框、搜索按钮并在库控件中显示结果的 PowerApp。  PowerApp 将连接到我们最近创建的自定义连接器，以从 Azure 搜索中获取数据。

1. 创建新的 Power App。 转到“应用”部分，单击“+ 新建应用”，然后选择“画布”    。

    :::image type="content" source="./media/search-howto-powerapps/2-1-create-canvas.png" alt-text="创建画布应用" border="true":::

1. 选择所需的应用程序类型。 对于本教程，请创建采用“手机布局”的空白应用。   此时会显示“Power Apps Studio”。 

1. 进入 Studio 后，选择“数据源”选项卡，然后单击刚刚创建的新连接器。  在本例中，该连接器名为 AzureSearchQuery。  单击“添加连接”。 

    :::image type="content" source="./media/search-howto-powerapps/2-3-connect-connector.png" alt-text="连接连接器" border="true":::

    现在，AzureSearchQuery 是可从应用程序使用的数据源。 
    
1. 导航到“插入”选项卡，以便可以在窗体中添加几个控件  。

    :::image type="content" source="./media/search-howto-powerapps/2-4-add-controls.png" alt-text="插入控件" border="true":::

1.  插入以下元素：
    -   值为“查询:”的一个文本标签
    -   一个文本输入元素（将其命名为 txtQuery，默认值："*"） 
    -   带有文本“搜索”的一个按钮 
    -   垂直库（将其命名为 galleryResults） 
    
    你的窗体应当如下所示：

    :::image type="content" source="./media/search-howto-powerapps/2-5-controls-layout.png" alt-text="控件布局" border="true":::

1. 若要使“搜索”按钮发出查询，请选择该按钮，然后粘贴要对 OnSelect 执行的以下操作   ：

    ```
    If(!IsBlank(txtQuery.Text),
        ClearCollect(azResult, AzureSearchQuery.Get({search: txtQuery.Text}).value))
    ```

    :::image type="content" source="./media/search-howto-powerapps/2-6-search-button-event.png" alt-text="OnSelect 按钮" border="true":::
 
    此操作会导致按钮使用 txtQuery 文本框中的文本作为查询词，使用搜索查询结果更新名为 azResult 的新集合。  
    
1.  下一步，我们将所创建的垂直库链接到 azResult 集合  。 选择库控件，并在属性窗格中执行以下操作。

    -  将“DataSource”设置为“azResult”。  
    
    -  根据索引中的数据类型，选择合适的布局。  在本例中，我们使用了“标题、副标题和正文”布局。 
    
    -  单击“编辑字段”，并选择要可视化的字段。 

    由于我们在定义连接器时提供了示例结果，因此该应用能够识别到索引中可用的字段。
    
    :::image type="content" source="./media/search-howto-powerapps/2-7-gallery-select-fields.png" alt-text="库字段" border="true":::   
 
1.  按 F5 来预览应用。   

    请记住，可将字段设置为计算的值。      
    对于本示例，我们使用“图像、标题和副标题”布局并将 Image 函数指定为数据根路径与文件名的串联形式（例如 `"https://mystore.blob.core.windows.net/multilang/" & ThisItem.metadata_storage_name`），这一设置将生成以下结果   。

    :::image type="content" source="./media/search-howto-powerapps/2-8-2-final.png" alt-text="最终的应用" border="true":::        

## <a name="next-steps"></a>后续步骤

有关详细信息和联机培训内容，请参阅 [Power Apps 学习目录](https://docs.microsoft.com/powerapps/learning-catalog/get-started)。

