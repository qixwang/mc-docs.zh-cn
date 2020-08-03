---
title: API 版本
titleSuffix: Azure Cognitive Search
description: 适用于 .NET SDK 中的 Azure 认知搜索 REST API 和客户端库的版本策略。
manager: nitinme
author: HeidiSteen
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 06/30/2020
ms.date: 07/17/2020
ms.openlocfilehash: 6d254cf7c51deda322d229884a71edd2ade1e8da
ms.sourcegitcommit: fe9ccd3bffde0dd2b528b98a24c6b3a8cbe370bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86472062"
---
# <a name="api-versions-in-azure-cognitive-search"></a>Azure 认知搜索中的 API 版本

Azure 认知搜索会定期发布功能更新。 这些更新有时（但并非总是）需要发布 API 的新版本，以保持后向兼容性。 发布新版本使你可以控制何时以及如何在代码中集成搜索服务更新。

通常，Azure 认知搜索团队仅在必要时发布新版本，因为它可能会促使你必须升级代码才能使用新版本 API。 仅在 API 的某些方面以破坏后向兼容性的方式发生更改时，才需要新版本。 此类更改可能会因以下情况而发生：现有功能修复，或更改现有 API 外围功能的新功能。

相同规则适用于 SDK 更新。 Azure 认知搜索 SDK 遵循[语义版本控制](https://semver.org/)规则，这意味着其版本分为三个部分：主要版本号、次要版本号和内部版本号（例如，1.1.0）。 仅在更改破坏了后向兼容性时，才会发布新的 SDK 主版本。 非破坏功能更新会增大次要版本号，Bug 修复仅增大内部版本号。

> [!NOTE]
> Azure 认知搜索服务实例支持几个 REST API 版本，包括最新版本。 可以不使用最新版本，但是我们建议迁移代码，以便使用最新版本。 使用 REST API 时，必须在每个请求中通过 api-version 参数指定 API 版本。 使用 .NET SDK 时，使用的 SDK 版本确定对应的 REST API 版本。 如果使用较旧的 SDK，即使已升级服务以支持较新的 API 版本，也可以继续运行代码，而不进行任何更改。

## <a name="rest-apis"></a>REST API

此表提供当前和以前发布的搜索服务 REST API 版本历史记录。 为当前的稳定版本和预览版本发布文档。

| 版本&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | 状态 | 向后兼容性问题 |
|-------------|--------|------------------------------|
| [管理 2020-03-13](https://docs.microsoft.com/rest/api/searchmanagement/) | 正式版 | 管理 REST API 的最新稳定版本，在终结点保护方面有改进。 |
| [管理 2019-10-01-Preview](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) | 预览  | 尽管具有版本号，但它仍是管理 REST API 的当前预览版。 目前没有预览功能。 所有预览功能最近已转换为正式发布版本。 |
| 管理 2015-08-19  | Stable| 管理 REST API 的第一个正式发布版本。 提供服务预配、纵向扩展和 API 密钥管理。 |
| 管理 2015-08-19-Preview | 预览| 管理 REST API 的第一个预览版本。 |
| [搜索 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/index)| Stable | 搜索 REST API 的最新稳定版本，在相关性评分方面有改进。 |
| [搜索 2020-06-30-Preview](https://docs.microsoft.com/rest/api/searchservice/index-preview)| 预览 | 与稳定版本关联的预览版本。 |
| 搜索 2019-05-06 | Stable | 添加复杂类型。 |
| 搜索 2019-05-06-Preview | 预览 | 与稳定版本关联的预览版本。 |
| 搜索 2017-11-11 | Stable  | 添加技能集和 AI 扩充。 |
| 搜索 2017-11-11-Preview | 预览 | 与稳定版本关联的预览版本。 |
| 搜索 2016-09-01 |Stable | 添加索引器|
| 搜索 2016-09-01-Preview | 预览 | 与稳定版本关联的预览版本。|
| 搜索 2015-02-28 | Stable  | 第一个正式发布版本。  |
| 搜索 2015-02-28-Preview | 预览 | 与稳定版本关联的预览版本。 |
| 搜索 2014-10-20-Preview | 预览 | 第二个公共预览版。 |
| 搜索 2014-07-31-Preview | 预览 | 第一个公共预览版。 |

## <a name="azure-sdk-for-net"></a>用于 .NET 的 Azure SDK

NuGet.org 上提供了包版本历史记录。此表提供了每个包页的链接。

| SDK 版本 | 状态 | 说明 |
|-------------|--------|------------------------------|
| [**Azure.Search.Documents 1.0.0-preview.4**](https://www.nuget.org/packages/Azure.Search.Documents/1.0.0-preview.4) | 预览 | Azure .NET SDK 中的新客户端库，2020 年 5 月发布。 针对 REST 2020-06-30 API 版本|
| [**Microsoft.Azure.Search 10.0**](https://www.nuget.org/packages/Microsoft.Azure.Search/) | 正式版，2019 年 5 月发布。 针对 REST 2019-05-06 API 版本。|
| [**Microsoft.Azure.Search 8.0-preview**](https://www.nuget.org/packages/Microsoft.Azure.Search/8.0.0-preview) | 预览版，2019 年 4 月发布。 针对 REST 2019-05-06-Preview API 版本。|
| [**Microsoft.Azure.Management.Search 3.0.0**](https://docs.microsoft.com/dotnet/api/overview/azure/search/management?view=azure-dotnet) | Stable | 针对管理 REST api-version=2015-08-19。 |

## <a name="azure-sdk-for-java"></a>用于 Java 的 Azure SDK

| SDK 版本 | 状态 | 说明  |
|-------------|--------|------------------------------|
| [**Java SearchManagementClient 1.35.0**](https://docs.microsoft.com/java/api/overview/azure/search/management?view=azure-java-stable) | Stable | 针对管理 REST api-version=2015-08-19。|

## <a name="azure-sdk-for-python"></a>用于 Python 的 Azure SDK

| SDK 版本 | 状态 | 说明  |
|-------------|--------|------------------------------|
| [**Python azure-mgmt-search 1.0**](https://docs.microsoft.com/python/api/overview/azure/search?view=azure-python) | Stable | 针对管理 REST api-version=2015-08-19。 |