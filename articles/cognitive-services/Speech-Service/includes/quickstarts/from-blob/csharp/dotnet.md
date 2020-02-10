---
title: 快速入门：识别存储在 Blob 存储中的语音，C# - 语音服务
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 01/13/2020
ms.date: 02/17/2020
ms.author: v-tawe
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 463e2b53c374d91c8de80b07b9f461e575c3e464
ms.sourcegitcommit: 888cbc10f2348de401d4839a732586cf266883bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77029204"
---
## <a name="prerequisites"></a>必备条件

在开始之前，请务必：

> [!div class="checklist"]
> * [设置开发环境](../../../../quickstarts/setup-platform.md?tabs=vs&pivots=programmming-language-csharp)
> * [创建空示例项目](../../../../quickstarts/create-project.md?pivots=programmming-language-csharp)
> * [创建 Azure 语音资源](../../../../get-started.md)
> * [将源文件上传到 Azure blob](https://docs.azure.cn/storage/blobs/storage-quickstart-blobs-portal)

## <a name="open-your-project-in-visual-studio"></a>在 Visual Studio 中打开项目

第一步是确保在 Visual Studio 中打开项目。

1. 启动 Visual Studio 2019。
2. 加载项目并打开 `Program.cs`。

## <a name="add-a-reference-to-newtonsoftjson"></a>添加对 Newtonsoft.Json 的引用

1. 在解决方案资源管理器中右键单击“helloworld”项目，然后选择“管理 NuGet 包”显示 NuGet 包管理器。  

1. 在右上角找到“包源”下拉框，确保选择了 `nuget.org`   。

1. 在左上角，选择“浏览”  。

1. 在搜索框中键入“Newtonsoft.Json”并选择“输入”   。

1. 从搜索结果中选择 [**Newtonsoft.Json**](https://www.nuget.org/packages/Newtonsoft.Json) 包，然后选择“安装”以安装最新稳定版本  。

1. 接受所有协议和许可证，开始安装。

   安装此包后，“包管理器控制台”  窗口中将显示一条确认消息。

## <a name="start-with-some-boilerplate-code"></a>从一些样本代码入手

添加一些代码作为项目的框架。

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Threading.Tasks;
using System.Net.Http;
using System.Net.Http.Formatting;

namespace BatchClient
{
    class Program
    {
        // Replace with your subscription key
        private const string SubscriptionKey = "YourSubscriptionKey";

        // Update with your service region
        private const string Region = "YourServiceRegion";
        private const int Port = 443;

        // recordings and locale
        private const string Locale = "en-US";
        private const string RecordingsBlobUri = "YourFileUrl";

        //name and description
        private const string Name = "Simple transcription";
        private const string Description = "Simple transcription description";

        private const string speechToTextBasePath = "api/speechtotext/v2.0/";

        static void Main(string[] args)
        {
            TranscribeAsync().Wait();
        }

        static async Task TranscribeAsync()
        {
            Console.WriteLine("Starting transcriptions client...");
    }
}
```

[!INCLUDE [placeholder-replacements](../placeholder-replacement.md)]

## <a name="json-wrappers"></a>JSON 包装器

由于 REST API 采用 JSON 格式的请求并且也返回 JSON 结果，因此我们可以仅使用字符串与之进行交互，但不建议这样做。
为了使请求和响应更易于管理，我们将声明一些用于对 JSON 进行序列化/反序列化处理的类。

开始操作并在 `TranscribeAsync` 之后放置声明。
```csharp
public sealed class ModelIdentity
{
    private ModelIdentity(Guid id)
    {
        this.Id = id;
    }

    public Guid Id { get; private set; }

    public static ModelIdentity Create(Guid Id)
    {
        return new ModelIdentity(Id);
    }
}

public sealed class Transcription
{
    [JsonConstructor]
    private Transcription(Guid id, string name, string description, string locale, DateTime createdDateTime, DateTime lastActionDateTime, string status, Uri recordingsUrl, IReadOnlyDictionary<string, string> resultsUrls)
    {
        this.Id = id;
        this.Name = name;
        this.Description = description;
        this.CreatedDateTime = createdDateTime;
        this.LastActionDateTime = lastActionDateTime;
        this.Status = status;
        this.Locale = locale;
        this.RecordingsUrl = recordingsUrl;
        this.ResultsUrls = resultsUrls;
    }

    /// <inheritdoc />
    public string Name { get; set; }

    /// <inheritdoc />
    public string Description { get; set; }

    /// <inheritdoc />
    public string Locale { get; set; }

    /// <inheritdoc />
    public Uri RecordingsUrl { get; set; }

    /// <inheritdoc />
    public IReadOnlyDictionary<string, string> ResultsUrls { get; set; }

    public Guid Id { get; set; }

    /// <inheritdoc />
    public DateTime CreatedDateTime { get; set; }

    /// <inheritdoc />
    public DateTime LastActionDateTime { get; set; }

    /// <inheritdoc />
    public string Status { get; set; }

    public string StatusMessage { get; set; }
}

public sealed class TranscriptionDefinition
{
    private TranscriptionDefinition(string name, string description, string locale, Uri recordingsUrl, IEnumerable<ModelIdentity> models)
    {
        this.Name = name;
        this.Description = description;
        this.RecordingsUrl = recordingsUrl;
        this.Locale = locale;
        this.Models = models;
        this.properties = new Dictionary<string, string>();
        this.properties.Add("PunctuationMode", "DictatedAndAutomatic");
        this.properties.Add("ProfanityFilterMode", "Masked");
        this.properties.Add("AddWordLevelTimestamps", "True");
    }

    /// <inheritdoc />
    public string Name { get; set; }

    /// <inheritdoc />
    public string Description { get; set; }

    /// <inheritdoc />
    public Uri RecordingsUrl { get; set; }

    public string Locale { get; set; }

    public IEnumerable<ModelIdentity> Models { get; set; }

    public IDictionary<string, string> properties { get; set; }

    public static TranscriptionDefinition Create(
        string name,
        string description,
        string locale,
        Uri recordingsUrl)
    {
        return TranscriptionDefinition.Create(name, description, locale, recordingsUrl, new ModelIdentity[0]);
    }

    public static TranscriptionDefinition Create(
        string name,
        string description,
        string locale,
        Uri recordingsUrl,
        IEnumerable<ModelIdentity> models)
    {
        return new TranscriptionDefinition(name, description, locale, recordingsUrl, models);
    }
}

public class AudioFileResult
{
    public string AudioFileName { get; set; }
    public List<SegmentResult> SegmentResults { get; set; }
}

public class RootObject
{
    public List<AudioFileResult> AudioFileResults { get; set; }
}

public class NBest
{
    public double Confidence { get; set; }
    public string Lexical { get; set; }
    public string ITN { get; set; }
    public string MaskedITN { get; set; }
    public string Display { get; set; }
}

public class SegmentResult
{
    public string RecognitionStatus { get; set; }
    public string Offset { get; set; }
    public string Duration { get; set; }
    public List<NBest> NBest { get; set; }
}
```

## <a name="create-and-configure-an-http-client"></a>创建和配置 Http 客户端
首先，我们需要具有正确的基本 URL 和身份验证集的 Http 客户端。
将此代码插入 `TranscribeAsync`
```csharp
// create the client object and authenticate
var client = new HttpClient();
client.Timeout = TimeSpan.FromMinutes(25);
client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, $"{Region}.cris.azure.cn", Port).Uri;

client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", SubscriptionKey);

```

## <a name="generate-a-transcription-request"></a>生成听录请求
接下来，我们将生成听录请求。 将此代码添加到 `TranscribeAsync`
```csharp
var path = $"{speechToTextBasePath}Transcriptions/";
var transcriptionDefinition = TranscriptionDefinition.Create(Name, Description, Locale, new Uri(RecordingsBlobUri));

string res = JsonConvert.SerializeObject(transcriptionDefinition);
StringContent sc = new StringContent(res);
sc.Headers.ContentType = JsonMediaTypeFormatter.DefaultMediaType;
```

## <a name="send-the-request-and-check-its-status"></a>发送请求并查看其状态
现在我们将请求发布到语音服务并检查初始响应代码。 此响应代码将仅指示服务是否已收到请求。 该服务将在响应标头中返回一个 URL，这是它将存储听录状态的位置。
```csharp
Uri transcriptionLocation = null;

using (var response = await client.PostAsync(path, sc))
{
    if (!response.IsSuccessStatusCode)
    {
        Console.WriteLine("Error {0} starting transcription.", response.StatusCode);
        return;
    }

    transcriptionLocation = response.Headers.Location;
}
```

## <a name="wait-for-the-transcription-to-complete"></a>等待听录完成
由于服务以异步方式处理听录，因此需要时常轮询其状态。 我们每 5 秒查看一次。

通过检索在发布请求时收到的 URL 中的内容，可以查看状态。 内容返回后，我们将其反序列化为一个帮助程序类，使其便于交互。

下面是一个轮询代码，其中显示了除成功完成之外的所有状态，我们会在下一步完成该操作。
```csharp
    Console.WriteLine($"Created transcription at location {transcriptionLocation}.");

    Console.WriteLine("Checking status.");

    bool completed = false;
    Transcription transcription = null;

    // check for the status of our transcriptions periodically
    while (!completed)
    {
        // get all transcriptions for the user
        using (var response = await client.GetAsync(transcriptionLocation.AbsolutePath).ConfigureAwait(false))
        {
            var contentType = response.Content.Headers.ContentType;

            if (response.IsSuccessStatusCode && string.Equals(contentType.MediaType, "application/json", StringComparison.OrdinalIgnoreCase))
            {
                transcription = await response.Content.ReadAsAsync<Transcription>().ConfigureAwait(false);
            }
            else
            {
                Console.WriteLine("Error with status {0} getting transcription result", response.StatusCode);
                continue;
            }

        }

        // for each transcription in the list we check the status
        switch (transcription.Status)
        {
            case "Failed":
                completed = true;
                Console.WriteLine("Transcription failed. Status: {0}", transcription.StatusMessage);
                break;
            case "Succeeded":
                break;

            case "Running":
                Console.WriteLine("Transcription is still running.");
                break;

            case "NotStarted":
                Console.WriteLine("Transcription has not started.");
                break;
        }
        
        await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
    }

    Console.WriteLine("Press any key...");
    Console.ReadKey();
}
```

## <a name="display-the-transcription-results"></a>显示听录结果
服务成功完成听录后，结果将存储在可从状态响应中获取的其他 URL 中。

在此，我们先发出请求将这些结果下载到临时文件中，再进行读取和反序列化操作。
加载结果后，可以将其打印到控制台。
```csharp
completed = true;

var resultsUri0 = transcription.ResultsUrls["channel_0"];

WebClient webClient = new WebClient();

var filename = Path.GetTempFileName();
webClient.DownloadFile(resultsUri0, filename);
var results0 = File.ReadAllText(filename);
var resultObject0 = JsonConvert.DeserializeObject<RootObject>(results0);
Console.WriteLine(results0);

Console.WriteLine("Transcription succeeded. Results: ");
Console.WriteLine(results0);
```

## <a name="check-your-code"></a>查看代码
此时，代码应如下所示：（我们已向此版本添加了一些注释）
```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Threading.Tasks;
using System.Net.Http;
using System.Net.Http.Formatting;

namespace BatchClient
{
    class Program
    {
        // Replace with your subscription key
        private const string SubscriptionKey = "YourSubscriptionKey";

        // Update with your service region
        private const string Region = "YourServiceRegion";
        private const int Port = 443;

        // recordings and locale
        private const string Locale = "en-US";
        private const string RecordingsBlobUri = "YourFileUrl";

        //name and description
        private const string Name = "Simple transcription";
        private const string Description = "Simple transcription description";

        private const string speechToTextBasePath = "api/speechtotext/v2.0/";

        static void Main(string[] args)
        {
            TranscribeAsync().Wait();
        }

        static async Task TranscribeAsync()
        {
            Console.WriteLine("Starting transcriptions client...");

            // create the client object and authenticate
            var client = new HttpClient();
            client.Timeout = TimeSpan.FromMinutes(25);
            client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, $"{Region}.cris.azure.cn", Port).Uri;

            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", SubscriptionKey);

            var path = $"{speechToTextBasePath}Transcriptions/";
            var transcriptionDefinition = TranscriptionDefinition.Create(Name, Description, Locale, new Uri(RecordingsBlobUri));

            string res = JsonConvert.SerializeObject(transcriptionDefinition);
            StringContent sc = new StringContent(res);
            sc.Headers.ContentType = JsonMediaTypeFormatter.DefaultMediaType;

            Uri transcriptionLocation = null;

            using (var response = await client.PostAsync(path, sc))
            {
                if (!response.IsSuccessStatusCode)
                {
                    Console.WriteLine("Error {0} starting transcription.", response.StatusCode);
                    return;
                }

                transcriptionLocation = response.Headers.Location;
            }

            Console.WriteLine($"Created transcription at location {transcriptionLocation}.");

            Console.WriteLine("Checking status.");

            bool completed = false;
            Transcription transcription = null;

            // check for the status of our transcriptions periodically
            while (!completed)
            {
                // get all transcriptions for the user
                using (var response = await client.GetAsync(transcriptionLocation.AbsolutePath).ConfigureAwait(false))
                {
                    var contentType = response.Content.Headers.ContentType;

                    if (response.IsSuccessStatusCode && string.Equals(contentType.MediaType, "application/json", StringComparison.OrdinalIgnoreCase))
                    {
                        transcription = await response.Content.ReadAsAsync<Transcription>().ConfigureAwait(false);
                    }
                    else
                    {
                        Console.WriteLine("Error with status {0} getting transcription result", response.StatusCode);
                        continue;
                    }

                }

                // for each transcription in the list we check the status
                switch (transcription.Status)
                {
                    case "Failed":
                        completed = true;
                        Console.WriteLine("Transcription failed. Status: {0}", transcription.StatusMessage);
                        break;
                    case "Succeeded":
                        completed = true;

                        var resultsUri0 = transcription.ResultsUrls["channel_0"];

                        WebClient webClient = new WebClient();

                        var filename = Path.GetTempFileName();
                        webClient.DownloadFile(resultsUri0, filename);
                        var results0 = File.ReadAllText(filename);
                        var resultObject0 = JsonConvert.DeserializeObject<RootObject>(results0);
                        Console.WriteLine(results0);

                        Console.WriteLine("Transcription succeeded. Results: ");
                        Console.WriteLine(results0);
                        break;

                    case "Running":
                        Console.WriteLine("Transcription is still running.");
                        break;

                    case "NotStarted":
                        Console.WriteLine("Transcription has not started.");
                        break;
                }
                
                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }

            Console.WriteLine("Press any key...");
            Console.ReadKey();
        }
    }

    public sealed class ModelIdentity
    {
        private ModelIdentity(Guid id)
        {
            this.Id = id;
        }

        public Guid Id { get; private set; }

        public static ModelIdentity Create(Guid Id)
        {
            return new ModelIdentity(Id);
        }
    }

    public sealed class Transcription
    {
        [JsonConstructor]
        private Transcription(Guid id, string name, string description, string locale, DateTime createdDateTime, DateTime lastActionDateTime, string status, Uri recordingsUrl, IReadOnlyDictionary<string, string> resultsUrls)
        {
            this.Id = id;
            this.Name = name;
            this.Description = description;
            this.CreatedDateTime = createdDateTime;
            this.LastActionDateTime = lastActionDateTime;
            this.Status = status;
            this.Locale = locale;
            this.RecordingsUrl = recordingsUrl;
            this.ResultsUrls = resultsUrls;
        }

        /// <inheritdoc />
        public string Name { get; set; }

        /// <inheritdoc />
        public string Description { get; set; }

        /// <inheritdoc />
        public string Locale { get; set; }

        /// <inheritdoc />
        public Uri RecordingsUrl { get; set; }

        /// <inheritdoc />
        public IReadOnlyDictionary<string, string> ResultsUrls { get; set; }

        public Guid Id { get; set; }

        /// <inheritdoc />
        public DateTime CreatedDateTime { get; set; }

        /// <inheritdoc />
        public DateTime LastActionDateTime { get; set; }

        /// <inheritdoc />
        public string Status { get; set; }

        public string StatusMessage { get; set; }
    }

    public sealed class TranscriptionDefinition
    {
        private TranscriptionDefinition(string name, string description, string locale, Uri recordingsUrl, IEnumerable<ModelIdentity> models)
        {
            this.Name = name;
            this.Description = description;
            this.RecordingsUrl = recordingsUrl;
            this.Locale = locale;
            this.Models = models;
            this.properties = new Dictionary<string, string>();
            this.properties.Add("PunctuationMode", "DictatedAndAutomatic");
            this.properties.Add("ProfanityFilterMode", "Masked");
            this.properties.Add("AddWordLevelTimestamps", "True");
        }

        /// <inheritdoc />
        public string Name { get; set; }

        /// <inheritdoc />
        public string Description { get; set; }

        /// <inheritdoc />
        public Uri RecordingsUrl { get; set; }

        public string Locale { get; set; }

        public IEnumerable<ModelIdentity> Models { get; set; }

        public IDictionary<string, string> properties { get; set; }

        public static TranscriptionDefinition Create(
            string name,
            string description,
            string locale,
            Uri recordingsUrl)
        {
            return TranscriptionDefinition.Create(name, description, locale, recordingsUrl, new ModelIdentity[0]);
        }

        public static TranscriptionDefinition Create(
            string name,
            string description,
            string locale,
            Uri recordingsUrl,
            IEnumerable<ModelIdentity> models)
        {
            return new TranscriptionDefinition(name, description, locale, recordingsUrl, models);
        }
    }

    public class AudioFileResult
    {
        public string AudioFileName { get; set; }
        public List<SegmentResult> SegmentResults { get; set; }
    }

    public class RootObject
    {
        public List<AudioFileResult> AudioFileResults { get; set; }
    }

    public class NBest
    {
        public double Confidence { get; set; }
        public string Lexical { get; set; }
        public string ITN { get; set; }
        public string MaskedITN { get; set; }
        public string Display { get; set; }
    }

    public class SegmentResult
    {
        public string RecognitionStatus { get; set; }
        public string Offset { get; set; }
        public string Duration { get; set; }
        public List<NBest> NBest { get; set; }
    }
}
```

## <a name="build-and-run-your-app"></a>生成并运行应用

现在，可以使用语音服务构建应用并测试语音识别。

1. “编译代码”- 在 Visual Studio 菜单栏中，选择“生成” > “生成解决方案”    。
2. **启动应用** - 在菜单栏中，选择“调试” > “开始调试”，或按 F5    。
3. **开始识别** - 它将提示你说英语短语。 语音将发送到语音服务，转录为文本，并在控制台中呈现。

## <a name="next-steps"></a>后续步骤

[!INCLUDE [footer](./footer.md)]
