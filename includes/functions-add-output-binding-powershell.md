---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/02/2020
ms.author: v-junlch
ms.openlocfilehash: fcad822a3de5eaff83840741b5c2891b385f0e5e
ms.sourcegitcommit: 1ac138a9e7dc7834b5c0b62a133ca5ce2ea80054
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2020
ms.locfileid: "78266063"
---
添加使用 `Push-OutputBinding` cmdlet 通过 `msg` 输出绑定将文本写入队列的代码。 在 `if` 语句中设置“正常”状态之前，请添加此代码。

```powershell
$outputMsg = $name
Push-OutputBinding -name msg -Value $outputMsg
```

此时，你的函数应如下所示：

```powershell
using namespace System.Net

# Input bindings are passed in via param block.
param($Request, $TriggerMetadata)

# Write to the Azure Functions log stream.
Write-Host "PowerShell HTTP trigger function processed a request."

# Interact with query parameters or the body of the request.
$name = $Request.Query.Name
if (-not $name) {
    $name = $Request.Body.Name
}

if ($name) {
    # Write the $name value to the queue, 
    # which is the name passed to the function.
    $outputMsg = $name
    Push-OutputBinding -name msg -Value $outputMsg

    $status = [HttpStatusCode]::OK
    $body = "Hello $name"
}
else {
    $status = [HttpStatusCode]::BadRequest
    $body = "Please pass a name on the query string or in the request body."
}

# Associate values to output bindings by calling 'Push-OutputBinding'.
Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = $status
    Body = $body
})
```
