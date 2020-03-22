---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/19/2020
ms.author: v-junlch
ms.openlocfilehash: 3e295d70ba354ce5da3b5eaab0acff498ee86d37
ms.sourcegitcommit: e500354e2fd8b7ac3dddfae0c825cc543080f476
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2020
ms.locfileid: "79546891"
---
### <a name="set-the-storage-account-connection"></a>设置存储帐户连接

打开 local.settings.json 文件并复制 `AzureWebJobsStorage` 的值（即存储帐户连接字符串）。 使用以下 Bash 命令将 `AZURE_STORAGE_CONNECTION_STRING` 环境变量设置为该连接字符串：

```bash
AZURE_STORAGE_CONNECTION_STRING="<STORAGE_CONNECTION_STRING>"
```

在 `AZURE_STORAGE_CONNECTION_STRING` 环境变量中设置连接字符串时，无需每次都要提供身份验证，即可访问你的存储帐户。

