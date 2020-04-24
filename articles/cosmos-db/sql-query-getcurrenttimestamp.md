---
title: Azure Cosmos DB 查询语言中的 GetCurrentTimestamp
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 GetCurrentTimestamp。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 933cf151005cd8a9757d593fee5abe495fb82831
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914728"
---
# <a name="getcurrenttimestamp-azure-cosmos-db"></a>GetCurrentTimestamp (Azure Cosmos DB)
 返回自 1970 年 1 月 1 日星期四 00:00:00 开始消逝的毫秒数。 

## <a name="syntax"></a>语法

```sql
GetCurrentTimestamp ()  
```  

## <a name="return-types"></a>返回类型

  返回一个数字值，表示自 Unix 纪元开始消逝的秒数，即自 1970 年 1 月 1 日星期四 00:00:00 开始消逝的毫秒数。

## <a name="remarks"></a>备注

  GetCurrentTimestamp() 是非确定性的函数。

  返回的结果采用 UTC（协调世界时）格式。

## <a name="examples"></a>示例

  以下示例演示如何使用 GetCurrentTimestamp() 内置函数获取当前时间戳。

```sql
SELECT GetCurrentTimestamp() AS currentUtcTimestamp
```  

 下面是示例结果集。

```json
[{
  "currentUtcTimestamp": 1556916469065
}]  
```

## <a name="next-steps"></a>后续步骤

- [日期和时间函数 Azure Cosmos DB](sql-query-date-time-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query get current timestamp  -->
<!--New.date: 10/28/2019-->