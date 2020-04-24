---
title: Azure Cosmos DB 查询语言中的 PI
description: 了解 Azure Cosmos DB 中的 SQL 系统函数 PI。
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
ms.date: 10/28/2019
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: c9896b7ac061fcd83792857542afcbe1c4981617
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72914694"
---
# <a name="pi-azure-cosmos-db"></a>PI (Azure Cosmos DB)
 返回 PI 的常量值。  

## <a name="syntax"></a>语法

```sql
PI ()  
```  

## <a name="return-types"></a>返回类型

  返回数值表达式。  

## <a name="examples"></a>示例

  下面的示例返回 `PI` 的值。  

```sql
SELECT PI() AS pi 
```  

 下面是结果集：  

```json
[{"pi": 3.1415926535897931}]  
```  

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!--Update_Description: new articles on sql query pi  -->
<!--New.date: 10/28/2019-->