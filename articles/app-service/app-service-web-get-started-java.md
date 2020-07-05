---
title: 快速入门：在 Windows 上创建 Java 应用
description: 在数分钟内将第一个 Java Hello World 部署到 Windows 上的 Azure 应用服务。 可以通过 Maven 的 Azure Web 应用插件方便地部署 Java 应用。
keywords: azure, 应用服务, web 应用, windows, java, maven, 快速入门
author: msangapu-msft
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.devlang: Java
ms.topic: quickstart
origin.date: 05/29/2019
ms.date: 06/22/2020
ms.author: v-tawe
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 6865f6222a649eb937f5d51304e6f4a7c786dfcf
ms.sourcegitcommit: d24e12d49708bbe78db450466eb4fccbc2eb5f99
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85613399"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service-on-windows"></a>快速入门：在 Windows 上的 Azure 应用服务中创建 Java 应用

<!--
> [!NOTE]
> This article deploys an app to App Service on Windows. To deploy to App Service on _Linux_, see [Create Java web app on Linux](./containers/quickstart-java.md).
>
-->

[Azure 应用服务](overview.md)提供高度可缩放、自修复的 Web 托管服务。  本快速入门介绍如何将 [Azure CLI](/cli/get-started-with-azure-cli) 与[用于 Maven 的 Azure Web 应用插件](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)配合使用来部署 Java Web 存档 (WAR) 文件。

> [!NOTE]
> 也可使用 IntelliJ 和 Eclipse 等常见 IDE 执行相同的操作。 请查看 [Azure Toolkit for IntelliJ 快速入门](/java/intellij/azure-toolkit-for-intellij-create-hello-world-web-app)或 [Azure Toolkit for Eclipse 快速入门](/java/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app)中的类似文档。
>
![在 Azure 应用服务中运行的示例应用](./media/app-service-web-get-started-java/java-hello-world-in-browser-azure-app-service.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<!-- [!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] -->

## <a name="create-a-java-app"></a>创建 Java 应用

在 Azure CLI 提示符下，执行以下 Maven 命令来创建一个名为 `helloworld` 的新应用：

```bash
mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp -Dversion=1.0-SNAPSHOT
```

然后，将工作目录更改为项目文件夹：

```bash
cd helloworld
```

## <a name="configure-the-maven-plugin"></a>配置 Maven 插件

可以在命令提示符中运行以下 maven 命令以配置部署，在第一步中为 Windows OS 选择“2”并按 Enter 接受默认配置，直到出现“确认(Y/N)”提示，然后按“y”完成配置    。 

```bash
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
```

示例过程如下所示：

```xml
<plugins>
    <!--*************************************************-->
    <!-- Deploy to Tomcat in App Service Windows         -->
    <!--*************************************************-->
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.9.0</version>
        <configuration>
            <!-- Specify v2 schema -->
            <schemaVersion>v2</schemaVersion>
            <!-- App information -->
            <subscriptionId>SUBSCRIPTION_ID</subscriptionId>
            <resourceGroup>RESOURCEGROUP_NAME</resourceGroup>
            <appName>WEBAPP_NAME</appName>
            <region>REGION</region>
            <!-- Java Runtime Stack for App Service on Windows-->
            <runtime>
                <os>windows</os>
                <javaVersion>1.8</javaVersion>
                <webContainer>tomcat 9.0</webContainer>
            </runtime>
            <deployment>
                <resources>
                    <resource>
                        <directory>${project.basedir}/target</directory>
                        <includes>
                            <include>*.war</include>
                        </includes>
                    </resource>
                </resources>
            </deployment>
        </configuration>
    </plugin>
</plugins>
```

<!-- > [!NOTE] -->
<!-- > In this article we are only working with Java apps packaged in WAR files. The plugin also supports JAR web applications, visit [Deploy a Java SE JAR file to App Service on Linux](/java/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) to try it out. -->

打开 `pom.xml`，查看更新后的配置。

```bash
code pom.xml
```

如果需要，可以直接在 pom 文件中修改应用服务的配置，下面列出了一些常见配置：

 属性 | 必须 | 说明 | 版本
---|---|---|---
`<schemaVersion>` | false | 指定配置架构的版本。 支持的值是：`v1`、`v2`。 | 1.5.2
`<resourceGroup>` | 是 | 用于 Web 应用的 Azure 资源组。 | 0.1.0+
`<appName>` | 是 | Web 应用的名称。 | 0.1.0+
`<region>` | 是 | 指定将托管 Web 应用的区域；默认值为“westeurope”。 [支持的区域](/java/api/overview/maven/azure-webapp-maven-plugin/readme)部分中列出了所有有效区域。 | 0.1.0+
`<pricingTier>` | false | Web 应用的定价层。 默认值为 **P1V2**。| 0.1.0+
`<runtime>` | 是 | 运行时环境配置，可以在[此处](/java/api/overview/maven/azure-webapp-maven-plugin/readme)查看详细信息。 | 0.1.0+
`<deployment>` | 是 | 部署配置，可以在[此处](/java/api/overview/maven/azure-webapp-maven-plugin/readme)查看详细信息。 | 0.1.0+

> [!div class="nextstepaction"]
> [我遇到了问题](https://www.research.net/r/javae2e?tutorial=app-service-web-get-started-java&step=config)

## <a name="deploy-the-app"></a>部署应用

部署到 Azure 应用服务的过程中会使用 Azure CLI 中的帐户凭据。 在继续操作之前[使用 Azure CLI 登录](/cli/authenticate-azure-cli?view=azure-cli-latest)。

```azurecli
az cloud set -n AzureChinaCloud
az login
```
然后可以使用以下命令将 Java 应用部署到 Azure：

```bash
mvn package azure-webapp:deploy
```

部署完成后，在 Web 浏览器中使用以下 URL 浏览到已部署的应用程序，例如 `http://<webapp>.chinacloudsites.cn/`。

![在 Azure 应用服务中运行的示例应用](./media/app-service-web-get-started-java/java-hello-world-in-browser-azure-app-service.png)

**祝贺你！** 现已将第一个 Java 应用部署到 Windows 应用服务。

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>后续步骤
> [!div class="nextstepaction"]
> [使用 Java 连接到 Azure SQL 数据库](/sql-database/sql-database-connect-query-java?toc=%2Fjava%2Ftoc.json)

> [!div class="nextstepaction"]
> [使用 Java 连接到 Azure DB for MySQL](/mysql/connect-java)

> [!div class="nextstepaction"]
> [使用 Java 连接到 Azure DB for PostgreSQL](/postgresql/connect-java)

> [!div class="nextstepaction"]
> [面向 Java 开发人员的 Azure 资源](/java/)

> [!div class="nextstepaction"]
> [映射自定义域](app-service-web-tutorial-custom-domain.md)

> [!div class="nextstepaction"]
> [详细了解 Azure 的 Maven 插件](https://github.com/microsoft/azure-maven-plugins)
