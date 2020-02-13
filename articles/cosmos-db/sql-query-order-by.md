---
title: Azure Cosmos DB 中的 ORDER BY 子句
description: 了解 Azure Cosmos DB 的 SQL ORDER BY 子句。 将 SQL 用作 Azure Cosmos DB JSON 查询语言。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 06/10/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: 8ad7fe2956745784796e116933e9f09669071acf
ms.sourcegitcommit: 925c2a0f6c9193c67046b0e67628d15eec5205c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77067861"
---
# <a name="order-by-clause-in-azure-cosmos-db"></a>Azure Cosmos DB 中的 ORDER BY 子句

可选的 ORDER BY 子句指定查询返回的结果的排序顺序。

## <a name="syntax"></a>语法

```sql  
ORDER BY <sort_specification>  
<sort_specification> ::= <sort_expression> [, <sort_expression>]  
<sort_expression> ::= {<scalar_expression> [ASC | DESC]} [ ,...n ]  
```  

## <a name="arguments"></a>参数

- `<sort_specification>`  

    指定对查询结果集进行排序时要依据的属性或表达式。 可将排序列指定为名称或属性别名。  

    可以指定多个属性。 属性名称必须唯一。 ORDER BY 子句中排序属性的顺序定义了排序结果集的组织方式。 也就是说，结果集首先按第一个属性排序，然后该有序列表按第二个属性排序，依此类推。  

    ORDER BY 子句中引用的属性名称必须与 select 列表中的某个属性或者与在 FROM 子句中指定的集合中定义的某个属性相对应，且不存在任何多义性。  

- `<sort_expression>`  

    指定一个或多个属性或表达式用作排序查询结果集的依据。  

- `<scalar_expression>`  

    有关详细信息，请参阅[标量表达式](sql-query-scalar-expressions.md)部分。  

- `ASC | DESC`  

    指定应当按升序或降序对指定列中的值进行排序。 ASC 将按照从最低值到最高值的顺序进行排序。 DESC 将按照从最高值到最低值的顺序进行排序。 ASC 是默认排序顺序。 Null 值被视为最低的可能值。  

## <a name="remarks"></a>备注  

ORDER BY 子句要求索引策略包含所要排序的字段的索引。 Azure Cosmos DB 查询运行时支持根据属性名称排序，而不支持根据计算的属性排序。 Azure Cosmos DB 支持多个 ORDER BY 属性。 若要运行包含多个 ORDER BY 属性的查询，应在所要排序的字段中定义[组合索引](index-policy.md#composite-indexes)。

> [!Note] 
> 如果对某些文档排序所依据的属性可能未定义，并且你希望在 ORDER BY 查询中检索它们，则必须显式在这些属性上创建索引。 默认索引策略不允许检索未定义排序属性的文档。

## <a name="examples"></a>示例

例如，以下查询按居住城市名称的升序检索家庭：

```sql
    SELECT f.id, f.address.city
    FROM Families f
    ORDER BY f.address.city
```

结果有：

```json
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"
      }
    ]
```

以下查询按项的创建日期检索家庭 `id`。 项 `creationDate` 是一个数字，表示纪元时间，或者自 1970 年 1 月 1 日开始消逝的时间（以秒为单位）。 

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.creationDate DESC
```

结果有：

```json
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472
      }
    ]
```

此外，可按多个属性排序。 按多个属性排序的查询需要[组合索引](index-policy.md#composite-indexes)。 请考虑下列查询：

```sql
    SELECT f.id, f.creationDate
    FROM Families f
    ORDER BY f.address.city ASC, f.creationDate DESC
```

此查询按城市名称的升序检索家庭 `id`。 如果多个项包含同一个城市名称，该查询将按 `creationDate` 的降序排序。

## <a name="next-steps"></a>后续步骤

- [入门](sql-query-getting-started.md)
- [SELECT 子句](sql-query-select.md)
- [OFFSET LIMIT 子句](sql-query-offset-limit.md)

<!-- Update_Description: wording update, update link -->