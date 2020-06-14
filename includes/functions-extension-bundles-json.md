---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/05/2020
ms.author: v-junlch
ms.openlocfilehash: 473547304e2f65767caebab2f7028811befdd2be
ms.sourcegitcommit: f1a76ee3242698123a3d77f44c860db040b48f70
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84563746"
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

| properties | 说明 |
| -------- | ----------- |
| id | Azure Functions 扩展捆绑的命名空间。 |
| 版本 | 要安装的捆绑包的版本。 Functions 运行时始终选取由版本范围或间隔定义的可允许最高版本。 上述版本值允许从 1.0.0 到（但不包括）2.0.0 的所有捆绑包版本。 有关详细信息，请参阅[用于指定版本范围的间隔表示法](https://docs.microsoft.com/nuget/reference/package-versioning#version-ranges)。 |

随着捆绑包中的包的变化，捆绑包版本递增。 当捆绑包中的包的主版本递增时，会发生主版本更改。 捆绑包的主版本更改通常与 Functions 运行时主版本的更改同时发生。  

默认捆绑包安装的当前扩展集在此 [extensions.json 文件](https://github.com/Azure/azure-functions-extension-bundles/blob/dev/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json)中进行枚举。

