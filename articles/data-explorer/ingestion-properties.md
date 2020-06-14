---
title: Azure 数据资源管理器的数据引入属性
description: 了解 Azure 数据资源管理器支持的各种数据引入属性。
author: orspod
ms.author: v-tawe
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
origin.date: 03/19/2020
ms.date: 06/09/2020
ms.openlocfilehash: a411a3423a81f7ec20215c9faf03c475804822b7
ms.sourcegitcommit: 73697fa9c19a40d235df033400c74741e7d0f3f4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574893"
---
# <a name="azure-data-explorer-data-ingestion-properties"></a>Azure 数据资源管理器的数据引入属性 

数据引入是指在 Azure 数据资源管理器中将数据添加到表中供查询这一过程。 可以将属性添加到 `with` 关键字后面的引入命令。

## <a name="ingestion-properties"></a>引入属性

下表列出了 Azure 数据资源管理器支持的属性，对其进行了说明，并提供了示例： 

|properties              |说明                                              |示例                                             |
|----------------------|---------------------------------------------------------|----------------------------------------------------|
|`ingestionMapping`    |一个字符串值，表示如何将数据从源文件映射到表中的实际列。 使用相关的映射类型定义 `format` 值。 请参阅[数据映射](https://docs.microsoft.com/azure/data-explorer/kusto/management/mappings)。|`with (format="json", ingestionMapping = "[{\"column\":\"rownumber\", \"Properties\":{\"Path\":\"$.RowNumber\"}}, {\"column\":\"rowguid\", \"Properties\":{\"Path\":\"$.RowGuid\"}}]")`<br>（已弃用：`avroMapping`、`csvMapping`、`jsonMapping`） |
|`ingestionMappingReference`|一个字符串值，表示如何使用命名的映射策略对象将数据从源文件映射到表中的实际列。 使用相关的映射类型定义 `format` 值。 请参阅[数据映射](https://docs.microsoft.com/azure/data-explorer/kusto/management/mappings)。|`with (format="csv", ingestionMappingReference = "Mapping1")`<br>（已弃用：`avroMappingReference`、`csvMappingReference`、`jsonMappingReference`）|
|`creationTime` |日期/时间值（格式化为 ISO8601 字符串），在创建引入的数据盘区时使用。 在未指定的情况下会使用当前值 (`now()`)。 在引入的数据较旧时可以重写默认值，以便正确应用保留策略。|`with (creationTime="2017-02-13T11:09:36.7992775Z")`|
|`extend_schema`|一个布尔值，在指定的情况下会指示命令扩展表的架构（默认为 `false`）。 此选项仅适用于 `.append` 和 `.set-or-append` 命令。 进行架构扩展时，只能在表的末尾添加其他列。|如果原始表架构为 `(a:string, b:int)`，则 `(a:string, b:int, c:datetime, d:string)` 是有效的架构扩展，而 `(a:string, c:datetime)` 则不是。|
|`folder` |使用 [ingest-from-query](https://docs.microsoft.com/azure/data-explorer/kusto/management/data-ingestion/ingest-from-query) 命令时分配给表的文件夹。 如果表已存在，则此属性会重写表的文件夹|`with (folder="Tables/Temporary")`|
|`format` |数据格式（请参阅[支持的数据格式](ingestion-supported-formats.md)）。|`with (format="csv")`|
|`ingestIfNotExists`|一个字符串值，如果已指定，则在表的数据已通过一个具有相同值的 `ingest-by:` 标记进行了标记时，引入操作会失败。 这确保了幂等数据引入。 有关详细信息，请参阅 [ingest-by: tags](https://docs.microsoft.com/azure/data-explorer/kusto/management/extents-overview#ingest-by-extent-tags)。|属性 `with (ingestIfNotExists='["Part0001"]', tags='["ingest-by:Part0001"]')` 表示，如果已存在带 `ingest-by:Part0001` 标记的数据，请勿完成当前的引入操作。 如果该数据不存在，则在执行这个新的引入操作时，应该设置此标记（防止将来有人尝试再次引入相同的数据）。|
|`ignoreFirstRecord` |一个布尔值，在设置为 `true` 的情况下表示引入操作应该忽略每个文件的第一条记录。 如果文件中的第一条记录是列名称，则此属性适用于 `CSV` 和类似格式的文件。 默认为 `false`。|`with (ignoreFirstRecord=false)`|
|`persistDetails` |一个布尔值，在指定的情况下表示命令应该保留详细的结果（即使命令已成功执行），这样 [.show operation details](https://docs.microsoft.com/azure/data-explorer/kusto/management/operations#show-operation-details) 命令就能够检索它们。 默认为 `false`。|`with (persistDetails=true)`|
|`policy_ingestiontime`|一个布尔值，在指定的情况下，说明是否在通过此命令创建的表上启用[引入时间策略](https://docs.microsoft.com/azure/data-explorer/kusto/management/ingestiontimepolicy)。 默认为 `true`。|`with (policy_ingestiontime=false)`|
|`recreate_schema` |一个布尔值，在指定的情况下，说明此命令是否会重新创建表的架构。 此属性只适用于 `.set-or-replace` 命令。 在同时设置的情况下，此属性优先于 `extend_schema` 属性。|`with (recreate_schema=true)`|
|`tags`|与引入的数据（格式化为 JSON 字符串）相关联的[标记](https://docs.microsoft.com/azure/data-explorer/kusto/management/extents-overview#extent-tagging)的列表 |`with (tags="['Tag1', 'Tag2']")`|
|`validationPolicy`|一个 JSON 字符串，表示在引入期间要运行哪些验证。 请参阅[数据引入](https://docs.microsoft.com/azure/data-explorer/kusto/management/data-ingestion/index)，了解不同选项的说明。| `with (validationPolicy='{"ValidationOptions":1, "ValidationImplications":1}')`（实际上，这是默认策略）。|
|`zipPattern`|从包含 ZIP 存档的存储中引入数据时，请使用此属性。 这是一个字符串值，表示在要引入的 ZIP 存档中选择具体文件时需使用的正则表达式。  存档中的所有其他文件会被忽略。|`with (zipPattern="*.csv")`|

## <a name="next-steps"></a>后续步骤

* 详细了解[数据引入](ingest-data-overview.md)
* 详细了解[支持的数据格式](ingestion-supported-formats.md)
