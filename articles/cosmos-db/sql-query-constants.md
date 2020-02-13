---
title: Azure Cosmos DB 中的 SQL 常量
description: 了解如何使用 Azure Cosmos DB 中的 SQL 查询常量来表示特定数据值
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 05/31/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: d79bf09362c349435705b4366a957fbb1c944333
ms.sourcegitcommit: 5c4141f30975f504afc85299e70dfa2abd92bea1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028680"
---
# <a name="azure-cosmos-db-sql-query-constants"></a>Azure Cosmos DB SQL 查询常量  

常量也称为文本或标量值，是一个表示特定数据值的符号。 常量的格式取决于它表示的值的数据类型。  

**支持的标量数据类型：**  

|**类型**|**值顺序**|  
|-|-|  
|**Undefined**|单一值：**undefined**|  
|**Null**|单一值：**null**|  
|**布尔值**|值：**false**、**true**。|  
|**数字**|双精度浮点数，按 IEEE 754 标准。|  
|**字符串**|由零个或多个 Unicode 字符构成的序列。 字符串必须括在单引号或双引号中。|  
|**数组**|由零个或多个元素构成的序列。 每个元素可以是任何标量数据类型的值，但 **Undefined** 除外。|  
|**Object**|由零个或多个名称/值对构成的无序集。 名称是一个 Unicode 字符串，值可以是任何标量数据类型，但 **Undefined** 除外。|  

<a name="bk_syntax"></a>
## <a name="syntax"></a>语法

```sql  
<constant> ::=  
   <undefined_constant>  
     | <null_constant>   
     | <boolean_constant>   
     | <number_constant>   
     | <string_constant>   
     | <array_constant>   
     | <object_constant>   

<undefined_constant> ::= undefined  

<null_constant> ::= null  

<boolean_constant> ::= false | true  

<number_constant> ::= decimal_literal | hexadecimal_literal  

<string_constant> ::= string_literal  

<array_constant> ::=  
    '[' [<constant>][,...n] ']'  

<object_constant> ::=   
   '{' [{property_name | "property_name"} : <constant>][,...n] '}'  

```  

<a name="bk_arguments"></a>
## <a name="arguments"></a>参数

* `<undefined_constant>; Undefined`  

    表示类型为 Undefined 的未定义值。  

* `<null_constant>; null`  

    表示类型为 **Null** 的 **null** 值。  

* `<boolean_constant>`  

    表示类型为 Boolean 的常量。  

* `false`  

    表示类型为 Boolean 的 **false** 值。  

* `true`  

    表示类型为 Boolean 的 **true** 值。  

* `<number_constant>`  

    表示一个常量。  

* `decimal_literal`  

    十进制文本是使用十进制表示法或科学记数法表示的数字。  

* `hexadecimal_literal`  

    十六进制文本是使用前缀“0x”和后跟的一个或多个十六进制数位表示的数字。  

* `<string_constant>`  

    表示类型为 String 的常量。  

* `string _literal`  

    字符串文本是以零个或多个 Unicode 字符序列或转义符序列表示的 Unicode 字符串。 字符串文本括在单引号 (') 或双引号 (") 中。  

允许以下转义序列：  

|**转义序列**|**说明**|**Unicode 字符**|  
|-|-|-|  
|\\'|撇号 (')|U+0027|  
|\\"|引号 (")|U+0022|  
|\\\ |反斜线号 (\\)|U+005C|  
|\\/|斜线号 (/)|U+002F|  
|\b|退格键|U+0008|  
|\f|换页符|U+000C|  
|\n|换行符|U+000A|  
|\r|回车键|U+000D|  
|\t|tab 键|U+0009|  
|\uXXXX|由 4 个十六进制数位定义的 Unicode 字符。|U+XXXX|  

## <a name="next-steps"></a>后续步骤

- [Azure Cosmos DB .NET 示例](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [模型文档数据](modeling-data.md)

<!-- Update_Description: update meta properties, wording update, update link -->