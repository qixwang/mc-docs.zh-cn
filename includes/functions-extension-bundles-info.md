---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/02/2020
ms.author: v-junlch
ms.openlocfilehash: 16d7a84601f34e9da459cc1c8f3211d453b129d7
ms.sourcegitcommit: 1ac138a9e7dc7834b5c0b62a133ca5ce2ea80054
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2020
ms.locfileid: "78266138"
---
::: zone pivot="programming-language-javascript,programming-language-powershell,programming-language-typescript"  
> [!TIP]
> 在启动过程中，主机会下载并安装[存储绑定扩展](../articles/azure-functions/functions-bindings-storage-queue.md#functions-2x-and-higher)和其他 Microsoft 绑定扩展。 之所以安装这些扩展，是因为默认情况下，已在 *host.json* 文件中使用以下属性启用了绑定扩展：
>
> ```json
> {
>     "version": "2.0",
>     "extensionBundle": {
>         "id": "Microsoft.Azure.Functions.ExtensionBundle",
>         "version": "[1.*, 2.0.0)"
>     }
> }
> ```
>
> 如果遇到任何与绑定扩展相关的错误，请检查上述属性是否在 *host.json* 中存在。
::: zone-end  

