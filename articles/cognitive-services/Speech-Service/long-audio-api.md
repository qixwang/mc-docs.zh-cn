---
title: 长音频 API（预览）- 语音服务
titleSuffix: Azure Cognitive Services
description: 了解长音频 API 是如何为长格式文本转语音的异步合成而设计的。
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 01/30/2020
ms.date: 07/09/2020
ms.author: v-tawe
ms.openlocfilehash: e367cb69a88d556b50e4f34ec092966196e13724
ms.sourcegitcommit: 08619b8fabb47a328c3ca43bf314ddc389328880
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86164528"
---
# <a name="long-audio-api-preview"></a>长音频 API（预览）

长音频 API 专为长格式文本转语音（例如有声读物、新闻文章和文档）的异步合成而设计。 此 API 不会实时返回合成音频，而是期望你轮询响应并消耗输出，因为服务中提供了这些响应和输出。 与语音 SDK 使用的文本转语音 API 不同，长音频 API 可以创建超过 10 分钟的合成音频，使其成为发布商和音频内容平台的的理想之选。

长音频 API 的其他好处：

* 服务返回的合成语音使用最佳的神经网络语音。
* 无需部署语音终结点，因为它可以在非实时批处理模式下合成语音。

> [!NOTE]
> 长音频 API 现在支持[公共神经网络语音](https://docs.azure.cn/cognitive-services/speech-service/language-support#neural-voices)。

## <a name="workflow"></a>工作流

通常，使用长音频 API 时，需要提交要合成的文本文件并轮询状态，如果状态成功，则可以下载音频输出。

此图高度概括了该工作流。

![长音频 API 工作流关系图](media/long-audio-api/long-audio-api-workflow.png)

## <a name="prepare-content-for-synthesis"></a>为合成准备内容

准备文本文件时，请确保：

* 为纯文本 (.txt) 或 SSML 文本 (.txt)
* 编码为[包含字节顺序标记 (BOM) 的 UTF-8](https://www.w3.org/International/questions/qa-utf8-bom.en#bom)
* 为单个文件，而不是 zip
* 包含 400 多个字符（对于纯文本），或包含 400 个[可计费字符](https://docs.azure.cn/cognitive-services/speech-service/text-to-speech#pricing-note)（对于 SSML 文本），并小于 10,000 个段落
  * 对于纯文本，通过点击 Enter/Return 来分隔每个段落 - 请查看[纯文本输入示例](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/en-US.txt)
  * 对于 SSML 文本，每个 SSML 部分都被视为一个段落。 SSML 部分将分隔为不同的段落 - 请查看 [SSML 文本输入示例](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/SSMLTextInputSample.txt)
> [!NOTE]
> 对于中文（大陆）、中文（香港特别行政区）、中文（台湾）、日语和韩语，一个字将计为两个字符。 

## <a name="submit-synthesis-requests"></a>提交合成请求

准备好输入内容后，请按照[长格式音频合成快速入门](https://docs.azure.cn/cognitive-services/speech-service/quickstarts/text-to-speech/async-synthesis-long-form-audio)提交请求。 如果有多个输入文件，则需要提交多个请求。 

HTTP 状态代码指示常见错误。

| API | HTTP 状态代码 | 说明 | 建议 |
|-----|------------------|-------------|----------|
| 创建 | 400 | 此区域未启用语音合成。 | 使用受支持区域更改语音订阅密钥。 |
|        | 400 | 只有此区域的标准语音订阅才有效。 | 将语音订阅密钥更改为“标准”定价层。 |
|        | 400 | 超过 Azure 帐户的 20,000 个请求限制。 请在提交新请求之前删除一些请求。 | 服务器将为每个 Azure 帐户最多保留 20,000 个请求。 请在提交新请求之前删除一些请求。 |
|        | 400 | 此模型不能用于语音合成 {modelID}。 | 请确保 {modelID} 的状态正确。 |
|        | 400 | 请求区域与模型区域 {modelID} 不匹配。 | 请确保 {modelID} 区域与请求区域匹配。 |
|        | 400 | 语音合成仅支持使用包含字节顺序标记的 UTF-8 编码中的文本文件。 | 请确保输入文件使用包含字节顺序标记的 UTF-8 编码。 |
|        | 400 | 语音合成请求中只允许有效的 SSML 输入。 | 请确保输入 SSML 表达式正确。 |
|        | 400 | 在输入文件中找不到语音名称 {voiceName}。 | 输入 SSML 语音名称与模型 ID 不对齐。 |
|        | 400 | 输入文件中的段落数应小于 10,000。 | 请确保文件中的段落数小于 10,000。 |
|        | 400 | 输入文件应超过 400 个字符。 | 请确保输入文件超过 400 个字符。 |
|        | 404 | 找不到语音合成定义中声明的模型：{modelID}。 | 请确保 {modelID} 正确。 |
|        | 429 | 超出活动语音合成限制。 请等待直到一些请求完成。 | 对于每个 Azure 帐户，服务器最多可以运行 120 个请求并将其排入队列。 请等待并避免提交新请求，直到完成一些请求。 |
| 全部       | 429 | 请求太多。 | 对于每个 Azure 帐户，客户端每秒最多可以向服务器提交 5 个请求。 请减少每秒的请求数。 |
| Delete    | 400 | 语音合成任务仍在使用中。 | 只能删除“已完成”或“已失败”的请求 。 |
| GetByID   | 404 | 找不到指定的实体。 | 请确保合成 ID 正确。 |

## <a name="regions-and-endpoints"></a>区域和终结点

长音频 API 可用于具有单独终结点的多个区域。

| 区域 | 终结点 |
|--------|----------|
| 中国东部 2 | `https://chinaeast2.api.speech.azure.cn` |

## <a name="audio-output-formats"></a>音频输出格式

我们支持灵活的语音输入格式。 可以通过设置“concatenateResult”参数，为每个段落生成音频输出或将音频输出串联到单个输出中。 长音频 API 支持以下音频输出格式：

> [!NOTE]
> 默认音频格式为 riff-16khz-16bit-mono-pcm。

* riff-8khz-16bit-mono-pcm
* riff-16khz-16bit-mono-pcm
* riff-24khz-16bit-mono-pcm
* riff-48khz-16bit-mono-pcm
* audio-16khz-32kbitrate-mono-mp3
* audio-16khz-64kbitrate-mono-mp3
* audio-16khz-128kbitrate-mono-mp3
* audio-24khz-48kbitrate-mono-mp3
* audio-24khz-96kbitrate-mono-mp3
* audio-24khz-160kbitrate-mono-mp3

## <a name="quickstarts"></a>快速入门

我们提供旨在帮助你成功运行长音频 API 的快速入门。 下表按语言列出了长音频 API 快速入门。

* [快速入门：Python](https://docs.azure.cn/cognitive-services/speech-service/quickstarts/text-to-speech/async-synthesis-long-form-audio)

## <a name="sample-code"></a>代码示例
GitHub 上提供了长语音 API 的示例代码。

* [示例代码：Python](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/Python)
* [示例代码：C#](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/CSharp)
* [示例代码：Java](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/)
