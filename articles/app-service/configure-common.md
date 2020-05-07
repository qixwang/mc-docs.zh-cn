---
title: 在门户中配置应用
description: 了解如何在 Azure 门户中配置应用服务应用的常用设置。 应用设置、连接字符串、平台、语言堆栈、容器等
keywords: azure 应用服务, web 应用, 应用设置, 环境变量
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.topic: article
origin.date: 08/13/2019
ms.date: 03/23/2020
ms.author: v-tawe
ms.custom: seodec18
ms.openlocfilehash: 82e2fb3499fed9c0848a043b27e35dd2a7c73416
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82588577"
---
# <a name="configure-an-app-service-app-in-the-azure-portal"></a>在 Azure 门户中配置应用服务应用

本主题介绍如何使用 [Azure 门户]配置 Web 应用、移动后端或 API 应用的常用设置。

## <a name="configure-app-settings"></a>配置应用设置

<!-- For Linux apps and custom containers, App Service passes app settings to the container using the `--env` flag to set the environment variable in the container. -->

在 [Azure 门户]中搜索并选择“应用服务”  ，然后选择应用。 

![搜索应用服务](./media/configure-common/search-for-app-services.png)

在应用的左侧菜单中，选择“配置” > “应用程序设置”。  

![应用程序设置](./media/configure-common/open-ui.png)

对于 ASP.NET 和 ASP.NET Core 开发人员而言，在应用服务中设置应用设置类似于在 Web.config  或 appsettings.json  中的 `<appSettings>` 内进行设置，但应用服务中的值会替代 Web.config  或 appsettings.json  中的值。 可以在 Web.config  或 appsettings.json  中保留开发设置（例如，本地 MySQL 密码），但在应用服务中保留生产机密（例如 Azure MySQL 数据库密码）会更安全。 相同的代码在本地调试时使用开发设置，部署到 Azure 时使用生产机密。

<!-- Other language stacks, likewise, get the app settings as environment variables at runtime. For language-stack specific steps, see: -->

应用程序设置在存储时始终进行加密（静态加密）。

> [!NOTE]
> 也可以使用 [Key Vault 引用](app-service-key-vault-references.md)从 [Key Vault](/key-vault/) 解析应用设置。

### <a name="show-hidden-values"></a>显示隐藏的值

默认情况下，出于安全考虑，应用设置值会隐藏在门户中。 若要查看某项应用设置的隐藏值，请单击该项设置的“值”字段。  若要查看所有应用设置的值，请单击“显示值”按钮。 

### <a name="add-or-edit"></a>添加或编辑

若要添加新的应用设置，请单击“新建应用程序设置”。  在对话框中，可[将设置绑定到当前槽](deploy-staging-slots.md#which-settings-are-swapped)。

若要编辑设置，请单击右侧的“编辑”按钮。 

完成后，单击“更新”。  别忘了返回“配置”页并单击“保存”。  

<!-- > [!NOTE] -->
<!-- > In a default Linux container or a custom Linux container, any nested JSON key structure in the app setting name like `ApplicationInsights:InstrumentationKey` needs to be configured in App Service as `ApplicationInsights__InstrumentationKey` for the key name. In other words, any `:` should be replaced by `__` (double underscore). -->

### <a name="edit-in-bulk"></a>批量编辑

若要批量添加或编辑应用设置，请单击“高级编辑”按钮。  完成后，单击“更新”。  别忘了返回“配置”页并单击“保存”。  

应用设置采用以下 JSON 格式：

```json
[
  {
    "name": "<key-1>",
    "value": "<value-1>",
    "slotSetting": false
  },
  {
    "name": "<key-2>",
    "value": "<value-2>",
    "slotSetting": false
  },
  ...
]
```

## <a name="configure-connection-strings"></a>配置连接字符串

在 [Azure 门户]中搜索并选择“应用服务”  ，然后选择应用。 在应用的左侧菜单中，选择“配置” > “应用程序设置”。  

![应用程序设置](./media/configure-common/open-ui.png)

对于 ASP.NET 和 ASP.NET Core 开发人员而言，在应用服务中设置连接字符串类似于在 *Web.config* 中的 `<connectionStrings>` 内进行设置，但应用服务中设置的值会替代 *Web.config* 中的值。可将开发设置（例如，数据库文件）保留在 Web.config  中，并将生产机密（例如，SQL 数据库凭据）安全保留在应用服务中。 相同的代码在本地调试时使用开发设置，部署到 Azure 时使用生产机密。

对于其他语言堆栈，最好是改用[应用设置](#configure-app-settings)，因为连接字符串需要在变量键中使用特殊的格式才能访问值。 但以下情况例外：如果在应用中配置了相应的连接字符串，则某些 Azure 数据库类型会连同应用一起备份。 有关详细信息，请参阅[备份的内容](manage-backup.md#what-gets-backed-up)。 如果不需要这种自动化备份，请使用应用设置。

在运行时，连接字符串可用作环境变量，其前缀为以下连接类型：

* SQL Server： `SQLCONNSTR_`
* MySQL： `MYSQLCONNSTR_`
* SQL 数据库： `SQLAZURECONNSTR_`
* 自定义：`CUSTOMCONNSTR_`

例如，可以使用环境变量 `MYSQLCONNSTR_connectionString1` 的形式访问名为 *connectionstring1* 的 MySql 连接字符串。 

<!--  For language-stack specific steps, see: -->

连接字符串在存储时始终进行加密（静态加密）。

> [!NOTE]
> 也可以使用 [Key Vault 引用](app-service-key-vault-references.md)从 [Key Vault](/key-vault/) 解析连接字符串。

### <a name="show-hidden-values"></a>显示隐藏的值

默认情况下，出于安全考虑，连接字符串的值会隐藏在门户中。 若要查看连接字符串的隐藏值，只需单击该字符串的“值”字段。  若要查看所有连接字符串的值，请单击“显示值”按钮。 

### <a name="add-or-edit"></a>添加或编辑

若要添加新的连接字符串，请单击“新建连接字符串”。  在对话框中，可[将连接字符串绑定到当前槽](deploy-staging-slots.md#which-settings-are-swapped)。

若要编辑设置，请单击右侧的“编辑”按钮。 

完成后，单击“更新”。  别忘了返回“配置”页并单击“保存”。  

### <a name="edit-in-bulk"></a>批量编辑

若要批量添加或编辑连接字符串，请单击“高级编辑”按钮。  完成后，单击“更新”。  别忘了返回“配置”页并单击“保存”。  

连接字符串采用以下 JSON 格式：

```json
[
  {
    "name": "name-1",
    "value": "conn-string-1",
    "type": "SQLServer",
    "slotSetting": false
  },
  {
    "name": "name-2",
    "value": "conn-string-2",
    "type": "PostgreSQL",
    "slotSetting": false
  },
  ...
]
```

<a name="platform"></a>
<a name="alwayson"></a>

## <a name="configure-general-settings"></a>配置常规设置

在 [Azure 门户]中搜索并选择“应用服务”  ，然后选择应用。 在应用的左侧菜单中，选择“配置” > “常规设置”。  

![常规设置](./media/configure-common/open-general.png)

在此处可以配置应用的某些常用设置。 某些设置要求[纵向扩展到更高的定价层](manage-scale-up.md)。

<!-- For Linux apps and custom container apps, you can also set an optional start-up command or file. -->

- **堆栈设置**：用于运行应用的软件堆栈，包括语言和 SDK 版本。
- **平台设置**：用于配置托管平台的设置，包括：
    - **位数**：32 位或 64 位。
    - **WebSocket 协议**：例如，[ASP.NET SignalR] 或 [socket.io](https://socket.io/)。
    - **Always On**：即使没有流量，也保持应用的加载状态。 对于连续性 WebJobs 或使用 CRON 表达式触发的 WebJobs，它是必需的。
      > [!NOTE]
      > 使用 Always On 功能，你无法控制终结点。 它始终向应用程序根发送请求。
    - **托管管道版本**：IIS [管道模式]。 如果某个旧式应用需要旧版 IIS，请将此选项设置为“经典”。 
    - **HTTP 版本**：设置为 **2.0**，以启用对 HTTPS/2 协议的支持。
    > [!NOTE]
    > 大多数新型浏览器仅支持通过 TLS 的 HTTP/2 协议，而非加密流量继续使用 HTTP/1.1。 为确保客户端浏览器通过 HTTP/2 连接到应用，请[在 Azure 应用服务中使用 SSL 绑定保护自定义 DNS 名称](configure-ssl-bindings.md)。
    - **ARR 相关性**：在多实例部署中，请确保在会话的整个生存期内，将客户端路由到同一实例。 对于无状态应用程序，请将此选项设置为“关闭”。 
- **调试**：为 [ASP.NET](troubleshoot-dotnet-visual-studio.md#remotedebug)、[ASP.NET Core](/visualstudio/debugger/remote-debugging-azure) 启用远程调试。 此选项在 48 小时后会自动关闭。
- **传入的客户端证书**：要求在[相互身份验证](app-service-web-configure-tls-mutual-auth.md)中使用客户端证书。

## <a name="configure-default-documents"></a>配置默认文档

此设置仅适用于 Windows 应用。

在 [Azure 门户]中搜索并选择“应用服务”  ，然后选择应用。 在应用的左侧菜单中，选择“配置” > “默认文档”。  

![默认文档](./media/configure-common/open-documents.png)

默认文档是在网站的根 URL 中显示的网页。 使用列表中第一个匹配文件。 若要添加新的默认文档，请单击“新建文档”。  别忘了单击“保存”。 

如果应用使用的模块基于 URL 进行路由而不是提供静态内容，则无需使用默认文档。

## <a name="configure-path-mappings"></a>配置路径映射

在 [Azure 门户]中搜索并选择“应用服务”  ，然后选择应用。 在应用的左侧菜单中，选择“配置” > “路径映射”。  

![路径映射](./media/configure-common/open-path.png)

“路径映射”页根据 OS 类型显示不同的内容。 

### <a name="windows-apps-uncontainerized"></a>Windows 应用（未容器化）

对于 Windows 应用，可以自定义 IIS 处理程序映射和虚拟应用程序与目录。

使用处理程序映射可以添加自定义脚本处理程序用于处理特定文件扩展名的请求。 若要添加自定义处理程序，请单击“新建处理程序”。  按如下所述配置处理程序：

- **扩展名**。 要处理的扩展名，例如 *\*.php* 或 *handler.fcgi*。
- **脚本处理程序**。 脚本处理程序的绝对路径。 与文件扩展名匹配的文件请求由脚本处理程序处理。 使用路径 `D:\home\site\wwwroot` 表示应用的根目录。
- **参数**。 脚本处理程序的可选命令行参数

每个应用具有已映射到 `D:\home\site\wwwroot`（代码的默认部署位置）的默认根路径 (`/`)。 如果应用根位于其他文件夹中，或者存储库包含多个应用程序，则你可以在此处编辑或添加虚拟应用程序和目录。 单击“新建虚拟应用程序或目录”。 

若要配置虚拟应用程序和目录，请指定每个虚拟目录及其相对于网站根目录 (`D:\home`) 的物理路径。 还可选中“应用程序”  复选框，将虚拟目录标记为应用程序。

<!-- ### Containerized apps -->

## <a name="next-steps"></a>后续步骤

- [在 Azure 应用服务中配置自定义域名]
- [设置 Azure 应用服务中的过渡环境]
- [在 Azure 应用服务中使用 SSL 绑定保护自定义 DNS 名称](configure-ssl-bindings.md)
- [启用诊断日志](troubleshoot-diagnostic-logs.md)
- [在 Azure 应用服务中缩放应用]
- [在 Azure 应用服务中监视基础知识]
- [使用 applicationHost.xdt 更改 applicationHost.config 设置](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)

<!-- URL List -->

[ASP.NET SignalR]: https://www.asp.net/signalr
[Azure 门户]: https://portal.azure.cn/
[在 Azure 应用服务中配置自定义域名]: ./app-service-web-tutorial-custom-domain.md
[设置 Azure 应用服务中的过渡环境]: ./deploy-staging-slots.md
[How to: Monitor web endpoint status]: /app-service/web-sites-monitor#webendpointstatus
[在 Azure 应用服务中监视基础知识]: ./web-sites-monitor.md
[管道模式]: https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[在 Azure 应用服务中缩放应用]: ./manage-scale-up.md
