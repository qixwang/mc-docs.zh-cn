---
title: 在 Azure 中创建用于响应 HTTP 请求的函数
description: 了解如何通过命令行创建函数，然后将本地项目发布到 Azure Functions 中托管的无服务器实例。
ms.date: 03/31/2020
ms.topic: quickstart
ms.openlocfilehash: c1404c18390ee59f8d99e7648ef7b1cbb5a3c123
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80581773"
---
# <a name="quickstart-create-a-function-in-azure-that-responds-to-http-requests"></a>快速入门：在 Azure 中创建用于响应 HTTP 请求的函数

在本文中，你将使用命令行工具创建响应 HTTP 请求的函数。 在本地测试代码后，将代码部署到 Azure Functions 的无服务器环境。 完成本快速入门会从你的 Azure 帐户中扣取最多几美分的费用。

本文还提供了[基于 Visual Studio 代码的版本](functions-create-first-function-vs-code.md)。

## <a name="configure-your-local-environment"></a>配置本地环境

在开始之前，必须满足以下条件：

+ 具有活动订阅的 Azure 帐户。 [创建帐户](https://www.azure.cn/pricing/1rmb-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。

::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell"  
+ [Azure Functions Core Tools](./functions-run-local.md#v2) 2.7.1846 或更高的 2.x 版本。
::: zone-end  

+ [Azure CLI](/cli/install-azure-cli) 2.0.76 或更高版本。 
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ [Node.js](https://nodejs.org/)，活动 LTS 和维护 LTS 版本（建议使用 8.11.1 和 10.14.1）。
::: zone-end

::: zone pivot="programming-language-powershell"
+ [PowerShell Core](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-windows)

+ [.NET Core SDK 2.2+](https://www.microsoft.com/net/download)
::: zone-end

### <a name="check-your-environment"></a>检查环境

+ 在终端或命令窗口中，运行 `func --version` 检查 Azure Functions Core Tools 的版本是否为 2.7.1846 或更高的 2.x 版本。

+ 运行 `az --version` 检查 Azure CLI 版本是否为 2.0.76 或以上。

+ 运行 `az login` 登录到 Azure 并验证活动订阅。

::: zone pivot="programming-language-javascript,programming-language-typescript"
+ 运行 `node --version` 检查报告的 Node.js 版本是否为 8.x 或 10.x。
::: zone-end

## <a name="create-a-local-function-project"></a>创建本地函数项目

在 Azure Functions 中，有一个函数项目是一个或多个单独函数（每个函数响应特定的触发器）的容器。 项目中的所有函数共享相同的本地和宿主配置。 在本部分，你将创建包含单个函数的函数项目。

1. 在虚拟环境中运行 `func init` 命令，以在某个文件夹中创建名为 *LocalFunctionProj* 的包含指定运行时的函数项目：

    ::: zone pivot="programming-language-csharp"
    ```
    func init LocalFunctionProj --dotnet
    ```
    ::: zone-end
    ::: zone pivot="programming-language-javascript"
    ```
    func init LocalFunctionProj --javascript
    ```
    ::: zone-end
    ::: zone pivot="programming-language-typescript"
    ```
    func init LocalFunctionProj --typescript
    ```
    ::: zone-end
    ::: zone pivot="programming-language-powershell"
    ```
    func init LocalFunctionProj --powershell
    ```
    ::: zone-end


    此文件夹包含项目的各个文件，其中包括名为 [local.settings.json](functions-run-local.md#local-settings-file) 和 [host.json](functions-host-json.md) 的配置文件。 由于 *local.settings.json* 可以包含从 Azure 下载的机密，因此，默认情况下，该文件会从 *.gitignore* 文件的源代码管理中排除。

1. 导航到项目文件夹：

    ```
    cd LocalFunctionProj
    ```
    
1. 使用以下命令将一个函数添加到项目，其中，`--name` 参数是该函数的唯一名称，`--template` 参数指定该函数的触发器。 

    ```
    func new --name HttpExample --template "HTTP trigger"
    ```

    ::: zone pivot="programming-language-csharp"
    `func new` 创建 HttpExample.cs 代码文件。
    ::: zone-end
    ::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-powershell"
    `func new` 创建一个与函数名称匹配的、包含项目所选语言适用的代码文件的子文件夹，以及一个名为 *function.json* 的配置文件。
    ::: zone-end

### <a name="optional-examine-the-file-contents"></a>（可选）检查文件内容

如果需要，可以跳到[在本地运行函数](#run-the-function-locally)并稍后再检查文件内容。

::: zone pivot="programming-language-csharp"
#### <a name="httpexamplecs"></a>HttpExample.cs

*HttpExample.cs* 包含一个接收 `req` 变量中的请求数据的 `Run` 方法，该变量是使用 HttpTriggerAttribute  **修饰的 [HttpRequest](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.httprequest)** ，用于定义触发器行为。 

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;

namespace LocalFunctionProj
{
    public static class HttpExample
    {
        [FunctionName("HttpExample")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            string name = req.Query["name"];

            string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
            dynamic data = JsonConvert.DeserializeObject(requestBody);
            name = name ?? data?.name;

            return name != null
                ? (ActionResult)new OkObjectResult($"Hello, {name}")
                : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
        }
    }
}
```

返回对象是 [ActionResult](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.actionresult)，它将响应消息作为 [OkObjectResult](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200) 或 [BadRequestObjectResult](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.badrequestobjectresult) (400) 返回。 要了解详细信息，请参阅 [Azure Functions HTTP 触发器和绑定](/azure-functions/functions-bindings-http-webhook?tabs=csharp)。
::: zone-end

::: zone pivot="programming-language-typescript"
#### <a name="indexts"></a>index.ts

*index.ts* 导出一个根据 *function.json* 中的配置触发的函数。

```typescript
import { AzureFunction, Context, HttpRequest } from "@azure/functions"

const httpTrigger: AzureFunction = async function (context: Context, req: HttpRequest): Promise<void> {
    context.log('HTTP trigger function processed a request.');
    const name = (req.query.name || (req.body && req.body.name));
    const responseMessage = name
        ? "Hello, " + name + ". This HTTP triggered function executed successfully."
        : "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.";

    context.res = {
        // status: 200, /* Defaults to 200 */
        body: responseMessage
    };

};

export default httpTrigger;
```

对于 HTTP 触发器，该函数将接收 *function.json* 中定义的 **HttpRequest** 类型的变量 `req` 中的请求数据。 在 *function.json* 中定义为 `$return` 的返回对象是响应。 
::: zone-end

::: zone pivot="programming-language-powershell"
#### <a name="runps1"></a>run.ps1

*run.ps1* 定义一个根据 *function.json* 中的配置触发的函数脚本。

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

$body = "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response."

if ($name) {
    $body = "Hello, $name. This HTTP triggered function executed successfully."
}

# Associate values to output bindings by calling 'Push-OutputBinding'.
Push-OutputBinding -Name Response -Value ([HttpResponseContext]@{
    StatusCode = [HttpStatusCode]::OK
    Body = $body
})
```

对于 HTTP 触发器，该函数将接收传递给 *function.json* 中定义的 `$Request` 参数的请求数据。 在 *function.json* 中定义为 `Response` 的返回对象将作为响应传递给 `Push-OutputBinding` cmdlet。 
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-powershell"
#### <a name="functionjson"></a>function.json

*function.json* 是一个配置文件，其中定义了函数的输入和输出 `bindings`，包括触发器类型。 
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript"
```Json
{
   "bindings": [
       {
           "authLevel": "function",
           "type": "httpTrigger",
           "direction": "in",
           "name": "req",
           "methods": [
               "get",
               "post"
           ]
       },
       {
           "type": "http",
           "direction": "out",
           "name": "res"
       }
   ]
}
```
::: zone-end

::: zone pivot="programming-language-powershell"
```powershell
{
 "bindings": [
   {
     "authLevel": "function",
     "type": "httpTrigger",
     "direction": "in",
     "name": "Request",
     "methods": [
       "get",
       "post"
     ]
   },
   {
     "type": "http",
     "direction": "out",
     "name": "Response"
   }
 ]
}
```
::: zone-end

::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-powershell"  
每个绑定都需要一个方向、类型和唯一的名称。 HTTP 触发器具有类型为 [`httpTrigger`](functions-bindings-http-webhook-trigger.md) 的输入绑定和类型为 [`http`](functions-bindings-http-webhook-output.md) 的输出绑定。
::: zone-end  

[!INCLUDE [functions-run-function-test-local-cli](../../includes/functions-run-function-test-local-cli.md)]

## <a name="create-supporting-azure-resources-for-your-function"></a>创建函数的支持性 Azure 资源

在将函数代码部署到 Azure 之前，需要创建三个资源：

- 一个资源组：相关资源的逻辑容器。
- 一个存储帐户：维护有关项目的状态和其他信息。
- 一个函数应用：提供用于执行函数代码的环境。 函数应用映射到本地函数项目，可让你将函数分组为一个逻辑单元，以便更轻松地管理、部署和共享资源。

使用以下 Azure CLI 命令创建这些项。 完成后，每个命令将提供 JSON 输出。

1. 使用 [az login](/cli/reference-index#az-login) 命令登录到 Azure（如果尚未这样做）：

    ```azurecli
    az login
    ```
    
1. 使用 [az group create](/cli/group#az-group-create) 命令创建资源组。 以下示例在 `chinanorth` 区域中创建名为 `AzureFunctionsQuickstart-rg` 的资源组。 （通常，你会在 `az account list-locations` 命令输出的、与你靠近的某个可用区域中创建资源组和资源。）

    ```azurecli
    az group create --name AzureFunctionsQuickstart-rg --location chinanorth
    ```
    
1. 使用 [az storage account create](/cli/storage/account#az-storage-account-create) 命令在资源组和区域中创建常规用途存储帐户。 在以下示例中，请将 `<STORAGE_NAME>` 替换为适合自己的全局唯一名称。 名称只能包含 3 到 24 个数字和小写字母字符。 `Standard_LRS` 指定 [Functions 支持](storage-considerations.md#storage-account-requirements)的常规用途帐户。

    ```azurecli
    az storage account create --name <STORAGE_NAME> --location chinanorth --resource-group AzureFunctionsQuickstart-rg --sku Standard_LRS
    ```
    
    在本快速入门中使用的存储帐户只会产生几美分的费用。
    
1. 使用 [az functionapp create](/cli/functionapp#az-functionapp-create) 命令创建函数应用。 在以下示例中，请将 `<STORAGE_NAME>` 替换为在上一步骤中使用的帐户的名称，并将 `<APP_NAME>` 替换为适合自己的全局唯一名称。 `<APP_NAME>` 也是函数应用的默认 DNS 域。 

    ::: zone pivot="programming-language-javascript,programming-language-typescript"  
    如果使用 Node.js 8，请同时将 `--runtime-version` 更改为 `8`。

    
    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --consumption-plan-location chinanorth --runtime node --runtime-version 10 --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```
    ::: zone-end  

    ::: zone pivot="programming-language-csharp"  
    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --consumption-plan-location chinanorth --runtime dotnet --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```
    ::: zone-end  
    
    ::: zone pivot="programming-language-powershell"  
    ```azurecli
    az functionapp create --resource-group AzureFunctionsQuickstart-rg --consumption-plan-location chinanorth --runtime powershell --functions-version 2 --name <APP_NAME> --storage-account <STORAGE_NAME>
    ```
    ::: zone-end  

    此命令将创建一个函数应用，该应用在 [Azure Functions 消耗计划](functions-scale.md#consumption-plan)下指定的语言运行时中运行，根据本教程产生的用量，此操作是免费的。 
    
## <a name="deploy-the-function-project-to-azure"></a>将函数项目部署到 Azure

::: zone pivot="programming-language-typescript"  
使用 Core Tools 将项目部署到 Azure 之前，请从 TypeScript 源文件创建一个生产就绪的 JavaScript 文件版本。

以下命令可以准备要部署的 TypeScript 项目：

```
npm run build:production 
```
::: zone-end  

准备好所需的资源后，便可以使用 [func azure functionapp publish](functions-run-local.md#project-file-deployment) 命令将本地函数项目部署到 Azure 中的函数应用。 在以下示例中，请将 `<APP_NAME>` 替换为你的应用的名称。

```
func azure functionapp publish <APP_NAME>
```

如果看到错误“找不到名为...的应用”，请等待几秒后重试，因为在执行上一 `az functionapp create` 命令后，Azure 可能尚未完全初始化该应用。

publish 命令显示类似于以下输出的结果（为简洁起见，示例中的结果已截断）：

<pre>
...

Getting site publishing info...
Creating archive for current directory...
Performing remote build for functions project.

...

Deployment successful.
Remote build succeeded!
Syncing triggers...
Functions in msdocs-azurefunctions-qs:
    HttpExample - [httpTrigger]
        Invoke url: https://msdocs-azurefunctions-qs.chinacloudsites.cn/api/httpexample?code=KYHrydo4GFe9y0000000qRgRJ8NdLFKpkakGJQfC3izYVidzzDN4gQ==
</pre>

## <a name="invoke-the-function-on-azure"></a>在 Azure 上调用函数

由于函数使用 HTTP 触发器，因此，可以通过在浏览器中或使用 curl 等工具，向此函数的 URL 发出 HTTP 请求来调用它。 在这两种情况下，`code` URL 参数都是唯一的[函数密钥](functions-bindings-http-webhook-trigger.md#authorization-keys)，用于授权函数终结点的调用。

# <a name="browser"></a>[浏览器](#tab/browser)

将 publish 命令的输出中显示的完整“调用 URL”复制到浏览器的地址栏，并追加查询参数 `&name=Functions`。  浏览器显示的输出应与本地运行函数时显示的输出类似。

![在 Azure 上运行函数后浏览器中的输出](./media/functions-create-first-azure-function-azure-cli/function-test-cloud-browser.png)


# <a name="curl"></a>[curl](#tab/curl)

结合“调用 URL”运行 [`curl`](https://curl.haxx.se/)，并追加参数 `&name=Functions`。  该命令的输出应是文本“Hello Functions”。

![使用 curl 在 Azure 上运行函数后的输出](./media/functions-create-first-azure-function-azure-cli/function-test-cloud-curl.png)

## <a name="clean-up-resources"></a>清理资源

若要继续执行下一步骤[添加 Azure 存储队列输出绑定](functions-add-output-binding-storage-queue-cli.md)，请保留到目前为止构建的所有资源。

否则，请使用以下命令删除资源组及其包含的所有资源，以免产生额外的费用。

```azurecli
az group delete --name AzureFunctionsQuickstart-rg
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [连接到 Azure 存储队列](functions-add-output-binding-storage-queue-cli.md)

