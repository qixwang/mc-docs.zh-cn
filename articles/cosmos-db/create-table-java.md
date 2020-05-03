---
title: 使用表 API 和 Java 生成应用 - Azure Cosmos DB
description: 本快速入门介绍如何在 Azure 门户和 Java 中使用 Azure Cosmos DB 表 API 创建应用程序
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: java
ms.topic: quickstart
origin.date: 04/10/2018
ms.date: 04/27/2020
ms.author: v-yeche
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 0777f6f111b7738dc12773be4fc2fb0ff4edaffe
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134949"
---
<!--Verify sucessfully-->
# <a name="quickstart-build-a-java-app-to-manage-azure-cosmos-db-table-api-data"></a>快速入门：生成 Java 应用以管理 Azure Cosmos DB 表 API 数据

> [!div class="op_single_selector"]
> * [.NET](create-table-dotnet.md)
> * [Java](create-table-java.md)
> * [Node.js](create-table-nodejs.md)
> * [Python](create-table-python.md)
> 

在本快速入门中，我们创建一个 Azure Cosmos DB 表 API 帐户，并使用从 GitHub 克隆的数据资源管理器和 Java 应用创建表和条目。 Azure Cosmos DB 是一种多模型数据库服务，可让你通过多区域分布和水平缩放功能快速创建和查询文档、表、键/值和图数据库。

## <a name="prerequisites"></a>先决条件

- 具有活动订阅的 Azure 帐户。 [免费创建一个](https://www.azure.cn/pricing/1rmb-trial/)。 你也可以通过 URI `https://localhost:8081` 和密钥 `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==` 使用 [Azure Cosmos DB 模拟器](https://aka.ms/cosmosdb-emulator)

    <!--Not Available on [try Azure Cosmos DB for free](https://www.azure.cn/try/cosmosdb/)-->

- [Java 开发工具包 (JDK) 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts&architecture=x86-64-bit&package=jdk)。 将 `JAVA_HOME` 环境变量指向其中安装了 JDK 的文件夹。
- [Maven 二进制存档](https://maven.apache.org/download.cgi)。 
- [Git](https://www.git-scm.com/downloads)。 

## <a name="create-a-database-account"></a>创建数据库帐户

> [!IMPORTANT] 
> 需创建新的适用于公开发表版表 API SDK 的表 API 帐户。 在预览期间创建的表 API 帐户不受公开发布版 SDK 的支持。
>

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>添加表

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>添加示例数据

[!INCLUDE [cosmos-db-create-table-add-sample-data](../../includes/cosmos-db-create-table-add-sample-data.md)]

## <a name="clone-the-sample-application"></a>克隆示例应用程序

现在让我们从 GitHub 克隆表应用，设置连接字符串，然后运行该应用。 会看到以编程方式处理数据是多么容易。 

1. 打开命令提示符，新建一个名为“git-samples”的文件夹，然后关闭命令提示符。

    ```bash
    md "C:\git-samples"
    ```

2. 打开诸如 git bash 之类的 git 终端窗口，并使用 `cd` 命令更改为要安装示例应用的新文件夹。

    ```bash
    cd "C:\git-samples"
    ```

3. 运行下列命令，克隆示例存储库。 此命令在计算机上创建示例应用程序的副本。

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-java-getting-started.git 
    ```

## <a name="update-your-connection-string"></a>更新连接字符串

现在返回到 Azure 门户，获取连接字符串信息，并将其复制到应用。 这样，应用程序就可以与托管的数据库进行通信。 

1. 在 [Azure 门户](https://portal.azure.cn/)中，选择“连接字符串”  。 

    ![在“连接字符串”窗格中查看连接字符串信息](./media/create-table-java/cosmos-db-quickstart-connection-string.png)

2. 使用右侧的复制按钮，复制主连接字符串。

3. 打开 C:\git-samples\storage-table-java-getting-started\src\main\resources 文件夹中的 config.properties。 

5. 注释掉第 1 行，并取消注释掉第 2 行。 前两行现在应如下所示。
    
    <!--MOONCAKE: Storage properties Append EndpointSuffix=core.chinacloudapi.cn-->
    
    ```xml
    #StorageConnectionString = UseDevelopmentStorage=true
    StorageConnectionString = "DefaultEndpointsProtocol=https;AccountName=[ACCOUNTNAME];AccountKey=[ACCOUNTKEY];EndpointSuffix=core.chinacloudapi.cn"
    ```
    
    <!--MOONCAKE: Storage properties Append EndpointSuffix=core.chinacloudapi.cn-->
    
6. 将从门户复制的主连接字符串粘贴到第 2 行的 StorageConnectionString 值中。 

    > [!IMPORTANT]
    > 如果终结点使用 documents.azure.cn，则意味着帐户为预览版帐户，需创建适用于公开发布版表 API SDK 的[新的表 API 帐户](#create-a-database-account)。
    >

7. 保存 config.properties 文件。

现已使用与 Azure Cosmos DB 进行通信所需的所有信息更新应用。 

## <a name="run-the-app"></a>运行应用程序

1. 在 git 终端窗口中，运行 `cd` 切换到 storage-table-java-getting-started 文件夹。

    ```git
    cd "C:\git-samples\storage-table-java-getting-started"
    ```

2. 在 git 终端窗口中，运行以下命令启动 Java 应用程序。

    ```git
    mvn compile exec:java 
    ```

    控制台窗口显示，正在将表数据添加到 Azure Cosmos DB 中的新表数据库。

    现可返回到数据资源管理器，查看查询、修改和处理此新数据。 

## <a name="review-slas-in-the-azure-portal"></a>在 Azure 门户中查看 SLA

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>清理资源

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>后续步骤

在本快速入门教程中，已了解如何创建 Azure Cosmos DB 帐户、使用数据资源管理器创建表和运行应用。  现在可以使用表 API 进行数据查询了。  

> [!div class="nextstepaction"]
> [将表数据导入表 API](table-import.md)

<!--Update_Description: wording update -->


