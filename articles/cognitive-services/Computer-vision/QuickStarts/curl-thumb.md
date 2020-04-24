---
title: 快速入门：生成缩略图 - REST、cURL
titleSuffix: Azure Cognitive Services
description: 在该快速入门中，你将使用计算机视觉 API 和 cURL 基于图像生成缩略图。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
origin.date: 07/03/2019
ms.date: 07/08/2019
ms.author: v-junlch
ms.custom: seodec18
ms.openlocfilehash: fd05f12735952694e03506dd0c7d639147caf6f6
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67844897"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-curl"></a>快速入门：使用计算机视觉 REST API 和 cURL 生成缩略图

在本快速入门中，你将使用计算机视觉的 REST API 基于图像生成缩略图。 可以指定所需的高度和宽度，可以与输入图像的纵横比不同。 计算机视觉使用智能裁剪来智能识别感兴趣的区域并围绕该区域生成裁剪坐标。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="prerequisites"></a>先决条件

- 必须具有 [cURL](https://curl.haxx.se/windows)。
- 必须具有计算机视觉的订阅密钥。 你可以按照[创建认知服务帐户](/cognitive-services/cognitive-services-apis-create-account)中的说明订阅计算机视觉并获取密钥。

## <a name="get-thumbnail-request"></a>获取缩略图请求

使用 [Get Thumbnail 方法](https://dev.cognitive.azure.cn/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb)，可以生成图像的缩略图。

若要运行此示例，请执行以下步骤：

1. 将以下代码复制到编辑器中。
1. 将 `<Subscription Key>` 替换为有效订阅密钥。
1. 将 `<File>` 替换为保存缩略图所需的路径和文件名。
1. 如有必要，请更改请求 URL (`https://api.cognitive.azure.cn/vision/v2.0`) 以使用获得订阅密钥的位置。
1. （可选）更改要分析的图像 (`{\"url\":\"...`)。
1. 在安装了 cURL 的计算机上打开命令窗口。
1. 将代码粘贴到窗口中并运行命令。

## <a name="create-and-run-the-sample-command"></a>创建并运行示例命令

要创建和运行示例，请执行以下步骤：

1. 将以下命令复制到文本编辑器中。
1. 必要时在命令中进行如下更改：
    1. 将 `<subscriptionKey>` 的值替换为你的订阅密钥。
    1. 将 `<thumbnailFile>` 的值替换为要保存缩略图的文件的路径和名称。
    1. 如有必要，请将请求 URL (`https://api.cognitive.azure.cn/vision/v2.0/generateThumbnail`) 替换为获取的订阅密钥所在的 Azure 区域中的[获取缩略图](https://dev.cognitive.azure.cn/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb)方法的终结点 URL。
    1. （可选）将请求正文 (`https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\`) 中的图像 URL 更改为要从中生成缩略图的其他图像的 URL。
1. 打开命令提示符窗口。
1. 将文本编辑器中的命令粘贴到命令提示符窗口，然后运行命令。

    ```bash
    curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -o <thumbnailFile> -H "Content-Type: application/json" "https://api.cognitive.azure.cn/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
    ```

## <a name="examine-the-response"></a>检查响应

成功的响应会将缩略图写入 `<thumbnailFile>` 中指定的文件。 如果请求失败，则响应包含错误代码和消息，以帮助确定问题所在。 如果请求似乎是成功的，但创建的缩略图不是有效的图像文件，则可能是因为订阅密钥无效。

## <a name="next-steps"></a>后续步骤

探索计算机视觉 API，了解如何分析图像、检测名人和地标、创建缩略图，并提取印刷体文本和手写文本。 要快速体验计算机视觉 API，请尝试使用 [Open API 测试控制台](https://dev.cognitive.azure.cn/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console)。

> [!div class="nextstepaction"]
> [探索计算机视觉 API](https://dev.cognitive.azure.cn/docs/services/5adf991815e1060e6355ad44)

<!-- Update_Description: wording update -->