---
title: 快速入门：适用于 Go 的计算机视觉客户端库
titleSuffix: Azure Cognitive Services
description: 通过本快速入门开始使用适用于 Go 的计算机视觉客户端库。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 06/08/2020
ms.author: v-tawe
origin.date: 01/27/2020
ms.openlocfilehash: b090bf991c12310ec239ed1737424d6498a72ab7
ms.sourcegitcommit: 8dae792aefbe44e8388f961b813e3da6564423ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84655008"
---
<a name="HOLTop"></a>

[参考文档](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/computervision) | [库源代码](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/computervision) | [包](https://github.com/Azure/azure-sdk-for-go)

## <a name="prerequisites"></a>先决条件

* Azure 订阅 - [创建试用订阅](https://wd.azure.cn/pricing/1rmb-trial-full)
* 最新版本的 Go<!--not available-->

## <a name="setting-up"></a>设置

### <a name="create-a-computer-vision-azure-resource"></a>创建计算机视觉 Azure 资源

Azure 认知服务由你订阅的 Azure 资源表示。 在本地计算机上使用 [Azure 门户](https://docs.azure.cn/cognitive-services/cognitive-services-apis-create-account)或 [Azure CLI](https://docs.azure.cn/cognitive-services/cognitive-services-apis-create-account-cli) 创建计算机视觉的资源。 也可执行以下操作：

<!-- trial key not avaiable -->
* 在 [Azure 门户](https://portal.azure.cn/)上查看资源。

获取试用订阅或资源的密钥后，请为该密钥和终结点 URL [创建环境变量](https://docs.azure.cn/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)，分别名为 `COMPUTER_VISION_SUBSCRIPTION_KEY` 和 `COMPUTER_VISION_ENDPOINT`。

### <a name="create-a-go-project-directory"></a>创建 Go 项目目录

在控制台窗口（cmd、PowerShell、终端、Bash）中，为 Go 项目创建一个名为 `my-app` 的新工作区并导航到该工作区。

```
mkdir -p my-app/{src, bin, pkg}  
cd my-app
```

工作区包含三个文件夹：

* **src** - 此目录包含源代码和包。 使用 `go get` 命令安装的任何包都将在此目录中。
* **pkg** - 此目录包含编译的 Go 包对象。 这些文件使用 `.a` 扩展名。
* **bin** - 此目录包含运行 `go install` 时创建的二进制可执行文件。

<!-- not available-->

### <a name="install-the-client-library-for-go"></a>安装适用于 Go 的客户端库

接下来，安装适用于 Go 的客户端库：

```bash
go get -u https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/computervision
```

或者，如果使用 dep，则在存储库中运行：

```bash
dep ensure -add https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.1/computervision
```

### <a name="create-a-go-application"></a>创建 Go 应用程序

接下来，在 src 目录中创建名为 `sample-app.go` 的文件****：

```bash
cd src
touch sample-app.go
```

在首选 IDE 或文本编辑器中打开 `sample-app.go`。 然后添加包名称并导入以下库：

```go
package main

import (
    "context"
    "encoding/json"
    "fmt"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.0/computervision"
    "github.com/Azure/go-autorest/autorest"
    "io"
    "log"
    "os"
    "strings"
    "time"
)
```

此外，还可以在脚本的根目录中声明一个上下文。 你将需要此对象来执行大多数计算机视觉函数调用：

```go
// Declare global so don't have to pass it to all of the tasks.
var computerVisionContext context.Context
```

接下来，将开始添加代码以执行不同的计算机视觉操作。

## <a name="object-model"></a>对象模型

以下类和接口将处理计算机视觉 Go SDK 的某些主要功能。

|名称|说明|
|---|---|
| [BaseClient](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/computervision#BaseClient) | 所有计算机视觉功能（如图像分析和文本阅读）都需要此类。 可以使用订阅信息实例化此类，然后使用它来执行大多数图像操作。|
|[ImageAnalysis](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/computervision#ImageAnalysis)| 此类型包含 AnalyzeImage 函数调用的结果****。 每个特定于类别的函数都具有相似的类型。|
|[ReadOperationResult](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/computervision#ReadOperationResult)| 此类型包含批读取操作的结果。 |
|[VisualFeatureTypes](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/computervision#VisualFeatureTypes)| 此类型定义了可在标准分析操作中完成的不同类型的图像分析。 请根据需求指定一组 VisualFeatureTypes 值。 |

## <a name="code-examples"></a>代码示例

这些代码片段演示如何使用适用于 Go 的计算机视觉客户端库执行以下任务：

* [对客户端进行身份验证](#authenticate-the-client)
* [分析图像](#analyze-an-image)
* [读取印刷体文本和手写文本](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>验证客户端

> [!NOTE]
> 此步骤假设已经为计算机视觉密钥和终结点（分别名为 `COMPUTER_VISION_SUBSCRIPTION_KEY` 和 `COMPUTER_VISION_ENDPOINT`）[创建了环境变量](https://docs.azure.cn/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)。

创建 `main` 函数，并向其添加以下代码，以使用终结点和密钥实例化客户端。

```go
/*  
 * Configure the Computer Vision client
 * Set environment variables for COMPUTER_VISION_SUBSCRIPTION_KEY and COMPUTER_VISION_ENDPOINT,
 * then restart your command shell or your IDE for changes to take effect.
 */
  computerVisionKey := os.Getenv("COMPUTER_VISION_SUBSCRIPTION_KEY")

if (computerVisionKey == "") {
    log.Fatal("\n\nPlease set a COMPUTER_VISION_SUBSCRIPTION_KEY environment variable.\n" +
                          "**You may need to restart your shell or IDE after it's set.**\n")
}

endpointURL := os.Getenv("COMPUTER_VISION_ENDPOINT")
if (endpointURL == "") {
    log.Fatal("\n\nPlease set a COMPUTER_VISION_ENDPOINT environment variable.\n" +
                          "**You may need to restart your shell or IDE after it's set.**")
}

computerVisionClient := computervision.New(endpointURL);
computerVisionClient.Authorizer = autorest.NewCognitiveServicesAuthorizer(computerVisionKey)

computerVisionContext = context.Background()
/*
 * END - Configure the Computer Vision client
 */
```

## <a name="analyze-an-image"></a>分析图像

以下代码使用客户端对象分析远程映像，并将结果打印到控制台。 可获取文本说明、分类、标记列表、检测到的物体、检测到的品牌、检测到的人脸、成人内容标志、主要颜色和图像类型。

### <a name="set-up-test-image"></a>设置测试图像

首先，保存对要分析的图像的 URL 的引用。 将其放入 `main` 函数。

```go
landmarkImageURL := "https://github.com/Azure-Samples/cognitive-services-sample-data-files/raw/master/ComputerVision/Images/landmark.jpg"
```

> [!NOTE]
> 还可以分析本地图像。 请参阅 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/ComputerVision/ComputerVisionQuickstart.go) 上的示例代码以了解涉及本地图像的方案。

### <a name="specify-visual-features"></a>指定视觉特性

以下函数调用从示例图像中提取不同的视觉特征。 将在以下各节中定义这些函数。

```go
// Analyze features of an image, remote
DescribeRemoteImage(computerVisionClient, landmarkImageURL)
CategorizeRemoteImage(computerVisionClient, landmarkImageURL)
TagRemoteImage(computerVisionClient, landmarkImageURL)
DetectFacesRemoteImage(computerVisionClient, facesImageURL)
DetectObjectsRemoteImage(computerVisionClient, objectsImageURL)
DetectBrandsRemoteImage(computerVisionClient, brandsImageURL)
DetectAdultOrRacyContentRemoteImage(computerVisionClient, adultRacyImageURL)
DetectColorSchemeRemoteImage(computerVisionClient, brandsImageURL)
DetectDomainSpecificContentRemoteImage(computerVisionClient, landmarkImageURL)
DetectImageTypesRemoteImage(computerVisionClient, detectTypeImageURL)
GenerateThumbnailRemoteImage(computerVisionClient, adultRacyImageURL)
```

### <a name="get-image-description"></a>获取图像说明

以下函数获取为图像生成的描述文字列表。 有关图像描述的详细信息，请参阅[描述图像](../../concept-describing-images.md)。

```go
func DescribeRemoteImage(client computervision.BaseClient, remoteImageURL string) {
    fmt.Println("-----------------------------------------")
    fmt.Println("DESCRIBE IMAGE - remote")
    fmt.Println()
    var remoteImage computervision.ImageURL
    remoteImage.URL = &remoteImageURL

    maxNumberDescriptionCandidates := new(int32)
    *maxNumberDescriptionCandidates = 1

    remoteImageDescription, err := client.DescribeImage(
            computerVisionContext,
            remoteImage,
            maxNumberDescriptionCandidates,
            "") // language
        if err != nil { log.Fatal(err) }

    fmt.Println("Captions from remote image: ")
    if len(*remoteImageDescription.Captions) == 0 {
        fmt.Println("No captions detected.")
    } else {
        for _, caption := range *remoteImageDescription.Captions {
            fmt.Printf("'%v' with confidence %.2f%%\n", *caption.Text, *caption.Confidence * 100)
        }
    }
    fmt.Println()
}
```

### <a name="get-image-category"></a>获取图像类别

以下函数获取已检测到的映像类别。 有关详细信息，请参阅[对图像进行分类](../../concept-categorizing-images.md)。

```go
func CategorizeRemoteImage(client computervision.BaseClient, remoteImageURL string) {
    fmt.Println("-----------------------------------------")
    fmt.Println("CATEGORIZE IMAGE - remote")
    fmt.Println()
    var remoteImage computervision.ImageURL
    remoteImage.URL = &remoteImageURL

    features := []computervision.VisualFeatureTypes{computervision.VisualFeatureTypesCategories}
    imageAnalysis, err := client.AnalyzeImage(
            computerVisionContext,
            remoteImage,
            features,
            []computervision.Details{},
            "")
    if err != nil { log.Fatal(err) }

    fmt.Println("Categories from remote image: ")
    if len(*imageAnalysis.Categories) == 0 {
        fmt.Println("No categories detected.")
    } else {
        for _, category := range *imageAnalysis.Categories {
            fmt.Printf("'%v' with confidence %.2f%%\n", *category.Name, *category.Score * 100)
        }
    }
    fmt.Println()
}
```

### <a name="get-image-tags"></a>获取图像标记

以下函数获取映像中检测到的标记集。 有关详细信息，请参阅[内容标记](../../concept-tagging-images.md)。

```go
func TagRemoteImage(client computervision.BaseClient, remoteImageURL string) {
    fmt.Println("-----------------------------------------")
    fmt.Println("TAG IMAGE - remote")
    fmt.Println()
    var remoteImage computervision.ImageURL
    remoteImage.URL = &remoteImageURL

    remoteImageTags, err := client.TagImage(
            computerVisionContext,
            remoteImage,
            "")
    if err != nil { log.Fatal(err) }

    fmt.Println("Tags in the remote image: ")
    if len(*remoteImageTags.Tags) == 0 {
        fmt.Println("No tags detected.")
    } else {
        for _, tag := range *remoteImageTags.Tags {
            fmt.Printf("'%v' with confidence %.2f%%\n", *tag.Name, *tag.Confidence * 100)
        }
    }
    fmt.Println()
}
```

### <a name="detect-objects"></a>检测物体

以下函数检测图像中的常见物体并将其打印到控制台。 有关详细信息，请参阅[物体检测](../../concept-object-detection.md)。

```go
func DetectObjectsRemoteImage(client computervision.BaseClient, remoteImageURL string) {
    fmt.Println("-----------------------------------------")
    fmt.Println("DETECT OBJECTS - remote")
    fmt.Println()
    var remoteImage computervision.ImageURL
    remoteImage.URL = &remoteImageURL

    imageAnalysis, err := client.DetectObjects(
            computerVisionContext,
            remoteImage,
    )
    if err != nil { log.Fatal(err) }

    fmt.Println("Detecting objects in remote image: ")
    if len(*imageAnalysis.Objects) == 0 {
        fmt.Println("No objects detected.")
    } else {
        // Print the objects found with confidence level and bounding box locations.
        for _, object := range *imageAnalysis.Objects {
            fmt.Printf("'%v' with confidence %.2f%% at location (%v, %v), (%v, %v)\n",
                *object.Object, *object.Confidence * 100,
                *object.Rectangle.X, *object.Rectangle.X + *object.Rectangle.W,
                *object.Rectangle.Y, *object.Rectangle.Y + *object.Rectangle.H)
        }
    }
    fmt.Println()
}
```

### <a name="detect-brands"></a>检测品牌

以下代码检测图像中的公司品牌和徽标，并将其输出到控制台。 有关详细信息，请参阅[品牌检测](../../concept-brand-detection.md)。

首先，在 `main` 函数内声明对新图像的引用。

```go
brandsImageURL := "https://docs.microsoft.com/azure/cognitive-services/computer-vision/images/gray-shirt-logo.jpg"
```

以下代码定义品牌检测函数。

```go
func DetectBrandsRemoteImage(client computervision.BaseClient, remoteImageURL string) {
    fmt.Println("-----------------------------------------")
    fmt.Println("DETECT BRANDS - remote")
    fmt.Println()
    var remoteImage computervision.ImageURL
    remoteImage.URL = &remoteImageURL

    // Define the kinds of features you want returned.
    features := []computervision.VisualFeatureTypes{computervision.VisualFeatureTypesBrands}

    imageAnalysis, err := client.AnalyzeImage(
        computerVisionContext,
        remoteImage,
        features,
        []computervision.Details{},
        "en")
    if err != nil { log.Fatal(err) }

    fmt.Println("Detecting brands in remote image: ")
    if len(*imageAnalysis.Brands) == 0 {
        fmt.Println("No brands detected.")
    } else {
        // Get bounding box around the brand and confidence level it's correctly identified.
        for _, brand := range *imageAnalysis.Brands {
            fmt.Printf("'%v' with confidence %.2f%% at location (%v, %v), (%v, %v)\n",
                *brand.Name, *brand.Confidence * 100,
                *brand.Rectangle.X, *brand.Rectangle.X + *brand.Rectangle.W,
                *brand.Rectangle.Y, *brand.Rectangle.Y + *brand.Rectangle.H)
        }
    }
    fmt.Println()
}
```

### <a name="detect-faces"></a>检测人脸

以下函数返回图像中检测到的人脸及其矩形坐标，以及某些人脸属性。 有关详细信息，请参阅[人脸检测](../../concept-detecting-faces.md)。

```go
func DetectFacesRemoteImage(client computervision.BaseClient, remoteImageURL string) {
    fmt.Println("-----------------------------------------")
    fmt.Println("DETECT FACES - remote")
    fmt.Println()
    var remoteImage computervision.ImageURL
    remoteImage.URL = &remoteImageURL

    // Define the features you want returned with the API call.
    features := []computervision.VisualFeatureTypes{computervision.VisualFeatureTypesFaces}
    imageAnalysis, err := client.AnalyzeImage(
            computerVisionContext,
            remoteImage,
            features,
            []computervision.Details{},
            "")
        if err != nil { log.Fatal(err) }

    fmt.Println("Detecting faces in a remote image ...")
    if len(*imageAnalysis.Faces) == 0 {
        fmt.Println("No faces detected.")
    } else {
        // Print the bounding box locations of the found faces.
        for _, face := range *imageAnalysis.Faces {
            fmt.Printf("'%v' of age %v at location (%v, %v), (%v, %v)\n",
                face.Gender, *face.Age,
                *face.FaceRectangle.Left, *face.FaceRectangle.Top,
                *face.FaceRectangle.Left + *face.FaceRectangle.Width,
                *face.FaceRectangle.Top + *face.FaceRectangle.Height)
        }
    }
    fmt.Println()
}
```

### <a name="detect-adult-racy-or-gory-content"></a>检测成人、色情或血腥内容

以下函数打印图像中检测到的成人内容。 有关详细信息，请参阅[成人、色情或血腥内容](../../concept-detecting-adult-content.md)。

```go
func DetectAdultOrRacyContentRemoteImage(client computervision.BaseClient, remoteImageURL string) {
    fmt.Println("-----------------------------------------")
    fmt.Println("DETECT ADULT OR RACY CONTENT - remote")
    fmt.Println()
    var remoteImage computervision.ImageURL
    remoteImage.URL = &remoteImageURL

    // Define the features you want returned from the API call.
    features := []computervision.VisualFeatureTypes{computervision.VisualFeatureTypesAdult}
    imageAnalysis, err := client.AnalyzeImage(
            computerVisionContext,
            remoteImage,
            features,
            []computervision.Details{},
            "") // language, English is default
    if err != nil { log.Fatal(err) }

    // Print whether or not there is questionable content.
    // Confidence levels: low means content is OK, high means it's not.
    fmt.Println("Analyzing remote image for adult or racy content: ");
    fmt.Printf("Is adult content: %v with confidence %.2f%%\n", *imageAnalysis.Adult.IsAdultContent, *imageAnalysis.Adult.AdultScore * 100)
    fmt.Printf("Has racy content: %v with confidence %.2f%%\n", *imageAnalysis.Adult.IsRacyContent, *imageAnalysis.Adult.RacyScore * 100)
    fmt.Println()
}
```

### <a name="get-image-color-scheme"></a>获取图像配色方案

以下函数打印图像中检测到的颜色属性，如主色和主题色。 有关详细信息，请参阅[配色方案](../../concept-detecting-color-schemes.md)。

```go
func DetectColorSchemeRemoteImage(client computervision.BaseClient, remoteImageURL string) {
    fmt.Println("-----------------------------------------")
    fmt.Println("DETECT COLOR SCHEME - remote")
    fmt.Println()
    var remoteImage computervision.ImageURL
    remoteImage.URL = &remoteImageURL

    // Define the features you'd like returned with the result.
    features := []computervision.VisualFeatureTypes{computervision.VisualFeatureTypesColor}
    imageAnalysis, err := client.AnalyzeImage(
            computerVisionContext,
            remoteImage,
            features,
            []computervision.Details{},
            "") // language, English is default
    if err != nil { log.Fatal(err) }

    fmt.Println("Color scheme of the remote image: ");
    fmt.Printf("Is black and white: %v\n", *imageAnalysis.Color.IsBWImg)
    fmt.Printf("Accent color: 0x%v\n", *imageAnalysis.Color.AccentColor)
    fmt.Printf("Dominant background color: %v\n", *imageAnalysis.Color.DominantColorBackground)
    fmt.Printf("Dominant foreground color: %v\n", *imageAnalysis.Color.DominantColorForeground)
    fmt.Printf("Dominant colors: %v\n", strings.Join(*imageAnalysis.Color.DominantColors, ", "))
    fmt.Println()
}
```

### <a name="get-domain-specific-content"></a>获取特定于域的内容

计算机视觉可以使用专用模型对图像进行进一步分析。 有关详细信息，请参阅[特定于域的内容](../../concept-detecting-domain-content.md)。 

以下代码分析了图像中检测到的名人的相关数据。

```go
func DetectDomainSpecificContentRemoteImage(client computervision.BaseClient, remoteImageURL string) {
    fmt.Println("-----------------------------------------")
    fmt.Println("DETECT DOMAIN-SPECIFIC CONTENT - remote")
    fmt.Println()
    var remoteImage computervision.ImageURL
    remoteImage.URL = &remoteImageURL

    fmt.Println("Detecting domain-specific content in the local image ...")

    // Check if there are any celebrities in the image.
    celebrities, err := client.AnalyzeImageByDomain(
            computerVisionContext,
            "celebrities",
            remoteImage,
            "") // language, English is default
    if err != nil { log.Fatal(err) }

    fmt.Println("\nCelebrities: ")

    // Marshal the output from AnalyzeImageByDomain into JSON.
    data, err := json.MarshalIndent(celebrities.Result, "", "\t")

    // Define structs for which to unmarshal the JSON.
    type Celebrities struct {
        Name string `json:"name"`
    }

    type CelebrityResult struct {
        Celebrities []Celebrities `json:"celebrities"`
    }

    var celebrityResult CelebrityResult

    // Unmarshal the data.
    err = json.Unmarshal(data, &celebrityResult)
    if err != nil { log.Fatal(err) }

    //  Check if any celebrities detected.
    if len(celebrityResult.Celebrities) == 0 {
        fmt.Println("No celebrities detected.")
    }   else {
        for _, celebrity := range celebrityResult.Celebrities {
            fmt.Printf("name: %v\n", celebrity.Name)
        }
    }
```

以下代码分析了图像中检测到的地标的相关数据。

```
fmt.Println("\nLandmarks: ")

    // Check if there are any landmarks in the image.
    landmarks, err := client.AnalyzeImageByDomain(
            computerVisionContext,
            "landmarks",
            remoteImage,
            "")
    if err != nil { log.Fatal(err) }

    // Marshal the output from AnalyzeImageByDomain into JSON.
    data, err = json.MarshalIndent(landmarks.Result, "", "\t")

    // Define structs for which to unmarshal the JSON.
    type Landmarks struct {
        Name string `json:"name"`
    }

    type LandmarkResult struct {
        Landmarks   []Landmarks `json:"landmarks"`
    }

    var landmarkResult LandmarkResult

    // Unmarshal the data.
    err = json.Unmarshal(data, &landmarkResult)
    if err != nil { log.Fatal(err) }

    // Check if any celebrities detected.
    if len(landmarkResult.Landmarks) == 0 {
        fmt.Println("No landmarks detected.")
    }   else {
        for _, landmark := range landmarkResult.Landmarks {
            fmt.Printf("name: %v\n", landmark.Name)
        }
    }
    fmt.Println()
}
```

### <a name="get-the-image-type"></a>获取图像类型

以下函数将打印有关图像类型的信息，即表明它是剪贴画还是素描。

```go
func DetectImageTypesRemoteImage(client computervision.BaseClient, remoteImageURL string) {
    fmt.Println("-----------------------------------------")
    fmt.Println("DETECT IMAGE TYPES - remote")
    fmt.Println()
    var remoteImage computervision.ImageURL
    remoteImage.URL = &remoteImageURL

    features := []computervision.VisualFeatureTypes{computervision.VisualFeatureTypesImageType}

    imageAnalysis, err := client.AnalyzeImage(
            computerVisionContext,
            remoteImage,
            features,
            []computervision.Details{},
            "")
    if err != nil { log.Fatal(err) }

    fmt.Println("Image type of remote image:")

    fmt.Println("\nClip art type: ")
    switch *imageAnalysis.ImageType.ClipArtType {
    case 0:
        fmt.Println("Image is not clip art.")
    case 1:
        fmt.Println("Image is ambiguously clip art.")
    case 2:
        fmt.Println("Image is normal clip art.")
    case 3:
        fmt.Println("Image is good clip art.")
    }

    fmt.Println("\nLine drawing type: ")
    if *imageAnalysis.ImageType.LineDrawingType == 1 {
        fmt.Println("Image is a line drawing.")
    }   else {
        fmt.Println("Image is not a line drawing.")
    }
    fmt.Println()
}
```

## <a name="read-printed-and-handwritten-text"></a>读取印刷体文本和手写文本

计算机视觉可以读取图像中的可见文本，并将其转换为字符流。 本部分的代码定义了函数 `RecognizeTextReadAPIRemoteImage`，该函数使用客户端对象检测并提取图像中的印刷体文本或手写文本。

在 `main` 函数中添加示例图像引用和函数调用。

```go
// Analyze text in an image, remote
BatchReadFileRemoteImage(computerVisionClient, printedImageURL)
```

> [!NOTE]
> 还可以从本地图像提取文本。 请参阅 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/ComputerVision/ComputerVisionQuickstart.go) 上的示例代码以了解涉及本地图像的方案。

### <a name="call-the-read-api"></a>调用读取 API

定义用于读取文本的新函数 `RecognizeTextReadAPIRemoteImage`。 添加以下代码，该代码对给定图像调用 BatchReadFile 方法****。 此方法返回一个操作 ID 并启动异步进程来读取图像的内容。

```go
func BatchReadFileRemoteImage(client computervision.BaseClient, remoteImageURL string) {
    fmt.Println("-----------------------------------------")
    fmt.Println("BATCH READ FILE - remote")
    fmt.Println()
    var remoteImage computervision.ImageURL
    remoteImage.URL = &remoteImageURL

    // The response contains a field called "Operation-Location", 
    // which is a URL with an ID that you'll use for GetReadOperationResult to access OCR results.
    textHeaders, err := client.BatchReadFile(computerVisionContext, remoteImage)
    if err != nil { log.Fatal(err) }

    // Use ExtractHeader from the autorest library to get the Operation-Location URL
    operationLocation := autorest.ExtractHeaderValue("Operation-Location", textHeaders.Response)

    numberOfCharsInOperationId := 36
    operationId := string(operationLocation[len(operationLocation)-numberOfCharsInOperationId : len(operationLocation)])
```

### <a name="get-read-results"></a>获取读取结果

接下来，获取 BatchReadFile 调用返回的操作 ID，并将其用于 GetReadOperationResult 方法，向服务查询操作结果**** ****。 下面的代码每隔一秒钟检查一次操作，直到返回结果。 然后，它将提取的文本数据输出到控制台。

```go
readOperationResult, err := client.GetReadOperationResult(computerVisionContext, operationId)
if err != nil { log.Fatal(err) }

// Wait for the operation to complete.
i := 0
maxRetries := 10

fmt.Println("Recognizing text in a remote image with the batch Read API ...")
for readOperationResult.Status != computervision.Failed &&
        readOperationResult.Status != computervision.Succeeded {
    if i >= maxRetries {
        break
    }
    i++

    fmt.Printf("Server status: %v, waiting %v seconds...\n", readOperationResult.Status, i)
    time.Sleep(1 * time.Second)

    readOperationResult, err = client.GetReadOperationResult(computerVisionContext, operationId)
    if err != nil { log.Fatal(err) }
}
```

### <a name="display-read-results"></a>显示读取结果

添加以下代码来分析和显示检索到的文本数据，并完成函数定义。

```go
// Display the results.
fmt.Println()
for _, recResult := range *(readOperationResult.RecognitionResults) {
    for _, line := range *recResult.Lines {
        fmt.Println(*line.Text)
    }
}
```

## <a name="run-the-application"></a>运行应用程序

从应用程序目录使用 `go run` 命令运行应用程序。

```bash
go run sample-app.go
```

## <a name="clean-up-resources"></a>清理资源

如果想要清理并删除认知服务订阅，可以删除资源或资源组。 删除资源组同时也会删除与之相关联的任何其他资源。

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [计算机视觉 API 参考 (Go)](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.1/computervision)

* [什么是计算机视觉？](../../Home.md)
* 可以在 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/ComputerVision/ComputerVisionQuickstart.go) 上找到此示例的源代码。
