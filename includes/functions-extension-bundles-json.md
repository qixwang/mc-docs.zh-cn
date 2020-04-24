---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/19/2020
ms.author: v-junlch
ms.openlocfilehash: 561c123940645f548cdd8d58414140646664eca0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
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

| properties | 说明 |
| -------- | ----------- |
| id | Azure Functions 扩展捆绑的命名空间。 |
| 版本 | 要安装的捆绑包的版本。 Functions 运行时始终选取由版本范围或间隔定义的可允许最高版本。 上述版本值允许从 1.0.0 到（但不包括）2.0.0 的所有捆绑包版本。 有关详细信息，请参阅[用于指定版本范围的间隔表示法](https://docs.microsoft.com/nuget/reference/package-versioning#version-ranges)。 |

随着捆绑包中的包的变化，捆绑包版本递增。 当捆绑包中的包的主版本递增时，会发生主版本更改。 捆绑包的主版本更改通常与 Functions 运行时主版本的更改同时发生。  

默认捆绑包安装的当前扩展集在此 [extensions.json 文件](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json)中进行枚举。

