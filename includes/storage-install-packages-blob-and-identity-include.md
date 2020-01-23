---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 11/26/2019
ms.date: 01/06/2020
ms.author: v-jay
ms.custom: include
ms.openlocfilehash: 10a717b116e95b1d0b0a1cbf650030977e4a777f
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624165"
---
## <a name="install-client-library-packages"></a>安装客户端库包

> [!NOTE]
> 本文中的示例使用 Azure 存储客户端库版本 12。 版本 12 的客户端库是 Azure SDK 的一部分。 有关 Azure SDK 的更多信息，请参阅 [GitHub](https://github.com/Azure/azure-sdk) 上的 Azure SDK 存储库。

若要安装 Blob 存储包，请在 NuGet 包管理器控制台中运行以下命令：

```powershell
Install-Package Azure.Storage.Blobs
```

本文中的示例还使用[用于 .NET 的 Azure 标识客户端库](https://www.nuget.org/packages/Azure.Identity/)的最新版通过 Azure AD 凭据进行身份验证。 若要安装包，请在 NuGet 包管理器控制台中运行以下命令：

```powershell
Install-Package Azure.Identity
```
