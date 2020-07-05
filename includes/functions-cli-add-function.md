---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/03/2020
ms.author: v-junlch
ms.openlocfilehash: 14cf11360e0059c0a040fb1a7a3aa900ffe0edab
ms.sourcegitcommit: f1a76ee3242698123a3d77f44c860db040b48f70
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "85945269"
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

