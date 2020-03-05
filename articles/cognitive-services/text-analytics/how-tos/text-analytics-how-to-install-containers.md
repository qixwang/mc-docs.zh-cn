---
title: 安装和运行容器 - 文本分析
titleSuffix: Azure Cognitive Services
description: 通过本演练教程了解如何下载、安装和运行文本分析容器。
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
origin.date: 11/21/2019
ms.date: 02/25/2020
ms.author: v-lingwu
ms.openlocfilehash: e4fd49e670a4043b8752582ee6086bb9cf14a908
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78154663"
---
# <a name="install-and-run-text-analytics-containers"></a>安装和运行文本分析容器

容器使你能够在自己的环境中运行文本分析 API，最适合特定安全性和数据管理要求。 文本分析容器提供对原始文本的高级自然语言处理，并且包含三项主要功能：情绪分析、关键短语提取和语言检测。 容器当前不支持实体链接。

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial)。

## <a name="prerequisites"></a>先决条件

若要运行任何文本分析容器，必须具有主计算机和容器环境。

## <a name="preparation"></a>准备工作

使用文本分析容器之前，必须满足以下先决条件：

|必须|目的|
|--|--|
|Docker 引擎| 需要在[主计算机](#the-host-computer)上安装 Docker 引擎。 Docker 提供用于在 [macOS](https://docs.docker.com/docker-for-mac/)、[Windows](https://docs.docker.com/docker-for-windows/) 和 [Linux](https://docs.docker.com/engine/installation/#supported-platforms) 上配置 Docker 环境的包。 有关 Docker 和容器的基础知识，请参阅 [Docker 概述](https://docs.docker.com/engine/docker-overview/)。<br><br> 必须将 Docker 配置为允许容器连接 Azure 并向其发送账单数据。 <br><br>  在 Windows 上，还必须将 Docker 配置为支持 Linux 容器。<br><br>|
|熟悉 Docker | 应对 Docker 概念有基本的了解，例如注册表、存储库、容器和容器映像，以及基本的 `docker` 命令的知识。| 
|文本分析资源 |若要使用容器，必须具有：<br><br>一个用于获取关联 API 密钥和终结点 URI 的 Azure [文本分析资源](../../cognitive-services-apis-create-account.md)。 这两个值可以从 Azure 门户中的“文本分析概述”和“密钥”页面获得，并且是启动容器时所必需的。<br><br>**{API_KEY}** ：“密钥”页上提供的两个可用资源密钥中的一个 <br><br>**{ENDPOINT_URI}** ：“概述”页上提供的终结点 |

[!INCLUDE [Gathering required parameters](../../containers/includes/container-gathering-required-parameters.md)]

## <a name="the-host-computer"></a>主计算机

[!INCLUDE [Host Computer requirements](../../../../includes/cognitive-services-containers-host-computer.md)]

## <a name="query-the-containers-prediction-endpoint"></a>查询容器的预测终结点

容器提供了基于 REST 的查询预测终结点 API。

使用主机 `http://localhost:5000`，以获得容器 API。

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>停止容器

[!INCLUDE [How to stop the container](../../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>故障排除

如果运行启用了输出[装入点](../text-analytics-resource-container-config.md#mount-settings)和日志记录的容器，该容器会生成有助于排查启动或运行容器时发生的问题的日志文件。

## <a name="billing"></a>计费

文本分析容器使用 Azure 帐户中的_文本分析_资源向 Azure 发送账单信息。 

[!INCLUDE [Container's Billing Settings](../../../../includes/cognitive-services-containers-how-to-billing-info.md)]

有关这些选项的详细信息，请参阅[配置容器](../text-analytics-resource-container-config.md)。

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>摘要

在本文中，我们已学习相关的概念，以及文本分析容器的下载、安装和运行工作流。 综上所述：

* 文本分析为 Docker 提供了三个 Linux 容器，其中封装了各种功能：
   * *关键短语提取*
   * *语言检测*
   * *情绪分析*
* 从 Azure 中的 Microsoft 容器注册表 (MCR) 下载容器映像。
* 容器映像在 Docker 中运行。
* 可以使用 REST API 或 SDK 通过指定容器的主机 URI 来调用文本分析容器中的操作。
* 必须在实例化容器时指定账单信息。

> [!IMPORTANT]
> 如果未连接到 Azure 进行计量，则无法授权并运行认知服务容器。 客户需要始终让容器向计量服务传送账单信息。 认知服务容器不会将客户数据（例如，正在分析的图像或文本）发送给 Microsoft。

## <a name="next-steps"></a>后续步骤

* 查看[配置容器](../text-analytics-resource-container-config.md)了解配置设置
* 参阅[常见问题解答 (FAQ)](../text-analytics-resource-faq.md) 解决与功能相关的问题。


<!-- Update_Description: wording update -->