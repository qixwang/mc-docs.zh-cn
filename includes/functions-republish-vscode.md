---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/01/2020
ms.author: v-junlch
ms.openlocfilehash: d15a2a173d35e3ab8d34bdee8c9a302c2d9b20d8
ms.sourcegitcommit: 1008ad28745709e8d666f07a90e02a79dbbe2be5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85945275"
---
1. 在 Visual Studio Code 中选择 F1 键，打开命令面板。 在命令面板中，搜索并选择“Azure Functions: 部署到函数应用”。

1. 如果尚未登录，系统会提示“登录到 Azure”。 从浏览器登录后，请返回到 Visual Studio Code。 如果拥有多个订阅，请选择包含函数应用的订阅。

1. 在 Azure 中选择现有的函数应用。 收到有关覆盖函数应用中的所有文件的警告时，请选择“部署”以确认警告并继续。

项目将重新构建、重新打包并上传到 Azure。 现有项目将替换为新包，且函数应用将重启。

