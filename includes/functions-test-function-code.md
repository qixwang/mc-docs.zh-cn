---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/13/2020
ms.author: v-junlch
ms.openlocfilehash: aa2670e634b3b169429dabd681e0a29c5f19642a
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77430030"
---
## <a name="test"></a>在 Azure 中验证函数

可以使用 Web 浏览器来验证已部署的函数。  将 URL（包括功能键）复制到 Web 浏览器的地址栏中。 在执行请求之前，先将查询字符串 `&name=<yourname>` 追加到该 URL。

![使用 Web 浏览器调用函数。](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  

或者，可以使用 cURL 来验证已部署的函数。 使用从上一步复制的 URL（包括功能键），将查询字符串 `&name=<yourname>` 追加到该 URL。

![使用 cURL 在 Azure 中调用函数。](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 


