---
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.topic: include
origin.date: 05/15/2020
ms.date: 06/19/2019
ms.author: v-tawe
ms.openlocfilehash: 2fcb39dd493d66a73e680c79678c0c8d96c201bd
ms.sourcegitcommit: d24e12d49708bbe78db450466eb4fccbc2eb5f99
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85611915"
---
## <a name="prerequisites"></a>先决条件

唯一先决条件是要有一个 Azure 语音订阅。 如果还没有订阅，请参阅[指南](../get-started.md#new-resource)了解如何新建订阅。

## <a name="download-and-install"></a>下载并安装

#### <a name="windows-install"></a>[Windows 安装](#tab/windowsinstall)

按照以下步骤在 Windows 上安装语音 CLI：

1. 安装 [.NET Framework 4.7](https://dotnet.microsoft.com/download/dotnet-framework/net471) 或 [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0)
2. 下载语音 CLI [zip 存档](https://aka.ms/speech/spx-zips.zip)然后提取它。
3. 转到从下载中提取的根目录 `spx-zips`，并提取所需的子目录（`spx-net471` 用于 .NET Framework 4.7，`spx-netcore-win-x64` 用于 x64 CPU 上的 .NET Core 3.0）。

在命令提示符中，将目录更改到此位置，然后键入 `spx` 查看语音 CLI 的帮助。

#### <a name="linux-install"></a>[Linux 安装](#tab/linuxinstall)

按照以下步骤在 x64 CPU 上安装 Linux 上的语音 CLI：

1. 安装 [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0)。
2. 下载语音 CLI [zip 存档](https://aka.ms/speech/spx-zips.zip)然后提取它。
3. 转到从下载中提取的根目录 `spx-zips`，并将 `spx-netcore-30-linux-x64` 提取到新 `~/spx` 目录。
4. 在终端中，键入以下命令：
   1. `cd ~/spx`
   2. `sudo chmod +r+x spx`
   3. `PATH=~/spx:$PATH`

若要查看语音 CLI 的帮助，请键入 `spx`。

***

## <a name="create-subscription-config"></a>创建订阅配置

若要开始使用语音 CLI，首先需要输入语音订阅密钥和区域信息。 请查看[区域支持](https://docs.azure.cn/cognitive-services/speech-service/regions#speech-sdk)页，找到你的区域标识符。 获得订阅密钥和区域标识符后（例如 `chinaeast2`），运行以下命令。

```shell
spx config @key --set YOUR-SUBSCRIPTION-KEY
spx config @region --set YOUR-REGION-ID
```

现在会存储订阅身份验证，用于将来的 SPX 请求。 如果需要删除这些已存储值中的任何一个，请运行 `spx config @region --clear` 或 `spx config @key --clear`。
