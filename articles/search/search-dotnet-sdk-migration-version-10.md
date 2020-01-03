---
title: 升级到 Azure 认知搜索 .NET SDK 版本 10
titleSuffix: Azure Cognitive Search
description: 从旧版本将代码迁移到 Azure 认知搜索 .NET SDK 版本 10。 了解新增功能和所需的代码更改。
manager: nitinme
author: arv100kri
ms.author: v-tawe
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
origin.date: 11/04/2019
ms.date: 12/16/2019
ms.openlocfilehash: 099060c94adecba262766d23d4490cdc9b572fd9
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75335463"
---
# <a name="upgrade-to-azure-cognitive-search-net-sdk-version-10"></a>升级到 Azure 认知搜索 .NET SDK 版本 10

如果你使用的是 9.0 或更低版本的 [Azure 搜索 .NET SDK](https://aka.ms/search-sdk)，本文可帮助你升级应用程序，以使用版本 10。

Azure 搜索在版本 10 中重命名为 Azure 认知搜索，但命名空间和包名不变。 以前的 SDK 版本（9.0 及更低版本）仍使用以前的名称。 若要详细了解如何使用 SDK（包括示例），请参阅[如何从 .NET 应用程序使用 Azure 认知搜索](search-howto-dotnet-sdk.md)。

版本 10 中增加了多项功能和 bug 修复，其功能级别与最新的 REST API 版本 `2019-05-06` 相同。 如果某项更改会中断现有的代码，我们将[逐步引导你解决相关的问题](#UpgradeSteps)。

> [!NOTE]
> 如果你使用的是版本 8.0-preview 或更低版本，应先升级到版本 9，再升级到版本 10。 有关说明，请参阅[升级到 Azure 搜索 .NET SDK 版本 9](search-dotnet-sdk-migration-version-9.md)。
>
> 搜索服务实例支持多个 REST API 版本，包括最新的版本。 可以不使用最新版本，但是我们建议迁移代码，以便使用最新版本。 使用 REST API 时，必须在每个请求中通过 api-version 参数指定 API 版本。 使用 .NET SDK 时，使用的 SDK 版本确定对应的 REST API 版本。 如果使用较旧的 SDK，即使已升级服务以支持较新的 API 版本，也可以继续运行代码，而不进行任何更改。

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-10"></a>版本 10 中的新增功能
Azure 认知搜索 .NET SDK 版本 10 面向 REST API 的最新正式版 (`2019-05-06`)，其中包含以下更新：

<!-- * Introduction of two new skills - [Conditional skill](cognitive-search-skill-conditional.md) and [Text Translation skill](cognitive-search-skill-text-translation.md). -->
<!-- * [Shaper skill](cognitive-search-skill-shaper.md) inputs have been restructured to accommodate consolidation from nested contexts. For more information, see this [example JSON definition](https://docs.azure.cn/search/cognitive-search-skill-shaper#scenario-3-input-consolidation-from-nested-contexts). -->

* 添加了两个新的[字段映射函数](search-indexer-field-mappings.md)：
    - [urlEncode](https://docs.azure.cn/search/search-indexer-field-mappings#urlencode-function)
    - [urlDecode](https://docs.azure.cn/search/search-indexer-field-mappings#urldecode-function)
* 在某些情况下，[索引器执行状态](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status)中显示的错误和警告可以提供有助于调试的更多详细信息。 `IndexerExecutionResult` 已更新，以反映此行为。
* `ServiceLimits` 显示[复杂类型](https://docs.azure.cn/search/search-howto-complex-data-types)的限制，`IndexerExecutionInfo` 显示相关的索引器限制/配额。

<!-- * Individual skills defined within a [skillset](cognitive-search-defining-skillset.md) can optionally be identified by specifying a `name` property. -->

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>升级步骤

1. 按以下方式更新 `Microsoft.Azure.Search` 的 NuGet 引用：使用 NuGet 包管理器控制台，或者在 Visual Studio 中右键单击项目引用，然后选择“管理 NuGet 程序包...”。

2. 在 NuGet 已下载新的程序包及其依赖项后，请重新生成项目。 

3. 如果生成失败，则需要修复每个生成错误。 有关如何解决每个潜在生成错误的详细信息，请参阅[版本 10 中的中断性变更](#ListOfChanges)。

4. 在修复了任何生成错误或警告后，可以对应用程序进行更改，以利用新功能（如果愿意）。 有关 SDK 中的新功能的详细信息，请参阅[版本 10 中的新增功能](#WhatsNew)。

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-10"></a>版本 10 中的中断性变更

除重新生成应用程序以外，版本 10 中还有几项可能需要更改代码的中断性变更。

> [!NOTE]
> 以下更改列表并不详尽。 某些更改可能不会导致生成错误，但在技术上是中断性的，因为它们会破坏与依赖于早期版本 Azure 认知搜索.NET SDK 程序集的程序集的二进制兼容性。 属于此类别的重大更改也会随建议一起列出。 升级到版本 10 时请重新生成应用程序，以免出现任何二进制兼容性问题。

<!-- ### Custom Web API skill definition -->

<!-- The definition of the [Custom Web API skill](cognitive-search-custom-skill-web-api.md) was incorrectly specified in version 9 and older.  -->

## <a name="shaper-skill-allows-nested-context-consolidation"></a>整形程序技能允许嵌套的上下文合并

整形程序技能现在允许合并嵌套上下文中的输入。 为了实现此更改，我们修改了 `InputFieldMappingEntry`，这样，只需指定 `Source` 属性，或者同时指定 `SourceContext` 和 `Inputs` 属性，即可将其实例化。

你很有可能不需要进行任何代码更改；但请注意，只允许这两种组合中的一种。 这意味着：

- 创建只初始化 `Source` 的 `InputFieldMappingEntry` 是有效操作。
- 创建只初始化 `SourceContext` 和 `Inputs` 的 `InputFieldMappingEntry` 是有效操作。
- 涉及这三个属性的所有其他组合是无效的。

如果你决定开始利用此新功能，请确保在实施该更改之前，先将所有客户端更新为使用版本 10。 否则，客户端（使用较旧版本的 SDK）对整形程序技能做出的更新可能导致验证错误。

<!-- > [!NOTE] -->
<!-- > Even though the underlying `InputFieldMappingEntry` model has been modified to allow consolidation from nested contexts, it's use is only valid within the definition of a Shaper skill. Using this capability in other skills, while valid at compile time, will result in a validation error at runtime. -->

<!-- ## Skills can be identified by a name -->

## <a name="details-about-errors-and-warnings"></a>有关错误和警告的详细信息

`ItemError` 和 `ItemWarning` 模型（分别用于封装索引器执行期间发生的错误和警告的详细信息）已经过修改，现在包含三个旨在帮助调试索引器的新属性。 这些属性为：

- `Name`：错误来源的名称。
- `Details`：有关错误或警告的其他详细信息。
- `DocumentationLink`：针对具体错误或警告的故障排除指南的链接。

> [!NOTE]
> 我们已开始组织错误和警告的内容，以尽可能地包含这些有用的信息。 我们正努力确保在所有错误和警告中提供这些详细信息，但这项工作正在进行，并且不一定总会填充这些附加的详细信息。

## <a name="next-steps"></a>后续步骤

<!-- - Changes to the Shaper skill have the most potential impact on new or existing code. As a next step, be sure to revisit this example illustrating the input structure: [Shaper skill JSON definition example](cognitive-search-skill-shaper.md) -->
<!-- - Go through the [AI enrichment overview](cognitive-search-concept-intro.md). -->

- 我们欢迎你对 SDK 提供反馈。 如果遇到问题，请随时通过 [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-search) 向我们寻求帮助。 如果找到 Bug，可以在 [Azure .NET SDK GitHub 存储库](https://github.com/Azure/azure-sdk-for-net/issues)中提出问题。 务必在问题标题上加前缀“[Azure 认知搜索]”。

