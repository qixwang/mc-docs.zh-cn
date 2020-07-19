---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/17/2020
ms.author: v-junlch
ms.openlocfilehash: e72a8a5d7ecab75e8978ca96af2ed8908a31ff9a
ms.sourcegitcommit: 403db9004b6e9390f7fd1afddd9e164e5d9cce6a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/17/2020
ms.locfileid: "86441276"
---
::: zone pivot="programming-language-csharp,programming-language-javascript,programming-language-typescript,programming-language-powershell" 
使用以下命令将一个函数添加到项目，其中，`--name` 参数是该函数 (HttpExample) 的唯一名称，`--template` 参数指定该函数的触发器 (HTTP)。 

```
func new --name HttpExample --template "HTTP trigger"
```  
::: zone-end  
::: zone pivot="programming-language-csharp"
`func new` 创建 HttpExample.cs 代码文件。
::: zone-end
::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-powershell"
`func new` 创建一个与函数名称匹配的、包含项目所选语言适用的代码文件的子文件夹，以及一个名为 *function.json* 的配置文件。
::: zone-end

