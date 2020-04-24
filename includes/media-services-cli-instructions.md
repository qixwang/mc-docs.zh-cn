---
title: include 文件
description: include 文件
services: media-services
author: WenJason
ms.service: media-services
ms.topic: include
origin.date: 01/28/2019
ms.date: 09/23/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 62cfc0146e396a976226fa27070571f4d229945d
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71124355"
---
## <a name="cli"></a>CLI

可以在本地安装 CLI。 有关适用于你的平台的说明，请参阅[安装 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)。

### <a name="sign-in"></a>登录

使用本地安装的 CLI 需要登录到 Azure。 使用 `az login` 命令登录。

如果 CLI 可以打开默认的浏览器，则它会打开该浏览器并加载登录页。 否则，你需要打开一个浏览器页面，在浏览器中导航到 https://microsoft.com/deviceloginchina 后，按照有关命令行的说明输入授权代码。

### <a name="specify-location-of-files"></a>指定文件位置

许多媒体服务 CLI 命令允许你通过文件名来传递参数。 

![上传文件]

需要根据所用的 OS 或 Shell（Bash 或 PowerShell）指定文件路径。 下面是一些示例：

文件（所有 OS）的相对路径

* `@"mytestfile.json"`
* `@"../mytestfile.json"`

Linux/Mac 和 Windows OS 上的绝对文件路径

* `@ "/usr/home/mytestfile.json"`
*   `@"c:\tmp\user\mytestfile.json"`

如果命令要求提供文件路径，请使用 `{file}`。 例如，`az ams transform create -a amsaccount -g resourceGroup -n custom --preset .\customPreset.json` 。 <br/> 如果命令将加载指定的文件，请使用 `@{file}`。 例如，`az ams account-filter create -a amsaccount -g resourceGroup -n filterName --tracks @tracks.json` 。

[上传文件]: ./media/media-services-cli/upload-download-files.png
