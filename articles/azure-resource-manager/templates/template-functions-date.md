---
title: 模板函数 - 日期
description: 介绍了可在 Azure 资源管理器模板中用来处理日期的函数。
ms.topic: conceptual
origin.date: 04/06/2020
ms.date: 04/30/2020
ms.author: v-yeche
ms.openlocfilehash: 8e48bbf671a0547e35658d086ee7caa4d39008f8
ms.sourcegitcommit: b469d275694fb86bbe37a21227e24019043b9e88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82596480"
---
# <a name="date-functions-for-arm-templates"></a>ARM 模板的日期函数

资源管理器提供了以下可用于在 Azure 资源管理器 (ARM) 模板中处理日期的函数：

* [dateTimeAdd](#datetimeadd)
* [utcNow](#utcnow)

## <a name="datetimeadd"></a>dateTimeAdd

`dateTimeAdd(base, duration, [format])`

向基础的日期/时间值加上一个持续时间。

### <a name="parameters"></a>参数

| 参数 | 必需 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| base | 是 | string | 用于相加的起始日期/时间值。 使用 [ISO 8601 时间戳格式](https://en.wikipedia.org/wiki/ISO_8601)。 |
| duration | 是 | string | 要与 base 相加的时间值。 它可以是负值。 使用 [ISO 8601 持续时间格式](https://en.wikipedia.org/wiki/ISO_8601#Durations)。 |
| format | 否 | string | 日期时间结果的输出格式。 如果未提供，则将使用 base 值的格式。 使用[标准格式字符串](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings)或[自定义格式字符串](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings)。 |

### <a name="return-value"></a>返回值

将 duration 值与 base 值相加后得到的日期/时间值。

### <a name="examples"></a>示例

以下示例模板展示了增加时间值的各种方式。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters":{
        "baseTime":{
            "type":"string",
            "defaultValue": "[utcNow('u')]"
        }
    },
    "variables": {
        "add3Years": "[dateTimeAdd(parameters('baseTime'), 'P3Y')]",
        "subtract9Days": "[dateTimeAdd(parameters('baseTime'), '-P9D')]",
        "add1Hour": "[dateTimeAdd(parameters('baseTime'), 'PT1H')]"
    },
    "resources": [],
    "outputs": {
        "add3Years": {
            "value": "[variables('add3Years')]",
            "type": "string"
        },
        "subtract9Days": {
            "value": "[variables('subtract9Days')]",
            "type": "string"
        },
        "add1Hour": {
            "value": "[variables('add1Hour')]",
            "type": "string"
        },
    }
}
```

在 baseTime为 `2020-04-07 14:53:14Z` 的情况下部署上述模板时，输出为：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| add3Years | String | 4/7/2023 2:53:14 PM |
| subtract9Days | String | 3/29/2020 2:53:14 PM |
| add1Hour | String | 4/7/2020 3:53:14 PM |

下一示例模板展示了如何设置自动化计划的开始时间。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "omsAutomationAccountName": {
            "type": "string",
            "defaultValue": "demoAutomation",
            "metadata": {
                "description": "Use an existing Automation account."
            }
        },
        "scheduleName": {
            "type": "string",
            "defaultValue": "demoSchedule1",
            "metadata": {
                "description": "Name of the new schedule."
            }
        },
        "baseTime":{
            "type":"string",
            "defaultValue": "[utcNow('u')]",
            "metadata": {
                "description": "Schedule will start one hour from this time."
            }
        }
    },
    "variables": {
        "startTime": "[dateTimeAdd(parameters('baseTime'), 'PT1H')]"
    },
    "resources": [
        {
            "name": "[concat(parameters('omsAutomationAccountName'), '/', parameters('scheduleName'))]",
            "type": "microsoft.automation/automationAccounts/schedules",
            "apiVersion": "2015-10-31",
            "location": "chinaeast2",
            "tags": {
            },
            "properties": {
                "description": "Demo Scheduler",
                "startTime": "[variables('startTime')]",
                "isEnabled": "true",
                "interval": 1,
                "frequency": "hour"
            }
        }
    ],
    "outputs": {
    }
}
```

## <a name="utcnow"></a>utcNow

`utcNow(format)`

以指定的格式返回当前的 (UTC) 日期时间值。 如果未提供格式，则使用 ISO 8601 (yyyyMMddTHHmmssZ) 格式。 **此函数只能在参数的默认值中使用。**

### <a name="parameters"></a>参数

| 参数 | 必需 | 类型 | 说明 |
|:--- |:--- |:--- |:--- |
| format |否 |string |要转换为字符串的 URI 编码值。 使用[标准格式字符串](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings)或[自定义格式字符串](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings)。 |

### <a name="remarks"></a>备注

只能在表达式中对参数的默认值使用此函数。 在模板中的其他任何位置使用此函数都会返回错误。 不允许在模板的其他部分使用该函数，因为每次调用该函数，都会返回不同的值。 使用相同的参数部署同一模板不能可靠地生成相同的结果。

如果[使用相应的选项来重新部署以前已成功的部署](rollback-on-error.md)，而以前的部署包含一个使用 utcNow 的参数，则不会重新评估该参数， 而是在回滚部署中自动重复使用以前部署中的参数值。

重新部署依赖于 utcNow 函数提供默认值的模板时请保持谨慎。 如果重新部署且不提供参数的值，则会重新评估该函数。 若要更新现有的资源而不是新建资源，请传入以前部署中的参数值。

### <a name="return-value"></a>返回值

当前的 UTC 日期时间值。

### <a name="examples"></a>示例

以下示例模板演示日期时间值的不同格式。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcValue": {
            "type": "string",
            "defaultValue": "[utcNow()]"
        },
        "utcShortValue": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "utcCustomValue": {
            "type": "string",
            "defaultValue": "[utcNow('M d')]"
        }
    },
    "resources": [
    ],
    "outputs": {
        "utcOutput": {
            "type": "string",
            "value": "[parameters('utcValue')]"
        },
        "utcShortOutput": {
            "type": "string",
            "value": "[parameters('utcShortValue')]"
        },
        "utcCustomOutput": {
            "type": "string",
            "value": "[parameters('utcCustomValue')]"
        }
    }
}
```

上述示例的输出根据每个部署的不同而异，但类似于：

| 名称 | 类型 | 值 |
| ---- | ---- | ----- |
| utcOutput | string | 20190305T175318Z |
| utcShortOutput | string | 03/05/2019 |
| utcCustomOutput | string | 3 5 |

以下示例演示在设置标记值时如何使用函数中的值。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcShort": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "rgName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "name": "[parameters('rgName')]",
            "location": "chinanorth",
            "tags":{
                "createdDate": "[parameters('utcShort')]"
            },
            "properties":{}
        }
    ],
    "outputs": {
        "utcShort": {
            "type": "string",
            "value": "[parameters('utcShort')]"
        }
    }
}
```

<!-- Update_Description: new article about template functions date -->
<!--NEW.date: 04/30/2020-->