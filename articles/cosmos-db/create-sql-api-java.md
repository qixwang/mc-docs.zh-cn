---
title: 快速入门 - 通过 Azure Cosmos DB 使用 Java 创建文档数据库
description: 本快速入门演示一个可以用来连接到 Azure Cosmos DB SQL API 并进行查询的 Java 代码示例
author: rockboyfor
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: quickstart
origin.date: 10/31/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.custom: seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 33c1cc2e79c36925acebe4b51c935c7c9ef4c938
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79292934"
---
# <a name="quickstart-build-a-java-app-to-manage-azure-cosmos-db-sql-api-data"></a>快速入门：生成 Java 应用以管理 Azure Cosmos DB SQL API 数据

> [!div class="op_single_selector"]
> * [.NET V3](create-sql-api-dotnet.md)
> * [.NET V4](create-sql-api-dotnet-V4.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

本快速入门介绍如何使用 Java 应用程序创建和管理 Azure Cosmos DB SQL API 帐户的文档数据库。 首先，请使用 Azure 门户创建 Azure Cosmos DB SQL API 帐户，使用 SQL Java SDK 创建 Java 应用，然后使用 Java 应用程序将资源添加到 Cosmos DB 帐户。 本快速入门中的说明适用于任何能够运行 Java 的操作系统。 完成本快速入门以后，你就会熟悉如何通过 UI 或编程方式（以首选方式为准）创建和修改 Cosmos DB 数据库和容器。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

此外： 

* [Java 开发工具包 (JDK) 版本 8](https://docs.azure.cn/java/java-supported-jdk-runtime?view=azure-java-stable)
    * 请确保设置 JAVA_HOME 环境变量，使之指向在其中安装了 JDK 的文件夹。
* [下载](https://maven.apache.org/download.cgi)和[安装](https://maven.apache.org/install.html)[Maven](https://maven.apache.org/) 二进制存档
    * 在 Ubuntu 上，可以通过运行 `apt-get install maven` 来安装 Maven。
* [Git](https://www.git-scm.com/)
    * 在 Ubuntu 上，可以通过运行 `sudo apt-get install git` 来安装 Git。

## <a name="create-a-database-account"></a>创建数据库帐户

在创建文档数据库之前，需通过 Azure Cosmos DB 创建 SQL API 帐户。

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-container"></a>添加容器

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a name="add-sample-data"></a>
## <a name="add-sample-data"></a>添加示例数据

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>查询数据

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>克隆示例应用程序

现在，让我们转到如何使用代码上来。 接下来，克隆 GitHub 中的一个 SQL API 应用，设置连接字符串，并运行该应用。 会看到以编程方式处理数据是多么容易。 

1. 运行下列命令，克隆示例存储库。 此命令在计算机上创建示例应用程序的副本。

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-java-getting-started.git
    ```

## <a name="review-the-code"></a>查看代码

此步骤是可选的。 如果有意了解如何使用代码创建数据库资源，可以查看以下代码片段。 否则，可以跳到[运行应用](#run-the-app)。 

* `CosmosClient` 初始化。 `CosmosClient` 为 Azure Cosmos 数据库服务提供客户端逻辑表示形式。 此客户端用于对服务配置和执行请求。

    ```java
    client = new CosmosClientBuilder()
        .setEndpoint(AccountSettings.HOST)
        .setKey(AccountSettings.MASTER_KEY)
        .setConnectionPolicy(defaultPolicy)
        .setConsistencyLevel(ConsistencyLevel.EVENTUAL)
        .buildClient();

    ```

* 创建 CosmosDatabase。

    ```java
    database = client.createDatabaseIfNotExists(databaseName).getDatabase();

    ```

* 创建 CosmosContainer。

    ```java
    CosmosContainerProperties containerProperties =
        new CosmosContainerProperties(containerName, "/lastName");

    //  Create container with 400 RU/s
    container = database.createContainerIfNotExists(containerProperties, 400).getContainer();

    ```

* 使用 `createItem` 方法创建项。

    ```java
    //  Create item using container that we created using sync client

    //  Use lastName as partitionKey for cosmos item
    //  Using appropriate partition key improves the performance of database operations
    CosmosItemRequestOptions cosmosItemRequestOptions = new CosmosItemRequestOptions(family.getLastName());
    CosmosItemResponse item = container.createItem(family, cosmosItemRequestOptions);

    ```

* 使用 `getItem` 和 `read` 方法执行点读取

    ```java
    CosmosItem item = container.getItem(family.getId(), family.getLastName());
    try {
        CosmosItemResponse read = item.read(new CosmosItemRequestOptions(family.getLastName()));
        double requestCharge = read.getRequestCharge();
        Duration requestLatency = read.getRequestLatency();
        System.out.println(String.format("Item successfully read with id %s with a charge of %.2f and within duration %s",
            read.getItem().getId(), requestCharge, requestLatency));
    } catch (CosmosClientException e) {
        e.printStackTrace();
        System.err.println(String.format("Read Item failed with %s", e));
    }

    ```

* 使用 `queryItems` 方法对 JSON 执行 SQL 查询。

    ```java
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.maxItemCount(10);
    queryOptions.setEnableCrossPartitionQuery(true);
    //  Set populate query metrics to get metrics around query executions
    queryOptions.populateQueryMetrics(true);

    Iterator<FeedResponse<CosmosItemProperties>> feedResponseIterator = container.queryItems(
        "SELECT * FROM Family WHERE Family.lastName IN ('Andersen', 'Wakefield', 'Johnson')", queryOptions);

    feedResponseIterator.forEachRemaining(cosmosItemPropertiesFeedResponse -> {
        System.out.println("Got a page of query result with " +
            cosmosItemPropertiesFeedResponse.getResults().size() + " items(s)"
            + " and request charge of " + cosmosItemPropertiesFeedResponse.getRequestCharge());

        System.out.println("Item Ids " + cosmosItemPropertiesFeedResponse
            .getResults()
            .stream()
            .map(Resource::getId)
            .collect(Collectors.toList()));
    });

    ```

## <a name="run-the-app"></a>运行应用程序

现在返回到 Azure 门户，获取连接字符串信息，并使用终结点信息启动应用。 这样，应用程序就可以与托管的数据库进行通信。

1. 在 git 终端窗口中，通过 `cd` 转至示例代码文件夹。

    ```bash
    cd azure-cosmos-java-getting-started
    ```

2. 在 git 终端窗口中，使用以下命令安装所需的 Java 包。

    ```bash
    mvn package
    ```

3. 在 git 终端窗口中，使用以下命令启动 Java 应用程序（将 YOUR_COSMOS_DB_HOSTNAME 替换为门户中带引号的 URI 值，将 YOUR_COSMOS_DB_MASTER_KEY 替换为门户中带引号的主键）

    ```bash
    mvn exec:java -DACCOUNT_HOST=YOUR_COSMOS_DB_HOSTNAME -DACCOUNT_KEY=YOUR_COSMOS_DB_MASTER_KEY

    ```

    此时，终端窗口显示通知，提示 FamilyDB 数据库已创建。 

4. 该应用创建名为 `AzureSampleFamilyDB` 的数据库
5. 该应用创建名为 `FamilyContainer` 的容器
6. 该应用使用对象 ID 和分区键值（在本示例中为 lastName）执行点读取。 
7. 该应用将查询项，以检索姓氏中包含 ('Andersen', 'Wakefield', 'Johnson') 的所有家庭

7. 应用不删除创建的资源。 切换回门户，以便在帐户中[清理资源](#clean-up-resources)，  以免产生费用。

## <a name="review-slas-in-the-azure-portal"></a>在 Azure 门户中查看 SLA

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>清理资源

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>后续步骤

本快速入门介绍了如何使用数据资源管理器创建 Azure Cosmos 帐户、文档数据库和容器，以及如何通过运行应用以编程方式执行同一操作。 现在可以将其他数据导入 Azure Cosmos 容器。 

> [!div class="nextstepaction"]
> [将数据导入 Azure Cosmos DB](import-data.md)

<!-- Update_Description: update meta properties, wording update, update link -->