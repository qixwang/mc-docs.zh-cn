---
title: 如何使用批量听录 - 语音服务
titleSuffix: Azure Cognitive Services
description: 如果要听录存储（如 Azure Blob）中的大量音频，则批量听录是理想的选择。 使用专用 REST API 可以通过共享访问签名 (SAS) URI 指向音频文件并异步接收听录。
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 12/17/2019
ms.date: 01/13/2020
ms.author: v-tawe
ms.openlocfilehash: 4a7d3fcb1cecb75e01bce79ed4c90ee0e6433fd3
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75630878"
---
# <a name="how-to-use-batch-transcription"></a>如何使用批量听录

批量听录非常适合用于在存储中听录大量音频。 使用专用 REST API 可以通过共享访问签名 (SAS) URI 指向音频文件并异步接收听录结果。

该 API 提供异步语音转文本听录和其他功能。 可以使用 REST API 来公开方法，以执行以下操作：

- 创建批处理请求
- 查询状态
- 下载听录结果
- 从服务中删除听录信息

详细的 API 以 [Swagger 文档](https://chinaeast2.cris.azure.cn/swagger/ui/index#/Custom%20Speech%20transcriptions%3A)的形式在标题 `Custom Speech transcriptions` 下提供。

批量听录作业是按“尽力而为”的原则安排的。 目前，无法预估作业何时更改为正在运行状态。 在正常的系统负载下，几分钟内应该就可以发生该作业。 进入运行状态后，实际听录的处理速度比实时音频更快。

凭借易用的 API，无需部署自定义终结点，且无需遵守任何并发性要求。

## <a name="prerequisites"></a>先决条件

### <a name="subscription-key"></a>订阅密钥

与语音服务的其他所有功能一样，需要按照[入门指南](get-started.md)通过 [Azure 门户](https://portal.azure.cn)创建订阅密钥。

>[!NOTE]
> 若要使用批量听录，需要具备语音服务的标准订阅 (S0)。 免费订阅密钥 (F0) 不可用。 有关详细信息，请参阅[定价和限制](https://www.azure.cn/pricing/details/cognitive-services/speech-services/)。

### <a name="custom-models"></a>自定义模式

如果你打算自定义声学或语言模型，请遵循[自定义声学模型](how-to-customize-acoustic-models.md)和[设计自定义语言模型](how-to-customize-language-model.md)中的步骤。 若要在批量听录中使用所创建的模型，需要提供其模型 ID。 可以在检查模型的详细信息时检索模型 ID。 批量听录服务不需要部署的自定义终结点。

## <a name="the-batch-transcription-api"></a>批量听录 API

### <a name="supported-formats"></a>支持的格式

批量听录 API 支持以下格式：

| 格式 | 编解码器 | 比特率 | 采样率 |
|--------|-------|---------|-------------|
| WAV | PCM | 16 位 | 8 kHz 或 16 kHz，单声道或立体声 |
| MP3 | PCM | 16 位 | 8 kHz 或 16 kHz，单声道或立体声 |
| OGG | OPUS | 16 位 | 8 kHz 或 16 kHz，单声道或立体声 |

对于立体声音频流，在听录期间会拆分左右声道。 对于每个声道，将创建一个 JSON 结果文件。 开发人员可利用为每个言语生成的时间戳创建有序的最终脚本。

### <a name="configuration"></a>配置

配置参数以 JSON 形式提供：

```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to use, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "ProfanityFilterMode": "None | Removed | Tags | Masked",
    "PunctuationMode": "None | Dictated | Automatic | DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True | False",
    "AddSentiment" : "True | False",
    "AddDiarization" : "True | False",
    "TranscriptionResultsContainerUrl" : "<SAS to Azure container to store results into (write permission required)>"
  }
}
```

### <a name="configuration-properties"></a>配置属性

使用以下可选属性来配置听录：

| 参数 | 说明 |
|-----------|------------|
|`ProfanityFilterMode`|指定如何处理识别结果中的亵渎内容
||**`Masked`** - 默认设置。 以星号取代亵渎内容<br>`None` - 禁用亵渎内容筛选<br>`Removed` - 从结果中删除所有亵渎内容<br>`Tags` - 添加亵渎内容标记
|`PunctuationMode`|指定如何处理识别结果中的标点
||`Automatic` - 服务插入标点<br>`Dictated` - 听写的（口述）标点<br>**`DictatedAndAutomatic`** - 默认设置。 听写和自动标点<br>`None` - 禁用标点
|`AddWordLevelTimestamps`|指定是否应将单词级时间戳添加到输出
||`True` - 启用单词级时间戳<br>**`False`** - 默认设置。 禁用单词级时间戳
|`AddSentiment`|指定是否将情绪分析添加到言语
||`True` - 为每个言语启用情绪分析<br>**`False`** - 默认设置。 禁用情绪分析
|`AddDiarization`|指定是否执行分割聚类分析。如果为 `true`，则输入预期为单声道音频，其中最多包含两段语音。 `AddWordLevelTimestamps` 需设置为 `true`
||`True` - 启用分割聚类<br>**`False`** - 默认设置。 禁用分割聚类
|`TranscriptionResultsContainerUrl`|将可选的 SAS 令牌存储到 Azure 中的可写容器。 结果将存储在此容器中

### <a name="storage"></a>存储

批量听录支持使用 [Azure Blob 存储](https://docs.azure.cn/storage/blobs/storage-blobs-overview)来读取音频以及将听录内容写入存储。

## <a name="the-batch-transcription-result"></a>批量听录结果

对于单声道输入音频，将创建一个听录结果文件。 对于立体声输入音频，将创建两个听录结果文件。 每个文件采用以下结构：

```json
{
  "AudioFileResults":[ 
    {
      "AudioFileName": "Channel.0.wav | Channel.1.wav"      'maximum of 2 channels supported'
      "AudioFileUrl": null                                  'always null'
      "AudioLengthInSeconds": number                        'Real number. Two decimal places'
      "CombinedResults": [
        {
          "ChannelNumber": null                             'always null'
          "Lexical": string
          "ITN": string
          "MaskedITN": string
          "Display": string
        }
      ]
      SegmentResults:[                                     'for each individual segment'
        {
          "RecognitionStatus": Success | Failure
          "ChannelNumber": null
          "SpeakerId": null | "1 | 2"                     'null if no diarization
                                                            or stereo input file, the
                                                            speakerId as a string if
                                                            diarization requested for
                                                            mono audio file'
          "Offset": number                                'time in milliseconds'
          "Duration": number                              'time in milliseconds'
          "OffsetInSeconds" : number                      'Real number. Two decimal places'
          "DurationInSeconds" : number                    'Real number. Two decimal places'
          "NBest": [
            {
              "Confidence": number                        'between 0 and 1'
              "Lexical": string
              "ITN": string
              "MaskedITN": string
              "Display": string
              "Sentiment":
                {                                          'this is omitted if sentiment is
                                                            not requested'
                  "Negative": number                        'between 0 and 1'
                  "Neutral": number                         'between 0 and 1'
                  "Positive": number                        'between 0 and 1'
                }
              "Words": [
                {
                  "Word": string
                  "Offset": number                         'time in milliseconds'
                  "Duration": number                       'time in milliseconds'
                  "OffsetInSeconds": number                'Real number. Two decimal places'
                  "DurationInSeconds": number              'Real number. Two decimal places'
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

结果包含以下形式：

|表单|内容|
|-|-|
|`Lexical`|识别的实际单词。
|`ITN`|已识别文本的反向文本规范化形式。 已应用缩写（“doctor smith”缩写为“dr smith”）、电话号码和其他转换。
|`MaskedITN`|应用了亵渎内容屏蔽的 ITN 形式。
|`Display`|已识别文本的显示形式。 包括添加的标点和大小写。

## <a name="speaker-separation-diarization"></a>讲述人分离（分割聚类）

分割聚类是将讲述人语音分隔成音频片段的过程。 Batch 管道支持分割聚类，并且能够识别单声道录制内容中的两个讲述人。 此功能不适用于立体声录音。

所有听录输出包含一个 `SpeakerId`。 如果不使用分割聚类，将在 JSON 输出中显示 `"SpeakerId": null`。 对于分割聚类，我们支持两段语音，因此讲述人将标识为 `"1"` 或 `"2"`。

若要请求分割聚类，只需在 HTTP 请求中添加相关的参数，如下所示。

 ```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to us, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "AddWordLevelTimestamps" : "True",
    "AddDiarization" : "True"
  }
}
```

如上述请求中的参数所示，还必须“启用”单词级时间戳。

## <a name="sentiment-analysis"></a>情绪分析

情绪分析功能评估音频中表达的情绪。 情绪由 0 到 1 的值表示，分别表示 `Negative`、`Neutral` 和 `Positive` 情绪。 例如，可以在呼叫中心方案中使用情绪分析：

- 获取有关客户满意度的见解
- 获取有关座席（受理来电的团队）绩效的见解
- 找出某次通话改变消极结局的确切时间点
- 将消极通话转变为积极通话时的良好交互情况
- 了解客户对某个产品或服务喜欢和不喜欢的方面

情绪根据词法形式按音频段评分。 该音频段内的整个文本用于计算情绪。 不会计算整个听录的聚合情绪。

下面是 JSON 输出示例：

```json
{
  "AudioFileResults": [
    {
      "AudioFileName": "Channel.0.wav",
      "AudioFileUrl": null,
      "SegmentResults": [
        {
          "RecognitionStatus": "Success",
          "ChannelNumber": null,
          "Offset": 400000,
          "Duration": 13300000,
          "NBest": [
            {
              "Confidence": 0.976174,
              "Lexical": "what's the weather like",
              "ITN": "what's the weather like",
              "MaskedITN": "what's the weather like",
              "Display": "What's the weather like?",
              "Words": null,
              "Sentiment": {
                "Negative": 0.206194,
                "Neutral": 0.793785,
                "Positive": 0.0
              }
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="best-practices"></a>最佳实践

听录服务可以处理大量的已提交听录内容。 可以通过[听录方法](https://chinaeast2.cris.azure.cn/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/GetTranscriptions)中的 `GET` 查询听录的状态。 通过指定 `take` 参数来保持以合理的大小（数百）返回信息。 检索结果后，定期从服务中[删除听录](https://chinaeast2.cris.azure.cn/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/DeleteTranscription)。 这可以保证快速获取听录管理调用的回复。

## <a name="sample-code"></a>代码示例

`samples/batch` 子目录内的 [GitHub 示例存储库](https://github.com/Azure-Samples/cognitive-services-speech-sdk)中提供了完整示例。

如要使用自定义声学或语言模型，必须使用订阅信息、服务区域、指向要转录的音频文件的 SAS URI 和模型 ID 来自定义示例代码。

> [!NOTE]
> 创建批处理客户端时，请为 Azure 中国将终结点更改为 `https://chinaeast2.cris.azure.cn`。

```csharp
// Replace with your subscription key
private const string SubscriptionKey = "YourSubscriptionKey";

// Update with your service region
private const string Region = "YourServiceRegion";
private const int Port = 443;

// recordings and locale
private const string Locale = "en-US";
private const string RecordingsBlobUri = "<SAS URI pointing to an audio file stored in Azure Blob Storage>";

// For usage of baseline models, no acoustic and language model needs to be specified.
private static Guid[] modelList = new Guid[0];

// For use of specific acoustic and language models:
// - comment the previous line
// - uncomment the next lines to create an array containing the guids of your required model(s)
// private static Guid AdaptedAcousticId = new Guid("<id of the custom acoustic model>");
// private static Guid AdaptedLanguageId = new Guid("<id of the custom language model>");
// private static Guid[] modelList = new[] { AdaptedAcousticId, AdaptedLanguageId };

//name and description
private const string Name = "Simple transcription";
private const string Description = "Simple transcription description";
```

示例代码将设置客户端并提交听录请求。 然后它将轮询状态信息并打印关于转录进度的详细信息。

```csharp
// get all transcriptions for the user
transcriptions = await client.GetTranscriptionsAsync().ConfigureAwait(false);

completed = 0; running = 0; notStarted = 0;
// for each transcription in the list we check the status
foreach (var transcription in transcriptions)
{
    switch (transcription.Status)
    {
        case "Failed":
        case "Succeeded":
            // we check to see if it was one of the transcriptions we created from this client.
            if (!createdTranscriptions.Contains(transcription.Id))
            {
                // not created form here, continue
                continue;
            }
            completed++;

            // if the transcription was successful, check the results
            if (transcription.Status == "Succeeded")
            {
                var resultsUri0 = transcription.ResultsUrls["channel_0"];

                WebClient webClient = new WebClient();

                var filename = Path.GetTempFileName();
                webClient.DownloadFile(resultsUri0, filename);
                var results0 = File.ReadAllText(filename);
                var resultObject0 = JsonConvert.DeserializeObject<RootObject>(results0);
                Console.WriteLine(results0);

                Console.WriteLine("Transcription succeeded. Results: ");
                Console.WriteLine(results0);
            }
            else
            {
                Console.WriteLine("Transcription failed. Status: {0}", transcription.StatusMessage);
            }
            break;

        case "Running":
            running++;
            break;

        case "NotStarted":
            notStarted++;
            break;
    }
}
```

有关上述调用的完整详细信息，请参阅 [Swagger 文档](https://chinaeast2.cris.azure.cn/swagger/ui/index)。 有关此处所示的完整示例，请转到 `samples/batch` 子目录中的 [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk)。

请注意用于发布音频和接收听录状态的异步设置。 创建的客户端是一个 .NET HTTP 客户端。 `PostTranscriptions` 方法用于发送音频文件详细信息，`GetTranscriptions` 方法用于接收结果。 `PostTranscriptions` 返回句柄，`GetTranscriptions` 使用此句柄创建一个句柄来获取听录状态。

当前示例代码未指定任何自定义模型。 该服务使用基线模型来听录一个或多个文件。 若要指定模型，可为声学和语言模型传递与模型 ID 相同的方法。

> [!NOTE]
> 对于基线听录，无需声明基线模型的 ID。 如果只指定语言模型 ID（而不指定声学模型 ID），则自动选择匹配的声学模型。 如果只指定声学模型 ID，则自动选择匹配的语言模型。

## <a name="download-the-sample"></a>下载示例

可以在 [GitHub 示例存储库](https://github.com/Azure-Samples/cognitive-services-speech-sdk)的 `samples/batch` 目录中查找到该示例。

## <a name="next-steps"></a>后续步骤

* [获取 1 元人民币的试用订阅](https://www.azure.cn/pricing/1rmb-trial/)
