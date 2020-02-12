---
title: 如何为 Linux 配置 OpenSSL
titleSuffix: Azure Cognitive Services
description: 了解如何为 Linux 配置 OpenSSL。
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 01/16/2020
ms.date: 02/17/2020
ms.author: v-tawe
ms.openlocfilehash: da4c811a7e80a58b85e0b896955d9891facb57a3
ms.sourcegitcommit: 888cbc10f2348de401d4839a732586cf266883bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028367"
---
# <a name="configure-openssl-for-linux"></a>为 Linux 配置 OpenSSL

使用 1.9.0 之前的任何语音 SDK 版本时，会将 [OpenSSL](https://www.openssl.org) 动态配置为主机系统版本。 在更高版本的语音 SDK 中，OpenSSL（版本为 [1.1.1 b](https://mta.openssl.org/pipermail/openssl-announce/2019-February/000147.html)）会静态链接到语音 SDK 的核心库。

## <a name="troubleshoot-connectivity"></a>排查连接问题

如果使用语音 SDK 的 1.9.0 版本时连接失败，请确保 `ssl/certs` 目录存在于 `/usr/lib` 目录（位于 Linux 文件系统）中。 如果 `ssl/certs` 目录不存在  ，请使用以下命令检查系统中安装 OpenSSL 的位置：

```bash
which openssl
```

然后，找到 OpenSSL `certs` 目录，将该目录的内容复制到 `/usr/lib/ssl/certs` 目录中。 接下来请重试，看是否已解决连接问题。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [关于语音 SDK](speech-sdk.md)