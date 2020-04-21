---
title: include 文件
description: include 文件
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 12/31/2019
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 33283f3515f03b66ac722bcea8b2cd79ae89a0bd
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75624109"
---
特定函数应用中所有函数的代码均位于根项目文件夹中，其中包含主机配置文件和一个或多个子文件夹。 每个子文件夹包含单独函数的代码。 文件夹结构如下图所示：

```
FunctionApp
 | - host.json
 | - MyFirstFunction
 | | - function.json
 | | - ...  
 | - MySecondFunction
 | | - function.json
 | | - ...  
 | - SharedCode
 | - bin
```

在 2.x 版及更高版本的 Functions 运行时中，函数应用中的所有函数必须共享同一语言堆栈。  

[host.json](../articles/azure-functions/functions-host-json.md) 文件包含特定于运行时的配置，并位于函数应用的根文件夹中。 *bin* 文件夹包含函数应用所需的包和其他库文件。 查看函数应用项目的语言特定要求：

* [C# 类库 (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [C# 脚本 (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [F# 脚本](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)

