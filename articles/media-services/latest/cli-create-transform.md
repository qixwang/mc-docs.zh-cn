---
title: Azure CLI 脚本示例 - 创建转换 | Microsoft Docs
description: 使用 Azure CLI 脚本创建转换。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
origin.date: 05/01/2019
ms.date: 09/23/2019
ms.author: v-jay
ms.openlocfilehash: 06d5c2bbf0648491b15161abc8603e77680cbcbb
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73416257"
---
# <a name="cli-example-create-a-transform"></a>CLI 示例：创建转换

本文中的 Azure CLI 脚本演示如何创建转换。 转换描述了处理视频或音频文件的任务的简单工作流（通常称为“工作程序”）。 应始终检查具有所需名称和“工作程序”的转换是否已存在。 如果已存在，应再次使用该转换。

## <a name="prerequisites"></a>必备条件 

[创建媒体服务帐户](create-account-cli-how-to.md)。

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

> [!NOTE]
> 只能为 [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) 指定自定义标准编码器预设 JSON 文件的路径，请参阅[使用自定义转换进行编码](custom-preset-cli-howto.md)示例。
>
> 使用 [BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) 时，不能传递文件名。

## <a name="example-script"></a>示例脚本

```cli
#!/bin/bash

# Update the following variables for your own settings:
$resourceGroup=amsResourceGroup
$amsAccountName=amsmediaaccountname

# Create a simple Transform for Adaptive Bitrate Encoding
az ams transform create \
 --name myFirstTransform \
 --preset AdaptiveStreaming \
 --description 'a simple Transform for Adaptive Bitrate Encoding' \
 -g $resourceGroup \
 -a $amsAccountName \

 # Create a Transform for Video Analyer Preset
az ams transform create \
 --name videoAnalyzerTransform \
 --preset  VideoAnalyzer \
 -g $resourceGroup \
 -a $amsAccountName \

 # Create a Transform for Audio Analzyer Preset
az ams transform create \
 --name audioAnalyzerTransform \
 --preset  AudioAnalyzer \
 -g $resourceGroup \
 -a $amsAccountName \

# List all the Transforms in an account
az ams transform list -a $amsAccountName -g $resourceGroup

echo "press  [ENTER]  to continue."
read continue
```

## <a name="next-steps"></a>后续步骤

[az ams transform (CLI)](https://docs.microsoft.com/cli/azure/ams/transform?view=azure-cli-latest)
