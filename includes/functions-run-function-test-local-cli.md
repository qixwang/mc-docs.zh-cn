---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/05/2020
ms.author: v-junlch
ms.openlocfilehash: e0e7ceaac3209a62350d88134aed8d15fe64a527
ms.sourcegitcommit: f1a76ee3242698123a3d77f44c860db040b48f70
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84563717"
---
## <a name="run-the-function-locally"></a>在本地运行函数

通过从 LocalFunctionProj  文件夹启动本地 Azure Functions 运行时主机来运行函数：

::: zone pivot="programming-language-csharp,programming-language-powershell,programming-language-javascript"
```
func start
```
::: zone-end

::: zone pivot="programming-language-typescript"
```
npm install
npm start
```
::: zone-end

::: zone pivot="programming-language-java"
```
mvn clean package 
mvn azure-functions:run
```
::: zone-end

在输出的末尾，应显示以下行： 

<pre>
...

Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

        HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
...

</pre>

>[!NOTE]  
> 如果 HttpExample 未按下面所示出现，则可能是在项目的根文件夹外启动了主机。 在这种情况下，请按 **Ctrl**+**C** 停止主机，导航到项目的根文件夹，然后重新运行上一命令。

将此输出中 `HttpExample` 函数的 URL 复制到浏览器，并追加查询字符串 `?name=<your-name>`，使完整 URL 类似于 `http://localhost:7071/api/HttpExample?name=Functions`。 浏览器应会显示类似于 `Hello Functions` 的消息：

![在浏览器中本地运行函数后的结果](./media/functions-run-function-test-local-cli/function-test-local-browser.png)

当你发出请求时，启动项目时所在的终端还会显示日志输出。

准备就绪后，按 **Ctrl**+**C** 并选择 `y` 停止函数主机。

