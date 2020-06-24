---
title: 快速入门：适用于 .NET 的计算机视觉客户端库
description: 在本快速入门中，开始使用适用于 .NET 的计算机视觉客户端库。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 06/08/2020
ms.author: v-tawe
origin.date: 12/05/2019
ms.openlocfilehash: fe6e815f3903489ab3c98f5bbbf8f56682d088b2
ms.sourcegitcommit: 8dae792aefbe44e8388f961b813e3da6564423ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84654990"
---
<a name="HOLTop"></a>

[参考文档](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/computervision?view=azure-dotnet) | [库源代码](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.ComputerVision) | [包 (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) | [示例](https://docs.microsoft.com/samples/browse/?products=azure)

## <a name="prerequisites"></a>先决条件

* Azure 订阅 - [创建试用订阅](https://wd.azure.cn/pricing/1rmb-trial-full)
* 最新版本的 [.NET Core SDK](https://dotnet.microsoft.com/download/)。

## <a name="setting-up"></a>设置

### <a name="create-a-computer-vision-azure-resource"></a>创建计算机视觉 Azure 资源

Azure 认知服务由你订阅的 Azure 资源表示。 在本地计算机上使用 [Azure 门户](https://docs.azure.cn/cognitive-services/cognitive-services-apis-create-account)或 [Azure CLI](https://docs.azure.cn/cognitive-services/cognitive-services-apis-create-account-cli) 创建计算机视觉的资源。 也可执行以下操作：

<!--trial key-->
* 在 [Azure 门户](https://portal.azure.cn/)上查看资源。

获取试用订阅或资源的密钥后，请为该密钥和终结点 URL [创建环境变量](https://docs.azure.cn/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)，分别名为 `COMPUTER_VISION_SUBSCRIPTION_KEY` 和 `COMPUTER_VISION_ENDPOINT`。

### <a name="create-a-new-c-application"></a>新建 C# 应用程序

在首选编辑器或 IDE 中创建新的 .NET Core 应用程序。 

在控制台窗口（例如 cmd、PowerShell 或 Bash）中，使用 `dotnet new` 命令创建名为 `computer-vision-quickstart` 的新控制台应用。 此命令将创建包含单个源文件的简单“Hello World”C# 项目：ComputerVisionQuickstart.cs。

```dotnetcli
dotnet new console -n computer-vision-quickstart
```

将目录更改为新创建的应用文件夹。 可使用以下代码生成应用程序：

```dotnetcli
dotnet build
```

生成输出不应包含警告或错误。 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

在你喜欢使用的编辑器或 IDE 中，打开项目目录中的 ComputerVisionQuickstart.cs 文件。 然后，添加以下 `using` 指令：

```csharp
using System;
using System.Collections.Generic;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;
using System.Threading.Tasks;
using System.IO;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```

在应用程序的 **Program** 类中，为资源的 Azure 终结点和密钥创建变量。

```csharp
// Add your Computer Vision subscription key and endpoint to your environment variables. 
// Close/reopen your project for them to take effect.
static string subscriptionKey = Environment.GetEnvironmentVariable("COMPUTER_VISION_SUBSCRIPTION_KEY");
static string endpoint = Environment.GetEnvironmentVariable("COMPUTER_VISION_ENDPOINT");
```

### <a name="install-the-client-library"></a>安装客户端库

在应用程序目录中，使用以下命令安装适用于 .NET 的计算机视觉客户端库：

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Vision.ComputerVision --version 5.0.0
```

如果你使用的是 Visual Studio IDE，客户端库可用作可下载的 NuGet 包。

## <a name="object-model"></a>对象模型

以下类和接口将处理计算机视觉 .NET SDK 的某些主要功能。

|名称|说明|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-dotnet) | 所有计算机视觉功能都需要此类。 可以使用订阅信息实例化此类，然后使用它来执行大多数图像操作。|
|[ComputerVisionClientExtensions](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclientextensions?view=azure-dotnet)| 此类包含 ComputerVisionClient 的其他方法。|
|[VisualFeatureTypes](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-dotnet)| 此枚举定义可在标准分析操作中执行的不同类型的图像分析。 请根据需求指定一组 VisualFeatureTypes 值。 |

## <a name="code-examples"></a>代码示例

这些代码片段演示如何使用适用于 .NET 的计算机视觉客户端库执行以下任务：

* [对客户端进行身份验证](#authenticate-the-client)
* [分析图像](#analyze-an-image)
* [读取印刷体文本和手写文本](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>验证客户端

> [!NOTE]
> 本快速入门假设已经为计算机视觉密钥和终结点（分别名为 `COMPUTER_VISION_SUBSCRIPTION_KEY` 和 `COMPUTER_VISION_ENDPOINT`）[创建了环境变量](/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)。

在新方法中，使用终结点和密钥实例化客户端。 使用密钥创建一个 **[ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.apikeyserviceclientcredentials?view=azure-dotnet)** 对象，并在终结点中使用该对象创建一个 **[ComputerVisionClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-dotnet)** 对象。

```csharp
/*
 * AUTHENTICATE
 * Creates a Computer Vision client used by each example.
 */
public static ComputerVisionClient Authenticate(string endpoint, string key)
{
    ComputerVisionClient client =
      new ComputerVisionClient(new ApiKeyServiceClientCredentials(key))
      { Endpoint = endpoint };
    return client;
}
```

你可能想要在 `Main` 方法中调用此方法。

```csharp
// Create a client
ComputerVisionClient client = Authenticate(endpoint, subscriptionKey);
```

## <a name="analyze-an-image"></a>分析图像

以下代码定义方法 `AnalyzeImageUrl`，该方法使用客户端对象分析远程图像并输出结果。 该方法返回文本说明、分类、标记列表、检测到的人脸、成人内容标志、主颜色和图像类型。

在 `Main` 方法中添加方法调用。

```csharp
// Analyze an image to get features and other properties.
AnalyzeImageUrl(client, ANALYZE_URL_IMAGE).Wait();
```

### <a name="set-up-test-image"></a>设置测试图像

在 Program 类中，保存对要分析的图像的 URL 的引用。

```csharp
// URL image used for analyzing an image (image of puppy)
private const string ANALYZE_URL_IMAGE = "https://moderatorsampleimages.blob.core.windows.net/samples/sample16.png";
```

> [!NOTE]
> 还可以分析本地图像。 请参阅 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs) 上的示例代码以了解涉及本地图像的方案。

### <a name="specify-visual-features"></a>指定视觉特性

定义新的图像分析方法。 添加下面的代码，它指定要在分析中提取的视觉特征。 有关完整列表，请参阅 **[VisualFeatureTypes](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-dotnet)** 枚举。

```csharp
/* 
 * ANALYZE IMAGE - URL IMAGE
 * Analyze URL image. Extracts captions, categories, tags, objects, faces, racy/adult content,
 * brands, celebrities, landmarks, color scheme, and image types.
 */
public static async Task AnalyzeImageUrl(ComputerVisionClient client, string imageUrl)
{
    Console.WriteLine("----------------------------------------------------------");
    Console.WriteLine("ANALYZE IMAGE - URL");
    Console.WriteLine();

    // Creating a list that defines the features to be extracted from the image. 
    List<VisualFeatureTypes> features = new List<VisualFeatureTypes>()
{
  VisualFeatureTypes.Categories, VisualFeatureTypes.Description,
  VisualFeatureTypes.Faces, VisualFeatureTypes.ImageType,
  VisualFeatureTypes.Tags, VisualFeatureTypes.Adult,
  VisualFeatureTypes.Color, VisualFeatureTypes.Brands,
  VisualFeatureTypes.Objects
};
```

将以下任一代码块插入 AnalyzeImageUrl 方法中以实现其功能。 请记得在末尾添加右括号。

```csharp
}
```

### <a name="analyze"></a>分析

**AnalyzeImageAsync** 方法将返回包含所有提取信息的 **ImageAnalysis** 对象。

```csharp
Console.WriteLine($"Analyzing the image {Path.GetFileName(imageUrl)}...");
Console.WriteLine();
// Analyze the URL image 
ImageAnalysis results = await client.AnalyzeImageAsync(imageUrl, features);
```

以下部分说明如何详细分析此信息。

### <a name="get-image-description"></a>获取图像说明

下面的代码获取为图像生成的描述文字列表。 有关更多详细信息，请参阅[描述图像](../../concept-describing-images.md)。

```csharp
// Sunmarizes the image content.
Console.WriteLine("Summary:");
foreach (var caption in results.Description.Captions)
{
    Console.WriteLine($"{caption.Text} with confidence {caption.Confidence}");
}
Console.WriteLine();
```

### <a name="get-image-category"></a>获取图像类别

下面的代码获取所检测到的图像类别。 有关更多详细信息，请参阅[对图像进行分类](../../concept-categorizing-images.md)。

```csharp
// Display categories the image is divided into.
Console.WriteLine("Categories:");
foreach (var category in results.Categories)
{
    Console.WriteLine($"{category.Name} with confidence {category.Score}");
}
Console.WriteLine();
```

### <a name="get-image-tags"></a>获取图像标记

以下代码获取图像中检测到的标记集。 有关更多详细信息，请参阅[内容标记](../../concept-tagging-images.md)。

```csharp
// Image tags and their confidence score
Console.WriteLine("Tags:");
foreach (var tag in results.Tags)
{
    Console.WriteLine($"{tag.Name} {tag.Confidence}");
}
Console.WriteLine();
```

### <a name="detect-objects"></a>检测物体

以下代码检测图像中的常见物体并将其输出到控制台。 有关更多详细信息，请参阅[物体检测](../../concept-object-detection.md)。

```csharp
// Objects
Console.WriteLine("Objects:");
foreach (var obj in results.Objects)
{
    Console.WriteLine($"{obj.ObjectProperty} with confidence {obj.Confidence} at location {obj.Rectangle.X}, " +
      $"{obj.Rectangle.X + obj.Rectangle.W}, {obj.Rectangle.Y}, {obj.Rectangle.Y + obj.Rectangle.H}");
}
Console.WriteLine();
```

### <a name="detect-brands"></a>检测品牌

以下代码检测图像中的公司品牌和徽标，并将其输出到控制台。 有关更多详细信息，请参阅[品牌检测](../../concept-brand-detection.md)。

```csharp
// Well-known (or custom, if set) brands.
Console.WriteLine("Brands:");
foreach (var brand in results.Brands)
{
    Console.WriteLine($"Logo of {brand.Name} with confidence {brand.Confidence} at location {brand.Rectangle.X}, " +
      $"{brand.Rectangle.X + brand.Rectangle.W}, {brand.Rectangle.Y}, {brand.Rectangle.Y + brand.Rectangle.H}");
}
Console.WriteLine();
```

### <a name="detect-faces"></a>检测人脸

下面的代码返回图像中检测到的人脸及其矩形坐标，以及选择面属性。 有关更多详细信息，请参阅[人脸检测](../../concept-detecting-faces.md)。

```csharp
// Faces
Console.WriteLine("Faces:");
foreach (var face in results.Faces)
{
    Console.WriteLine($"A {face.Gender} of age {face.Age} at location {face.FaceRectangle.Left}, " +
      $"{face.FaceRectangle.Left}, {face.FaceRectangle.Top + face.FaceRectangle.Width}, " +
      $"{face.FaceRectangle.Top + face.FaceRectangle.Height}");
}
Console.WriteLine();
``

### Detect adult, racy, or gory content

The following code prints the detected presence of adult content in the image. See [Adult, racy, gory content](../../concept-detecting-adult-content.md) for more details.

```csharp
// Adult or racy content, if any.
Console.WriteLine("Adult:");
Console.WriteLine($"Has adult content: {results.Adult.IsAdultContent} with confidence {results.Adult.AdultScore}");
Console.WriteLine($"Has racy content: {results.Adult.IsRacyContent} with confidence {results.Adult.RacyScore}");
Console.WriteLine();
```

### <a name="get-image-color-scheme"></a>获取图像配色方案

以下代码输出图像中检测到的颜色属性，如主色和主题色。 有关更多详细信息，请参阅[配色方案](../../concept-detecting-color-schemes.md)。

```csharp
// Identifies the color scheme.
Console.WriteLine("Color Scheme:");
Console.WriteLine("Is black and white?: " + results.Color.IsBWImg);
Console.WriteLine("Accent color: " + results.Color.AccentColor);
Console.WriteLine("Dominant background color: " + results.Color.DominantColorBackground);
Console.WriteLine("Dominant foreground color: " + results.Color.DominantColorForeground);
Console.WriteLine("Dominant colors: " + string.Join(",", results.Color.DominantColors));
Console.WriteLine();
```

### <a name="get-domain-specific-content"></a>获取特定于域的内容

计算机视觉可以使用专用模型对图像进行进一步分析。 有关更多详细信息，请参阅[特定于域的内容](../../concept-detecting-domain-content.md)。 

以下代码分析了图像中检测到的名人的相关数据。

```csharp
// Celebrities in image, if any.
Console.WriteLine("Celebrities:");
foreach (var category in results.Categories)
{
    if (category.Detail?.Celebrities != null)
    {
        foreach (var celeb in category.Detail.Celebrities)
        {
            Console.WriteLine($"{celeb.Name} with confidence {celeb.Confidence} at location {celeb.FaceRectangle.Left}, " +
              $"{celeb.FaceRectangle.Top}, {celeb.FaceRectangle.Height}, {celeb.FaceRectangle.Width}");
        }
    }
}
Console.WriteLine();
```

以下代码分析了图像中检测到的地标的相关数据。

```csharp
// Popular landmarks in image, if any.
Console.WriteLine("Landmarks:");
foreach (var category in results.Categories)
{
    if (category.Detail?.Landmarks != null)
    {
        foreach (var landmark in category.Detail.Landmarks)
        {
            Console.WriteLine($"{landmark.Name} with confidence {landmark.Confidence}");
        }
    }
}
Console.WriteLine();
```

### <a name="get-the-image-type"></a>获取图像类型

以下代码输出有关图像类型的信息&mdash;无论它是剪贴画还是线条图。

```csharp
// Detects the image types.
Console.WriteLine("Image Type:");
Console.WriteLine("Clip Art Type: " + results.ImageType.ClipArtType);
Console.WriteLine("Line Drawing Type: " + results.ImageType.LineDrawingType);
Console.WriteLine();
```

## <a name="read-printed-and-handwritten-text"></a>读取印刷体文本和手写文本

计算机视觉可以读取图像中的可见文本，并将其转换为字符流。 本部分中的代码定义了方法 `ExtractTextUrl`，该方法使用客户端对象检测并提取图像中的印刷体文本或手写文本。

在 `Main` 方法中添加方法调用。

```csharp
// Read the batch text from an image (handwriting and/or printed).
BatchReadFileUrl(client, EXTRACT_TEXT_URL_IMAGE).Wait();
BatchReadFileLocal(client, EXTRACT_TEXT_LOCAL_IMAGE).Wait();
```

### <a name="set-up-test-image"></a>设置测试图像

在 **Program** 类中，保存要从中提取文本的图像的 URL 的引用。

```csharp
private const string EXTRACT_TEXT_URL_IMAGE = "https://raw.githubusercontent.com/MicrosoftDocs/azure-docs/master/articles/cognitive-services/Computer-vision/Images/readsample.jpg";
// URL image for OCR (optical character recognition).
```

> [!NOTE]
> 还可以从本地图像提取文本。 请参阅 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs) 上的示例代码以了解涉及本地图像的方案。

### <a name="call-the-read-api"></a>调用读取 API

定义用于读取文本的新方法。 添加以下代码，该代码对给定图像调用 **BatchReadFileAsync** 方法。 这会返回一个操作 ID 并启动异步进程来读取图像的内容。

```csharp
/*
 * BATCH READ FILE - URL IMAGE
 * Recognizes handwritten text. 
 * This API call offers an improvement of results over the Recognize Text calls.
 */
public static async Task BatchReadFileUrl(ComputerVisionClient client, string urlImage)
{
    Console.WriteLine("----------------------------------------------------------");
    Console.WriteLine("BATCH READ FILE - URL IMAGE");
    Console.WriteLine();

    // Read text from URL
    BatchReadFileHeaders textHeaders = await client.BatchReadFileAsync(urlImage);
    // After the request, get the operation location (operation ID)
    string operationLocation = textHeaders.OperationLocation;
```

### <a name="get-read-results"></a>获取读取结果

接下来，获取从 **BatchReadFileAsync** 调用返回的操作 ID，并使用它查询服务以获取操作结果。 下面的代码每隔一秒钟检查一次操作，直到返回结果。 然后，它将提取的文本数据输出到控制台。

```csharp
// Retrieve the URI where the recognized text will be stored from the Operation-Location header.
    // We only need the ID and not the full URL
    const int numberOfCharsInOperationId = 36;
    string operationId = operationLocation.Substring(operationLocation.Length - numberOfCharsInOperationId);

    // Extract the text
    // Delay is between iterations and tries a maximum of 10 times.
    int i = 0;
    int maxRetries = 10;
    ReadOperationResult results;
    Console.WriteLine($"Extracting text from URL image {Path.GetFileName(urlImage)}...");
    Console.WriteLine();
    do
    {
        results = await client.GetReadOperationResultAsync(operationId);
        Console.WriteLine("Server status: {0}, waiting {1} seconds...", results.Status, i);
        await Task.Delay(1000);
        if (i == 9) 
{ 
    Console.WriteLine("Server timed out."); 
}
    }
    while ((results.Status == TextOperationStatusCodes.Running ||
        results.Status == TextOperationStatusCodes.NotStarted) && i++ < maxRetries);
```

### <a name="display-read-results"></a>显示读取结果

添加以下代码来分析和显示检索到的文本数据，并完成方法定义。

```csharp
// Display the found text.
    Console.WriteLine();
    var textRecognitionLocalFileResults = results.RecognitionResults;
    foreach (TextRecognitionResult recResult in textRecognitionLocalFileResults)
    {
        foreach (Line line in recResult.Lines)
        {
            Console.WriteLine(line.Text);
        }
    }
    Console.WriteLine();
}
```

## <a name="run-the-application"></a>运行应用程序

从应用程序目录使用 `dotnet run` 命令运行应用程序。

```dotnetcli
dotnet run
```

## <a name="clean-up-resources"></a>清理资源

如果想要清理并删除认知服务订阅，可以删除资源或资源组。 删除资源组同时也会删除与之相关联的任何其他资源。

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
>[计算机视觉 API 参考 (.NET)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/computervision?view=azure-dotnet)

* [什么是计算机视觉？](../../Home.md)
* 可以在 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/ComputerVision/ComputerVisionQuickstart.cs) 上找到此示例的源代码。
