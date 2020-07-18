---
title: 在 **iframe** 中嵌入 Web UI - Azure 数据资源管理器
description: 本文介绍了 Azure 数据资源管理器中的“在 **iframe** 中嵌入 Web UI”。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/19/2020
ms.date: 07/01/2020
ms.openlocfilehash: 1430740df5888e0a0d931f7fd6d11f042a5dcb33
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226335"
---
# <a name="embed-web-ui-in-an-iframe"></a>在 iframe 中嵌入 Web UI

可以在 iframe 中嵌入 Azure 数据资源管理器 Web UI，并将其承载在第三方网站中。
 
:::image type="content" source="../images/host-web-ux-in-iframe/web-ux.png" alt-text="Azure 数据资源管理器 Web UI":::

在网站中嵌入 Azure 数据资源管理器 Web UX 后，你的用户就可以执行以下操作：

- 编辑查询（包括所有语言功能，例如着色和 intellisense）
- 直观地浏览表架构
- 向 Azure AD 进行身份验证
- 执行查询
- 显示查询执行结果
- 创建多个选项卡
- 在本地保存查询
- 通过电子邮件共享查询

所有功能都针对辅助功能进行了测试，并且支持深色和浅色屏幕主题。

## <a name="use-monaco-kusto-or-embed-the-web-ui"></a>使用 Monaco-Kusto 还是嵌入 Web UI？

Monaco-Kusto 提供编辑体验，例如完成、着色、重构、重命名和“转到定义”。 它要求你针对身份验证、查询执行、结果显示和架构探索来构建解决方案，但你可以十分灵活地根据自己的需要改变用户体验。

通过嵌入 Azure 数据资源管理器 Web UI，可以很轻松地实现丰富的功能，但针对用户体验提供的灵活性有限。 可以通过一组固定的查询参数对系统的外观和行为进行有限的控制。

## <a name="how-to-embed-the-web-ui-in-an-iframe"></a>如何在 iframe 中嵌入 Web UI

### <a name="host-the-website-in-an-iframe"></a>将网站承载在 iframe 中

将以下代码添加到网站：

```html
<iframe
  src="https://dataexplorer.azure.cn/clusters/<cluster>?ibizaPortal=true"
></iframe>
```

`ibizaPortal` 查询参数告知 Azure 数据资源管理器 Web UI 不要通过重定向来获取身份验证令牌。 此操作是必需的，因为承载网站负责为嵌入的 iframe 提供身份验证令牌。

将 `<cluster>` 替换为要加载到连接窗格中的群集的主机名，例如 `help.kusto.chinacloudapi.cn`。 默认情况下，iframe 嵌入模式不提供从 UI 添加群集的方法，因为假定承载网站知道所需的群集。

### <a name="handle-authentication"></a>处理身份验证

1. 如果设置为“iframe 模式”(`ibizaPortal=true`)，则 Azure 数据资源管理器 Web UI 不会尝试通过重定向进行身份验证。 Web UI 将使用浏览器所用的消息发布机制来请求和接收令牌。 在页面加载过程中，以下消息将发布到父窗口：

   ```javascript
   window.parent.postMessage(
     {
       signature: "queryExplorer",
       type: "getToken"
     },
     "*"
   );
   window.addEventListener(
     "message",
     event => this.handleIncomingMessage(event),
     false
   );
   ```

1. 然后，它将侦听具有以下结构的消息：

   ```json
   {
     "type": "postToken",
     "message": "${the actual authentication token}"
   }
   ```

1. 提供的令牌应该是从 [[AAD 身份验证终结点]](../../management/access-control/how-to-authenticate-with-aad.md#web-client-javascript-authentication-and-authorization)获取的 [JWT 令牌](https://tools.ietf.org/html/rfc7519)。

> [!IMPORTANT]
> 承载窗口必须在过期之前刷新令牌，并使用同一机制为应用程序提供更新的令牌。 否则，在令牌过期后，服务调用将失败。

### <a name="feature-flags"></a>功能标志

承载应用程序可能希望控制用户体验的某些方面。 例如，隐藏连接窗格或禁止连接到其他群集。
针对这种情况，Web 浏览器支持功能标志。

功能标志可在 URL 中用作查询参数。 如果承载应用程序希望禁止添加其他群集，它应使用 https://dataexplorer.azure.cn/?ShowConnectionButtons=false

| 设置                 | 说明                                                                                                        | 默认值 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------ | ------------- |
| ShowShareMenu           | 显示“共享”菜单项                                                                                           | 是          |
| ShowConnectionButtons   | 显示用于添加新群集的“添加连接”按钮                                                            | 是          |
| ShowOpenNewWindowButton | 显示“在 Web 中打开”UI 按钮，该按钮打开一个新的浏览器窗口，并指向在作用域中具有适当群集和数据库的 https://dataexplorer.azure.cn           | false         |
| ShowFileMenu            | 显示文件菜单（“下载”、“选项卡”、“内容”等）                                                 | 是          |
| ShowToS                 | 在设置对话框中显示“链接到 Azure 数据资源管理器的服务条款”                             | 是          |
| ShowPersona             | 在右上角的设置菜单中显示用户名                                                 | 是          |
| IFrameAuth              | 如果为 true，则 Web 浏览器将指望 iframe 来处理身份验证并通过消息来提供令牌。 对于 iframe 方案，此过程将始终为 true                                                                                                                                      | false         |
| PersistAfterEachRun     | 通常，Web 浏览器在卸载事件中会持久保留。 当承载在 iframe 中时，它并不总是激发。 此标志会在每次运行查询后触发“保留本地状态”。 因此，数据丢失只会影响从未运行过的文本，因而其影响有限          | false         |
| ShowSmoothIngestion     | 如果为 true，则在右键单击数据库时显示一键式引入体验                                   | 是          |
| RefreshConnection       | 如果为 true，则在加载页面时始终刷新架构，而不依赖于本地存储                      | false         |
| ShowPageHeader          | 如果为 true，则显示包含 Azure 数据资源管理器标题和设置的页眉                            | 是          |
| HideConnectionPane      | 如果为 true，则左侧的连接窗格不会显示                                                                  | false         |
| SkipMonacoFocusOnInit   | 修复了承载在 iframe 上时的焦点问题                                                                       | false         |

### <a name="feature-flag-presets"></a>功能标志预设

预设可一次性设置一组功能标志。
目前只有一个预设。

`IbizaPortal` - 在默认设置中更改以下标志。

```json
ShowOpenNewWindowButton: true,
PersistAfterEachRun: true,
IFrameAuth: true,
ShowPageHeader: false,                                 |
```

> [!WARNING]
> 如果使用预设，将无法在其基础上添加其他功能标志。 如果你需要能够灵活地添加其他功能标志，则应使用单独的功能标志。
