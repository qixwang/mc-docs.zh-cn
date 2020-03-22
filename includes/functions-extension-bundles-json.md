---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/19/2020
ms.author: v-junlch
ms.openlocfilehash: 561c123940645f548cdd8d58414140646664eca0
ms.sourcegitcommit: e500354e2fd8b7ac3dddfae0c825cc543080f476
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2020
ms.locfileid: "79549591"
---
```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

`extensionBundle` 中提供了以下属性：

| 属性 | 说明 |
| -------- | ----------- |
| id | Azure Functions 扩展捆绑的命名空间。 |
| 版本 | 要安装的捆绑包的版本。 Functions 运行时始终选取按版本范围或间隔定义的最宽松版本。 上述版本值允许使用从 1.0.0 到 2.0.0（不含 2.0.0）的所有捆绑版本。 有关详细信息，请参阅[用于指定版本范围的间隔表示法](https://docs.microsoft.com/nuget/reference/package-versioning#version-ranges)。 |

随着捆绑包中的包的变化，捆绑包版本递增。 当捆绑中的包增加主版本时，就会发生主版本更改。 捆绑中的主版本更改通常与 Functions 运行时的主版本更改一致。  

默认捆绑安装的当前扩展集将在此 [extensions.json 文件](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json)中枚举。

