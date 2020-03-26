---
title: 错误和异常处理 - Azure 逻辑应用 | Microsoft Docs
description: 了解 Azure 逻辑应用中的错误和异常处理模式
services: logic-apps
ms.service: logic-apps
author: dereklee
ms.author: v-yiso
manager: jeconnoc
origin.date: 01/11/2020
ms.date: 02/24/2020
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 37b4845a4d12d7a9f16faf3d3169856850a770f0
ms.sourcegitcommit: 305361c96d1d5288d3dda7e81833820640e2afac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2020
ms.locfileid: "80109753"
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>在 Azure 逻辑应用中处理错误和异常

恰当地处理依赖系统造成的停机或问题对任何集成体系结构而言可能都是一个难题。 为了帮助创建可适当处理问题和故障的可靠、可复原集成，逻辑应用提供了用于处理错误和异常的一流体验。 

<a name="retry-policies"></a>

## <a name="retry-policies"></a>重试策略

对于最基本的异常和错误处理，可在支持的任何操作或触发器中使用重试策略  。 重试策略指定当原始请求（导致 408、429 或 5xx 响应的任何请求）超时或失败时，操作或触发器是否且如何重试请求。 如果未使用任何其他重试策略，则使用默认策略。 

重试策略类型如下所示： 

| 类型 | 说明 | 
|------|-------------| 
| **默认** | 此策略可*按指数级增长*间隔发送最多 4 次重试，增幅为 7.5 秒，但范围限定在 5 到 45 秒之间。 | 
| **指数间隔**  | 此策略会等待从指数增长的范围中随机选定的时间间隔，然后再发送下一个请求。 | 
| **固定间隔**  | 此策略会等待指定的时间间隔，然后再发送下一个请求。 | 
| **无**  | 不重新发送请求。 | 
||| 

要了解重试策略限制，请参阅[逻辑应用限制和配置](../logic-apps/logic-apps-limits-and-config.md#request-limits)。 

### <a name="change-retry-policy"></a>更改重试策略

要选择其他重试策略，请执行以下步骤： 

1. 在逻辑应用设计器中打开逻辑应用。 

2. 打开操作或触发器的“设置”  。

3. 如果操作或触发器支持重试策略，请在“重试策略”下选择所需类型  。 

或者，可以在支持重试策略的操作或触发器的 `inputs` 部分指定重试策略。 如果不指定重试策略，该操作将使用默认策略。

```json
"<action-name>": {
   "type": "<action-type>", 
   "inputs": {
      "<action-specific-inputs>",
      "retryPolicy": {
         "type": "<retry-policy-type>",
         "interval": "<retry-interval>",
         "count": <retry-attempts>,
         "minimumInterval": "<minimum-interval>",
         "maximumInterval": "<maximum-interval>"
      },
      "<other-action-specific-inputs>"
   },
   "runAfter": {}
}
```

*必需*

| Value | 类型 | 说明 |
|-------|------|-------------|
| <*retry-policy-type*> | String | 要使用的重试策略类型：`default`、`none`、`fixed` 或 `exponential` | 
| <*retry-interval*> | String | 其中值必须使用 [ISO 8601 格式](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)的重试时间间隔。 默认的最小时间间隔是 `PT5S`，而最大时间间隔是 `PT1D`。 如果使用指数式时间间隔策略，可更改最小值和最大值。 | 
| <*retry-attempts*> | Integer | 重试尝试次数，它必须介于 1 和 90 之间 | 
||||

*可选*

| Value | 类型 | 说明 |
|-------|------|-------------|
| <*minimum-interval*> | String | 对于指数式时间间隔策略，是指随机选定的时间间隔的最小时间间隔（采用 [ISO 8601 格式](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)） | 
| <*maximum-interval*> | String | 对于指数式时间间隔策略，是指随机选定的时间间隔的最大时间间隔（采用 [ISO 8601 格式](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)） | 
|||| 

下面详细介绍不同的策略类型。

<a name="default-retry"></a>

### <a name="default"></a>默认

如果不指定重试策略，则操作将使用默认策略，它实际上是一个[指数式时间间隔策略](#exponential-interval)，按指数级增长的时间间隔（以 7.5 秒为增幅）最多发送 4 次重试。 时间间隔的范围为 5 到 45 秒。 

尽管未在操作或触发器中显式定义，但默认策略在示例 HTTP 操作中的行为方式如下所示：

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "http://myAPIendpoint/api/action",
      "retryPolicy" : {
         "type": "exponential",
         "interval": "PT7S",
         "count": 4,
         "minimumInterval": "PT5S",
         "maximumInterval": "PT1H"
      }
   },
   "runAfter": {}
}
```


<a name="no-retry"></a>
### <a name="none"></a>无

要指定操作或触发器不重试失败的请求，请将 <retry-policy-type> 设置为 `none`  。

<a name="fixed-retry"></a>
### <a name="fixed-interval"></a>固定间隔

要指定操作或触发器在等待指定的时间间隔后再发送下一个请求，请将 <retry-policy-type> 设置为 `fixed`  。

*示例*

该重试策略在首次请求失败后再尝试获取最新资讯两次，每次尝试之间延迟 30 秒：

```json
"Get_latest_news": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "https://mynews.example.com/latest",
      "retryPolicy": {
         "type": "fixed",
         "interval": "PT30S",
         "count": 2
      }
   }
}
```

<a name="exponential-interval"></a>

### <a name="exponential-interval"></a>指数间隔

要指定操作或触发器在等待随机的时间间隔后再发送下一个请求，请将 <retry-policy-type> 设置为 `exponential`  。 随机时间间隔选自呈指数增长的范围。 此外，可通过自行指定最小时间间隔和最大时间间隔替代默认的最小和最大时间间隔。

**随机变量范围**

下表展示了逻辑应用如何为每次重试生成指定范围内的一个统一随机变量（不超过重试次数）：

| 重试次数 | 最小间隔 | 最大间隔 |
|--------------|------------------|------------------|
| 1 | max(0, <minimum-interval>)  | min(interval, <maximum-interval>)  |
| 2 | max(interval, <minimum-interval>)  | min(2 * interval, <maximum-interval>)  |
| 3 | max(2 * interval, <minimum-interval>)  | min(4 * interval, <maximum-interval>)  |
| 4 | max(4 * interval, <minimum-interval>)  | min(8 * interval, <maximum-interval>)  |
| .... | .... | .... | 
|||| 

<a name="control-run-after-behavior"></a>

## <a name="catch-and-handle-failures-by-changing-run-after-behavior"></a>通过更改“随后运行”行为来捕获和处理失败

在逻辑应用设计器中添加操作时，需隐式声明用于运行这些操作的顺序。 某个操作完成运行后，该操作将标记为 `Succeeded`、`Failed`、`Skipped` 或 `TimedOut` 等状态。 在每个操作定义中，`runAfter` 属性指定必须先完成的前置操作，以及在后继操作能够运行之前，该前置操作允许的状态。 默认情况下，在设计器中添加的操作只会在前置操作已完成并且状态为 `Succeeded` 之后才运行。

当某个操作引发了未经处理的错误或异常时，该操作将标记为 `Failed`，而任何后继操作将标记为 `Skipped`。 如果具有并行分支的操作发生此行为，逻辑应用引擎将跟踪其他分支来确定其完成状态。 例如，如果某个分支以 `Skipped` 操作结束，该分支的完成状态基于该已跳过操作的前置操作的状态。 逻辑应用运行完成后，引擎将通过评估所有分支的状态来确定整个运行的状态。 如果任一分支失败，整个逻辑应用运行将标记为 `Failed`。

![演示如何评估运行状态的示例](./media/logic-apps-exception-handling/status-evaluation-for-parallel-branches.png)

为了确保无论前置操作状态如何，某个操作都可运行，请[自定义操作的“随后运行”行为](#customize-run-after)，以处理前置操作的不成功状态。

<a name="customize-run-after"></a>

### <a name="customize-run-after-behavior"></a>自定义“随后运行”行为

可以自定义某个操作的“随后运行”行为，使该操作在前置操作的状态为 `Succeeded`、`Failed`、`Skipped` 或 `TimedOut` 时均可运行。 例如，若要在 Excel Online `Add_a_row_into_a_table` 前置操作标记为 `Failed`（而不是 `Succeeded`）后发送电子邮件，请遵循以下任一步骤更改“随后运行”行为：

* 在设计视图中，选择省略号 ( **...** ) 按钮，然后选择“配置随后运行”。 

  ![为操作配置“随后运行”行为](./media/logic-apps-exception-handling/configure-run-after-property-setting.png)

  操作形状将显示前置操作所需的默认状态，在本示例中为“在表中插入新行”： 

  ![操作的默认“随后运行”行为](./media/logic-apps-exception-handling/change-run-after-property-status.png)

  将“随后运行”行为更改为所需状态，在本示例中为“失败”： 

  ![将“随后运行”行为更改为“失败”](./media/logic-apps-exception-handling/run-after-property-status-set-to-failed.png)

  若要指定无论前置操作是标记为 `Failed`、`Skipped` 还是 `TimedOut`，该操作都会运行，请选择其他状态：

  ![将“随后运行”行为更改为出现任何其他状态](./media/logic-apps-exception-handling/run-after-property-multiple-statuses.png)

* 在代码视图中的操作 JSON 定义内，遵循以下语法编辑 `runAfter` 属性：

  ```json
  "<action-name>": {
     "inputs": {
        "<action-specific-inputs>"
     },
     "runAfter": {
        "<preceding-action>": [
           "Succeeded"
        ]
     },
     "type": "<action-type>"
  }
  ```

  对于本示例，请将 `runAfter` 属性从 `Succeeded` 更改为 `Failed`：

  ```json
  "Send_an_email_(V2)": {
     "inputs": {
        "body": {
           "Body": "<p>Failed to&nbsp;add row to &nbsp;@{body('Add_a_row_into_a_table')?['Terms']}</p>",,
           "Subject": "Add row to table failed: @{body('Add_a_row_into_a_table')?['Terms']}",
           "To": "Sophia.Owen@fabrikam.com"
        },
        "host": {
           "connection": {
              "name": "@parameters('$connections')['office365']['connectionId']"
           }
        },
        "method": "post",
        "path": "/v2/Mail"
     },
     "runAfter": {
        "Add_a_row_into_a_table": [
           "Failed"
        ]
     },
     "type": "ApiConnection"
  }
  ```

  若要指定无论前置操作是标记为 `Failed`、`Skipped` 还是 `TimedOut`，该操作都会运行，请添加其他状态：

  ```json
  "runAfter": {
     "Add_a_row_into_a_table": [
        "Failed", "Skipped", "TimedOut"
     ]
  },
  ```

<a name="scopes"></a>

## <a name="evaluate-actions-with-scopes-and-their-results"></a>评估具有作用域的操作及其结果

类似于使用 `runAfter` 属性在单个操作之后运行步骤，可以将操作在某个范围内分组在一起。 如果希望以逻辑方式将各个操作组合在一起，可以使用作用域，评估作用域的聚合状态，并基于该状态执行操作。 当某个作用域中的所有操作都完成运行后，该作用域本身也确定了其自己的状态。

<!--Not Avaialble on [scope](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)-->

若要检查范围的状态，可以使用与用来检查逻辑应用运行状态（例如 `Succeeded`、`Failed` 等等）的条件相同的条件。

默认情况下，当范围的所有操作都成功时，范围的状态将标记为 `Succeeded`。 如果范围内最后一个操作的状态为 `Failed` 或 `Aborted`，则范围的状态将标记为 `Failed`。

若要捕获 `Failed` 范围内的异常并运行用来处理这些错误的操作，可对该 `Failed` 范围使用 `runAfter` 属性。 这样，如果范围内的任何操作失败并且为该范围使用了 `runAfter` 属性，则可以创建单个操作来捕获失败。 

有关作用域的限制，请参阅[限制和配置](../logic-apps/logic-apps-limits-and-config.md)。

<a name="get-results-from-failures"></a>

### <a name="get-context-and-results-for-failures"></a>获取失败的上下文和结果

尽管从作用域中捕获失败非常有用，但可能还需要借助上下文来确切了解失败的操作以及返回的任何错误或状态代码。

[`result()`](../logic-apps/workflow-definition-language-functions-reference.md#result) 函数提供了有关作用域中所有操作的结果的上下文。 `result()` 函数接受单个参数（即作用域的名称），并返回一个数组，其中包含该作用域中的所有操作结果。 这些操作对象包括与 `actions()` 对象相同的属性，例如操作的开始时间、结束时间、状态、输入、相关 ID 和输出。 若要发送作用域内失败的任何操作的上下文，可以轻松将 `@result()` 表达式与 `runAfter` 属性配对。

若要为范围中具有 `Failed` 结果的每个操作运行操作，并将结果数组筛选到失败的操作，可将 `@result()` 表达式与[**筛选器数组**](logic-apps-perform-data-operations.md#filter-array-action)操作和 [**For each**](../logic-apps/logic-apps-control-flow-loops.md) 循环配对。 可以采用筛选的结果数组并使用 `For_each` 循环对每个失败执行操作。

以下示例（后附详细说明）发送一个 HTTP POST 请求，其中包含范围内“My_Scope”中失败的任何操作的响应正文：

```json
"Filter_array": {
   "type": "Query",
   "inputs": {
      "from": "@result('My_Scope')",
      "where": "@equals(item()['status'], 'Failed')"
   },
   "runAfter": {
      "My_Scope": [
         "Failed"
      ]
    }
},
"For_each": {
   "type": "foreach",
   "actions": {
      "Log_exception": {
         "type": "Http",
         "inputs": {
            "method": "POST",
            "body": "@item()['outputs']['body']",
            "headers": {
               "x-failed-action-name": "@item()['name']",
               "x-failed-tracking-id": "@item()['clientTrackingId']"
            },
            "uri": "http://requestb.in/"
         },
         "runAfter": {}
      }
   },
   "foreach": "@body('Filter_array')",
   "runAfter": {
      "Filter_array": [
         "Succeeded"
      ]
   }
}
```

下面是描述此示例中发生的情况的详细演练：

1. 要获取“My_Scope”中所有操作的结果，**筛选数组**操作将使用筛选表达式：`@result('My_Scope')`

2. **筛选数组**的条件是状态等于“Failed”的任何 `@result()` 项  。 此条件将具有“My_Scope”中所有操作结果的数组筛选为仅包含失败操作结果的数组。

3. 对“筛选后的数组”输出执行 For Each 循环操作   。 此步骤针对前面筛选的每个失败操作结果执行操作。

   如果范围中的单个操作失败，For Each 循环中的操作仅运行一次  。 
   如果存在多个失败的操作，则将对每个失败执行一次操作。

4. 针对 For Each 项响应正文（即 `@item()['outputs']['body']` 表达式）发送 HTTP POST  。 

   `@result()` 项的形状与 `@actions()` 形状相同，可按相同的方式进行分析。

5. 包括两个自定义标头，其中包含失败操作的名称 (`@item()['name']`) 和失败的运行客户端跟踪 ID (`@item()['clientTrackingId']`)。

下面提供了一个 `@result()` 项的示例供参考，其中显示了在上一示例中分析过的 name、body 和 clientTrackingId 属性    。 在 **For Each** 操作外部，`@result()` 会返回这些对象的数组。

```json
{
   "name": "Example_Action_That_Failed",
   "inputs": {
      "uri": "https://myfailedaction.azurewebsites.net",
      "method": "POST"
   },
   "outputs": {
      "statusCode": 404,
      "headers": {
         "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
         "Server": "Microsoft-IIS/8.0",
         "X-Powered-By": "ASP.NET",
         "Content-Length": "68",
         "Content-Type": "application/json"
      },
      "body": {
         "code": "ResourceNotFound",
         "message": "/docs/folder-name/resource-name does not exist"
      }
   },
   "startTime": "2016-08-11T03:18:19.7755341Z",
   "endTime": "2016-08-11T03:18:20.2598835Z",
   "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
   "clientTrackingId": "08587307213861835591296330354",
   "code": "NotFound",
   "status": "Failed"
}
```

若要执行不同的异常处理模式，可以使用本文中前面所述的表达式。 可选择在范围外执行单个异常处理操作，使此操作接受筛选后的整个失败数组，然后再删除 For Each 操作  。 如前面所述，还可以包含 **\@result()** 响应中其他有用的属性。

## <a name="azure-diagnostics-and-metrics"></a>Azure 诊断和指标

上述模式非常适合于处理运行中的错误和异常，不过也可以独立于运行本身来标识和响应错误。 
[Azure 诊断](../logic-apps/logic-apps-monitor-your-logic-apps.md)提供了一种简单方式，用来将所有工作流事件（包括所有运行和操作状态）发送到 Azure 存储帐户或通过 Azure 事件中心创建的事件中心。 

要评估运行状态，可以监视日志和指标，或将它们发布到你习惯使用的任何监视工具中。 一种潜在选项是通过事件中心将所有事件流式传输到 [Azure 流分析](https://azure.microsoft.com/services/stream-analytics/)。 在流分析中，可以根据诊断日志中的任何异常、平均值或失败编写实时查询。 可以使用流分析将信息发送到其他数据源，例如队列、主题、SQL、Azure Cosmos DB 或 Power BI。

## <a name="next-steps"></a>后续步骤

* [了解如何使用 Azure 逻辑应用构建错误处理逻辑](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [查找更多逻辑应用示例和方案](../logic-apps/logic-apps-examples-and-scenarios.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
