---
title: 模板语法和表达式
description: 介绍 Azure 资源管理器模板的声明性 JSON 语法。
ms.topic: conceptual
origin.date: 09/03/2019
ms.author: v-yeche
ms.date: 01/06/2020
ms.openlocfilehash: e5ade99673fc31a9ae599a647aa041b54868d274
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75631332"
---
# <a name="syntax-and-expressions-in-azure-resource-manager-templates"></a>Azure 资源管理器模板中的语法和表达式

模板的基本语法为 JSON。 但是，可以使用表达式来扩展模板中可用的 JSON 值。  表达式分别以方括号 `[` 与 `]` 开头和结尾。 部署模板时会计算表达式的值。 表达式可以返回字符串、整数、布尔值、数组或对象。

模板表达式不能超过 24,576 个字符。

## <a name="use-functions"></a>使用函数

以下示例演示了参数默认值中的表达式：

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
```

在该表达式中，语法 `resourceGroup()` 调用资源管理器提供的、在模板中使用的某个函数。 在本例中，它是 [resourceGroup](template-functions-resource.md#resourcegroup) 函数。 如同在 JavaScript 中一样，函数调用的格式为 `functionName(arg1,arg2,arg3)`。 语法 `.location` 从该函数返回的对象中检索一个属性。

模板函数及其参数不区分大小写。 例如，资源管理器将 **variables('var1')** 和 **VARIABLES('VAR1')** 解析为相同内容。 在求值时，除非函数明确修改大小写（例如，使用 toUpper 或 toLower 进行修改），否则函数保留大小写。 某些资源类型可能有独立于函数求值方式的大小写要求。

若要将字符串值作为参数传递给函数，请使用单引号。

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]"
```

## <a name="escape-characters"></a>转义字符

要使文本字符串以左方括号 `[` 开头，以右方括号 `]` 结尾，但不将其解释为表达式，请添加额外的方括号使字符串以 `[[` 开头。 例如，变量：

```json
"demoVar1": "[[test value]"
```

解析为 `[test value]`。

但是，如果文本字符串没有以方括号结束，则不要转义第一个方括号。 例如，变量：

```json
"demoVar2": "[test] value"
```

解析为 `[test] value`。

若要转义表达式中的双引号（例如，在模板中添加 JSON 对象），请使用反斜杠。

```json
"tags": {
    "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
},
```

## <a name="next-steps"></a>后续步骤

* 有关模板函数的完整列表，请参阅 [Azure Resource Manager 模板函数](template-functions.md)。
* 有关模板文件的详细信息，请参阅[了解 Azure 资源管理器模板的结构和语法](template-syntax.md)。

<!-- Update_Description: update meta properties, wording update, update link -->