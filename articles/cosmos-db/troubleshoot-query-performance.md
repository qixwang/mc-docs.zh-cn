---
title: 排查使用 Azure Cosmos DB 时遇到的查询问题
description: 了解如何识别、诊断和排查 Azure Cosmos DB SQL 查询问题。
author: rockboyfor
ms.service: cosmos-db
ms.topic: troubleshooting
origin.date: 01/14/2020
ms.date: 02/10/2020
ms.author: v-yeche
ms.subservice: cosmosdb-sql
ms.reviewer: sngun
ms.openlocfilehash: 21289f08b61106802e0621b8e0e34ca726bc70b2
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77068340"
---
# <a name="troubleshoot-query-issues-when-using-azure-cosmos-db"></a>排查使用 Azure Cosmos DB 时遇到的查询问题

本文逐步说明排查 Azure Cosmos DB 中的查询问题的一般建议方法。 尽管文档中所述的步骤不应被视作各种潜在查询问题的“以一应百”的解决方法，但其中包含了最常用的性能提示。 应使用本文档作为起点，来排查 Azure Cosmos DB 核心 (SQL) API 中的查询速度缓慢或开销较高的问题。 还可以使用[诊断日志](cosmosdb-monitor-resource-logs.md)来识别速度缓慢的或消耗大量吞吐量的查询。

可对 Azure Cosmos DB 中的查询优化进行广泛分类：降低查询请求单位 (RU) 开销的优化，以及只是降低延迟的优化。 几乎可以肯定，降低查询的 RU 开销也会降低延迟。

本文档将使用可以通过[营养](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json)数据集重新创建的示例。

### <a name="obtaining-query-metrics"></a>获取查询指标：

在 Azure Cosmos DB 中优化查询时，第一步始终是[获取查询的查询指标](profile-sql-api-query.md)。 也可以按如下所示通过 Azure 门户获取这些指标：

[ ![获取查询指标](./media/troubleshoot-query-performance/obtain-query-metrics.png) ](./media/troubleshoot-query-performance/obtain-query-metrics.png#lightbox)

获取查询指标后，将查询的“已检索文档计数”与“输出文档计数”进行比较。 使用这种比较可以确定下面要参考的相关部分。

“已检索文档计数”是查询需要加载的文档数。 “输出文档计数”是查询结果所需的文档数。 如果“已检索文档计数”明显大于“输出文档计数”，则表明查询中至少有一个部分无法利用索引，并需要执行扫描。

可以参考以下部分来了解方案的相关查询优化：

### <a name="querys-ru-charge-is-too-high"></a>查询的 RU 开销过高

#### <a name="retrieved-document-count-is-significantly-greater-than-output-document-count"></a>“已检索文档计数”明显大于“输出文档计数”

- [在索引策略中包含所需的路径](#include-necessary-paths-in-the-indexing-policy)

- [了解哪些系统函数利用索引](#understand-which-system-functions-utilize-the-index)

- [包含筛选器和 ORDER BY 子句的查询](#queries-with-both-a-filter-and-an-order-by-clause)

- [使用子查询优化 JOIN 表达式](#optimize-join-expressions-by-using-a-subquery)

<br />

#### <a name="retrieved-document-count-is-approximately-equal-to-output-document-count"></a>“已检索文档计数”约等于“输出文档计数”

- [避免跨分区查询](#avoid-cross-partition-queries)

- [按多个属性进行筛选](#filters-on-multiple-properties)

- [包含筛选器和 ORDER BY 子句的查询](#queries-with-both-a-filter-and-an-order-by-clause)

<br />

### <a name="querys-ru-charge-is-acceptable-but-latency-is-still-too-high"></a>查询的 RU 开销可接受，但延迟仍然过高

- [改善邻近性](#improve-proximity)

- [增大预配吞吐量](#increase-provisioned-throughput)

- [增大 MaxConcurrency](#increase-maxconcurrency)

- [增大 MaxBufferedItemCount](#increase-maxbuffereditemcount)

## <a name="queries-where-retrieved-document-count-exceeds-output-document-count"></a>“已检索文档计数”超过“输出文档计数”的查询

 “已检索文档计数”是查询需要加载的文档数。 “输出文档计数”是查询结果所需的文档数。 如果“已检索文档计数”明显大于“输出文档计数”，则表明查询中至少有一个部分无法利用索引，并需要执行扫描。

 下面是不完全由索引提供服务的扫描查询示例。

查询：

 ```sql
SELECT VALUE c.description
FROM c
WHERE UPPER(c.description) = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
```

查询指标：

```
Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
Output Document Count                    :               7
Output Document Size                     :             510 bytes
Index Utilization                        :            0.00 %
Total Query Execution Time               :        4,500.34 milliseconds
  Query Preparation Times
    Query Compilation Time               :            0.09 milliseconds
    Logical Plan Build Time              :            0.05 milliseconds
    Physical Plan Build Time             :            0.04 milliseconds
    Query Optimization Time              :            0.01 milliseconds
  Index Lookup Time                      :            0.01 milliseconds
  Document Load Time                     :        4,177.66 milliseconds
  Runtime Execution Times
    Query Engine Times                   :          322.16 milliseconds
    System Function Execution Time       :           85.74 milliseconds
    User-defined Function Execution Time :            0.00 milliseconds
  Document Write Time                    :            0.01 milliseconds
Client Side Metrics
  Retry Count                            :               0
  Request Charge                         :        4,059.95 RUs
```

“已检索文档计数”(60951) 明显大于“输出文档计数”(7)，因此，此查询需要执行扫描。 在本例中，系统函数 [UPPER()](sql-query-upper.md) 不利用索引。

## <a name="include-necessary-paths-in-the-indexing-policy"></a>在索引策略中包含所需的路径

索引策略应涵盖 `WHERE` 子句、`ORDER BY` 子句、`JOIN` 和大多数系统函数中包含的所有属性。 索引策略中指定的路径应与 JSON 文档中的属性相匹配（区分大小写）。

如果对[营养](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json)数据集运行简单查询，将会注意到，在为 `WHERE` 子句中的属性编制索引时，RU 开销要低得多。

### <a name="original"></a>原始

查询：

```sql
SELECT * FROM c WHERE c.description = "Malabar spinach, cooked"
```

索引策略：

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": [
        {
            "path": "/description/*"
        }
    ]
}
```

**RU 开销：** 409.51 RU

### <a name="optimized"></a>已优化

更新的索引策略：

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": []
}
```

**RU 开销：** 2.98 RU

随时可将其他属性添加到索引策略，而不会影响写入可用性或性能。 如果将新属性添加到索引，则使用此属性的查询会立即利用新的可用索引。 查询将利用正在生成的新索引。 因此，查询结果可能会不一致，因为索引正在重新生成。 如果为新属性编制索引，在索引重新生成期间，仅利用现有索引的查询将不受影响。 可以[跟踪索引转换进度](/cosmos-db/how-to-manage-indexing-policy#use-the-net-sdk-v3)。

## <a name="understand-which-system-functions-utilize-the-index"></a>了解哪些系统函数利用索引

如果表达式可以转换为一系列字符串值，则它可以使用索引，否则不可使用索引。

下面是可以使用索引的字符串函数的列表：

- STARTSWITH(str_expr, str_expr)
- LEFT(str_expr, num_expr) = str_expr
- SUBSTRING(str_expr, num_expr, num_expr) = str_expr，但前提是第一个 num_expr 为 0

下面列出了一些不使用索引并且必须加载每个文档的常用系统函数：

| **系统函数** | **优化思路** |
| --------------------------------------- |------------------------------------------------------------ |
| CONTAINS                                | 使用 Azure 搜索执行全文搜索                        |
| UPPER/LOWER                             | 不要每次在比较时都使用系统函数来规范化数据，而是在插入时将大小写规范化。 然后，诸如 ```SELECT * FROM c WHERE UPPER(c.name) = 'BOB'``` 的查询就会变成 ```SELECT * FROM c WHERE c.name = 'BOB'``` |
| 数学函数（非聚合） | 如果需要频繁计算查询中的某个值，请考虑在 JSON 文档中将此值存储为属性。 |

------

尽管系统函数不使用索引，但查询的其他部分仍可以利用索引。

## <a name="queries-with-both-a-filter-and-an-order-by-clause"></a>包含筛选器和 ORDER BY 子句的查询

尽管包含筛选器和 `ORDER BY` 子句的查询通常利用范围索引，但如果能够通过组合索引为它们提供服务，则它们会更有效。 除了修改索引策略以外，还应将组合索引中的所有属性添加到 `ORDER BY` 子句。 进行这种查询修改可确保该查询利用组合索引。  可以通过对[营养](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json)数据集运行查询来观察影响。

### <a name="original"></a>原始

查询：

```sql
SELECT * FROM c WHERE c.foodGroup = "Soups, Sauces, and Gravies" ORDER BY c._ts ASC
```

索引策略：

```json
{

        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[]
}
```

**RU 开销：** 44.28 RU

### <a name="optimized"></a>已优化

更新的查询（包含 `ORDER BY` 子句中的两个属性）：

```sql
SELECT * FROM c 
WHERE c.foodGroup = "Soups, Sauces, and Gravies" 
ORDER BY c.foodGroup, c._ts ASC
```

更新的索引策略：

```json
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/foodGroup",
                    "order":"ascending"
        },
                {  
                    "path":"/_ts",
                    "order":"ascending"
                }
            ]
        ]
    }

```

**RU 开销：** 8.86 RU

## <a name="optimize-join-expressions-by-using-a-subquery"></a>使用子查询优化 JOIN 表达式
多值子查询可以通过在 `WHERE` 子句中的每个 select-many 表达式后面（而不是所有 cross-join 后面）推送谓词，来优化 `JOIN` 表达式。

请考虑下列查询：

```sql
SELECT Count(1) AS Count
FROM c
JOIN t IN c.tags
JOIN n IN c.nutrients
JOIN s IN c.servings
WHERE t.name = 'infant formula' AND (n.nutritionValue > 0
AND n.nutritionValue < 10) AND s.amount > 1
```

**RU 开销：** 167.62 RU

对于此查询，索引将匹配包含名为“infant formula”的标记、nutritionValue 大于 0 且份量大于 1 的任何文档。 此处的 `JOIN` 表达式将针对应用任何筛选器之前的每个匹配文档，执行所有 tags、nutrients 和 servings 数组项的叉积计算。 然后，`WHERE` 子句将针对每个 `<c, t, n, s>` 元组应用筛选谓词。

例如，如果在匹配文档的三个数组中，每个数组包含 10 个项，则此子句将扩展为 1 x 10 x 10 x 10（即 1,000）个元组。 在与下一个表达式联接之前，使用此处的子查询可帮助筛选出联接的数组项。

此查询等效于前面的查询，但使用了子查询：

```sql
SELECT Count(1) AS Count
FROM c
JOIN (SELECT VALUE t FROM t IN c.tags WHERE t.name = 'infant formula')
JOIN (SELECT VALUE n FROM n IN c.nutrients WHERE n.nutritionValue > 0 AND n.nutritionValue < 10)
JOIN (SELECT VALUE s FROM s IN c.servings WHERE s.amount > 1)
```

**RU 开销：** 22.17 RU

假设 tags 数组中只有一个项与筛选器相匹配，而 nutrients 和 servings 数组各有 5 个项。 那么，`JOIN` 表达式将扩展为 1 x 1 x 5 x 5 = 25 个项，而不是第一个查询中的 1,000 个项。

## <a name="queries-where-retrieved-document-count-is-equal-to-output-document-count"></a>“已检索文档计数”等于“输出文档计数”的查询

如果“已检索文档计数”约等于“输出文档计数”，则表示查询不一定要扫描许多不必要的文档。 对于许多查询（例如，使用 TOP 关键字的查询）而言，“已检索文档计数”可比“输出文档计数”多 1。 这不会造成问题。

## <a name="avoid-cross-partition-queries"></a>避免跨分区查询

当请求单位和数据存储需求提高时，Azure Cosmos DB 将使用[分区](partitioning-overview.md)来扩展单个容器。 每个物理分区具有不同的独立索引。 如果查询包含用于匹配容器分区键的相等性筛选器，则你只需检查相关分区的索引。 这种优化可以减少查询所需的 RU 总数。

如果有大量预配的 RU（超过 30,000）或存储了大量数据（超过大约 100 GB），则你可能会获得一个足够大的容器，可以发现查询 RU 开销明显降低。

例如，如果我们创建使用分区键 foodGroup 的容器，则以下查询只需检查单个物理分区：

```sql
SELECT * FROM c
WHERE c.foodGroup = "Soups, Sauces, and Gravies" and c.description = "Mushroom, oyster, raw"
```

还可以通过在查询中包含分区键来优化这些查询：

```sql
SELECT * FROM c
WHERE c.foodGroup IN("Soups, Sauces, and Gravies", ""Vegetables and Vegetable Products") and  c.description = "Mushroom, oyster, raw"
```

对分区键使用范围筛选器或者对分区键不使用任何筛选器的查询需要“扇出”，并在每个物理分区的索引中检查结果。

```sql
SELECT * FROM c
WHERE c.description = "Mushroom, oyster, raw"
```

```sql
SELECT * FROM c
WHERE c.foodGroup > "Soups, Sauces, and Gravies" and c.description = "Mushroom, oyster, raw"
```

## <a name="filters-on-multiple-properties"></a>按多个属性进行筛选

尽管对多个属性包含筛选器的查询通常利用范围索引，但如果能够通过组合索引为它们提供服务，则它们会更有效。 对于少量的数据而言，这种优化不会产生明显的影响。 但是，对于大量的数据，它可能确实很有效。 对于每个组合索引，最多只能优化一个非相等性筛选器。 如果查询包含多个非相等性筛选器，应选取其中一个利用组合索引的筛选器。 剩余的筛选器将继续利用范围索引。 非相等性筛选器必须在组合索引中最后定义。 [详细了解组合索引](index-policy.md#composite-indexes)

下面是可以使用组合索引优化的一些查询示例：

```sql
SELECT * FROM c
WHERE c.foodGroup = "Vegetables and Vegetable Products" AND c._ts = 1575503264
```

```sql
SELECT * FROM c
WHERE c.foodGroup = "Vegetables and Vegetable Products" AND c._ts > 1575503264
```

下面是相关的组合索引：

```json
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/foodGroup",
                    "order":"ascending"
                },
                {  
                    "path":"/_ts",
                    "order":"ascending"
                }
            ]
        ]
}
```

## <a name="optimizations-that-reduce-query-latency"></a>降低查询延迟的优化：

在许多情况下，RU 开销是可接受的，但查询延迟仍然过高。 以下部分概述了降低查询延迟的提示。 如果对同一个数据集多次运行同一个查询，该查询每次都会产生相同的 RU 开销。 但是，每次执行查询时，查询延迟可能各不相同。

## <a name="improve-proximity"></a>改善邻近性

在非 Azure Cosmos DB 帐户的区域中运行的查询，比在同一区域中运行的查询的延迟更高。 例如，如果在台式机上运行代码，则延迟比从 Azure Cosmos DB 所在的同一 Azure 区域中的某个虚拟机上运行查询要高出几十或几百毫秒（或更高）。 可以轻松地[在 Azure Cosmos DB 中以多区域方式分布数据](distribute-data-globally.md)，以确保将数据放在更靠近应用的位置。

## <a name="increase-provisioned-throughput"></a>增大预配吞吐量

在 Azure Cosmos DB 中，预配的吞吐量以请求单位 (RU) 计量。 假设某个查询消耗 5 RU 吞吐量。 如果预配 1,000 RU，则每秒可以运行该查询 200 次。 如果在没有足够吞吐量的情况下尝试运行查询，Azure Cosmos DB 将返回 HTTP 429 错误。 任何当前核心 (SQL) API SDK 在等待一小段时间后，将自动重试该查询。 受限制的请求需要花费更长时间，因此提高预配的吞吐量可以改善查询延迟。 可以在 Azure 门户的“指标”边栏选项卡中观察[受限制的请求总数](use-metrics.md#understand-how-many-requests-are-succeeding-or-causing-errors)。

## <a name="increase-maxconcurrency"></a>增大 MaxConcurrency

并行查询的工作原理是并行查询多个分区。 但就查询本身而言，会按顺序提取单个已分区集合中的数据。 因此，将 MaxConcurrency 调整为分区数最有可能实现最高性能的查询，前提是其他所有系统条件保持不变。 如果不知道分区数，可将 MaxConcurrency（或旧 SDK 版本中的 MaxDegreesOfParallelism）设置为较大的数字，系统会选择最小值（分区数、用户提供的输入）作为最大并行度。

## <a name="increase-maxbuffereditemcount"></a>增大 MaxBufferedItemCount

查询设计为当客户端正在处理当前结果批时预先提取结果。 预提取帮助改进查询中的的总体延迟。 设置 MaxBufferedItemCount 可限制预先提取的结果数。 将此值设置为预期返回的结果数（或更大的数字）可让查询通过预先提取获得最大优势。 将此值设置为 -1 可让系统自动确定要缓冲的项数。

## <a name="next-steps"></a>后续步骤
参阅以下文档了解如何度量每个查询的 RU、获取用于优化查询的执行统计信息，等等：

* [使用 .NET SDK 获取 SQL 查询执行指标](profile-sql-api-query.md)
* [优化 Azure Cosmos DB 的查询性能](sql-api-sql-query-metrics.md)
* [.NET SDK 性能提示](performance-tips.md)

<!-- Update_Description: update meta properties, wording update, update link -->