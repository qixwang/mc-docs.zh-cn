---
author: orspod
ms.service: data-explorer
ms.topic: include
origin.date: 01/08/2020
ms.date: 02/17/2020
ms.author: v-tawe
ms.openlocfilehash: 5abc5020d2cafc8b1ccc11d1c4ac595d602dcbae
ms.sourcegitcommit: 7c80405a6b48380814b4b414e9f8a5756c007880
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77067748"
---
### <a name="event-system-properties-mapping"></a>事件系统属性映射

如果在上表的“数据源”  部分中选择了“事件系统属性”  ，请转到 [Web UI](https://dataexplorer.azure.cn/) 运行相关 KQL 命令以正确创建映射。

   **对于 csv 映射：**

    ```kusto
    .create table MyTable ingestion csv mapping "CsvMapping1"
    '['
    '   { "column" : "messageid", "DataType":"string", "Properties":{"Ordinal":"0"}},'
    '   { "column" : "userid", "DataType":"string", "Properties":{"Ordinal":"1"}},'
    '   { "column" : "other", "DataType":"int", "Properties":{"Ordinal":"2"}}'
    ']'
    ```
 
   **对于 json 映射：**

    ```kusto
    .create table MyTable ingestion json mapping "JsonMapping1"
    '['
    '    { "column" : "messageid", "datatype" : "string", "Properties":{"Path":"$.message-id"}},'
    '    { "column" : "userid", "Properties":{"Path":"$.user-id"}},'
    '    { "column" : "other", "Properties":{"Path":"$.other"}}'
    ']'
    ```

   > [!TIP]
   > * 必须在映射中包含所有选定属性。 
   > * 在 csv 映射中，属性顺序很重要。 系统属性必须列在其他所有属性之前，并且顺序必须与它们在“事件系统属性”  下拉列表中的显示顺序相同。