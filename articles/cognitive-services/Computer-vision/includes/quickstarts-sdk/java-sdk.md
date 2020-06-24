---
title: 快速入门：适用于 Java 的计算机视觉客户端库
description: 在本快速入门中，开始使用适用于 Java 的计算机视觉客户端库。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 06/08/2020
ms.author: v-tawe
origin.date: 12/19/2019
ms.openlocfilehash: 107c381778b905030caafd192da70707d7ea103e
ms.sourcegitcommit: 8dae792aefbe44e8388f961b813e3da6564423ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84655005"
---
<a name="HOLTop"></a>

[参考文档](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/computervision?view=azure-java-stable) | [项目 (Maven)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) | [示例](https://docs.microsoft.com/samples/browse/?products=azure)

## <a name="prerequisites"></a>先决条件

* Azure 订阅 - [创建试用订阅](https://wd.azure.cn/pricing/1rmb-trial-full)
* 最新版本的 [Java 开发工具包 (JDK)](https://www.oracle.com/java/technologies/javase-downloads.html)
* [Gradle 生成工具](https://gradle.org/install/)，或其他依赖项管理器。

## <a name="setting-up"></a>设置

### <a name="create-a-computer-vision-azure-resource"></a>创建计算机视觉 Azure 资源

Azure 认知服务由你订阅的 Azure 资源表示。 在本地计算机上使用 [Azure 门户](https://docs.azure.cn/cognitive-services/cognitive-services-apis-create-account)或 [Azure CLI](https://docs.azure.cn/cognitive-services/cognitive-services-apis-create-account-cli) 创建计算机视觉的资源。 也可执行以下操作：

<!--trial key not available -->
* 在 [Azure 门户](https://portal.azure.cn/)上查看资源。

然后，为密钥和服务终结点字符串[创建环境变量](https://docs.azure.cn/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)，分别名为 `COMPUTER_VISION_SUBSCRIPTION_KEY` 和 `COMPUTER_VISION_ENDPOINT`。

### <a name="create-a-new-gradle-project"></a>创建新的 Gradle 项目

在控制台窗口（例如 cmd、PowerShell 或 Bash）中，为应用创建一个新目录并导航到该目录。 

```console
mkdir myapp && cd myapp
```

从工作目录运行 `gradle init` 命令。 此命令将创建 Gradle 的基本生成文件，包括 *build.gradle.kts*，在运行时将使用该文件创建并配置应用程序。

```console
gradle init --type basic
```

当提示你选择一个 **DSL** 时，选择 **Kotlin**。

找到 *build.gradle.kts*，并使用喜好的 IDE 或文本编辑器将其打开。 然后在该文件中复制以下生成配置。 此配置将项目定义一个 Java 应用程序，其入口点为 **ComputerVisionQuickstarts** 类。 它将导入计算机视觉库。

```kotlin
plugins {
    java
    application
}
application { 
    mainClassName = "ComputerVisionQuickstarts"
}
repositories {
    mavenCentral()
}
```

从工作目录运行以下命令，以创建项目源文件夹：

```console
mkdir -p src/main/java
```

导航到新文件夹，创建名为 *ComputerVisionQuickstarts.java* 的文件。 在喜好的编辑器或 IDE 中打开该文件并添加以下 `import` 语句：

```java
import com.microsoft.azure.cognitiveservices.vision.computervision.*;
import com.microsoft.azure.cognitiveservices.vision.computervision.models.*;

import java.io.File;
import java.io.FileInputStream;
import java.nio.file.Files;

import java.util.ArrayList;
import java.util.List;
```

然后添加 **ComputerVisionQuickstarts** 的类定义。

### <a name="install-the-client-library"></a>安装客户端库

本快速入门使用 Gradle 依赖项管理器。 可以在 [Maven 中央存储库](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-textanalytics/)中找到客户端库以及其他依赖项管理器的信息。

在项目的 *build.gradle.kts* 文件中，包含计算机视觉客户端库作为依赖项。

```kotlin
dependencies {
    compile(group = "com.microsoft.azure.cognitiveservices", name = "azure-cognitiveservices-computervision", version = "1.0.2-beta")
}
```

## <a name="object-model"></a>对象模型

以下类和接口将处理人脸计算机视觉 Java SDK 的某些主要功能。

|名称|说明|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-java-stable) | 所有计算机视觉功能都需要此类。 请使用你的订阅信息实例化此类，然后使用它来生成其他类的实例。|
|[ComputerVision](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervision?view=azure-java-stable)| 此类来自客户端对象，它直接处理所有图像操作，例如图像分析、文本检测和缩略图生成。|
|[VisualFeatureTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-java-stable)| 此枚举定义可在标准分析操作中执行的不同类型的图像分析。 请根据需求指定一组 VisualFeatureTypes 值。 |

## <a name="code-examples"></a>代码示例

这些代码片段演示如何使用适用于 Java 的计算机视觉客户端库执行以下任务：

* [对客户端进行身份验证](#authenticate-the-client)
* [分析图像](#analyze-an-image)
* [读取印刷体文本和手写文本](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>验证客户端

> [!NOTE]
> 本快速入门假设你已为计算机视觉密钥创建了名为 `COMPUTER_VISION_SUBSCRIPTION_KEY` 的[环境变量](https://docs.azure.cn/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)。

以下代码将 `main` 方法添加到类，并为资源的 Azure 终结点和密钥创建变量。 你需要输入自己的终结点字符串，可以在 Azure 门户的**概述**部分找到该字符串。 

```java
public static void main(String[] args) {
    // Add your Computer Vision subscription key and endpoint to your environment
    // variables.
    // After setting, close and then re-open your command shell or project for the
    // changes to take effect.
    String subscriptionKey = System.getenv("COMPUTER_VISION_SUBSCRIPTION_KEY");
    String endpoint = System.getenv("COMPUTER_VISION_ENDPOINT");
```

接下来添加以下代码，以创建 [ComputerVisionClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-java-stable) 对象并将其传递给稍后要定义的其他方法。

```java
ComputerVisionClient compVisClient = ComputerVisionManager.authenticate(subscriptionKey).withEndpoint(endpoint);
// END - Create an authenticated Computer Vision client.

System.out.println("\nAzure Cognitive Services Computer Vision - Java Quickstart Sample");

// Analyze local and remote images
AnalyzeLocalImage(compVisClient);

// Recognize printed text with OCR for a local and remote (URL) image
RecognizeTextOCRLocal(compVisClient);
```

> [!NOTE]
> 如果在启动应用程序后创建了环境变量，则需要关闭再重新打开运行该应用程序的编辑器、IDE 或 shell 才能访问该变量。

## <a name="analyze-an-image"></a>分析图像

以下代码定义方法 `AnalyzeLocalImage`，该方法使用客户端对象分析本地图像并输出结果。 该方法返回文本说明、分类、标记列表、检测到的人脸、成人内容标志、主颜色和图像类型。

### <a name="set-up-test-image"></a>设置测试图像

首先，在项目的 **src/main/** 文件夹中创建 **resources/** 文件夹，并添加要分析的图像。 然后，将以下方法定义添加到 **ComputerVisionQuickstarts** 类。 如有必要，请更改 `pathToLocalImage` 的值，使之与图像文件相匹配。 

```java
public static void AnalyzeLocalImage(ComputerVisionClient compVisClient) {
    /*
     * Analyze a local image:
     *
     * Set a string variable equal to the path of a local image. The image path
     * below is a relative path.
     */
    String pathToLocalImage = "src\\main\\resources\\myImage.jpg";
```

> [!NOTE]
> 还可以使用其 URL 分析远程图像。 请参阅 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java)上的示例代码以了解涉及远程图像的方案。

### <a name="specify-visual-features"></a>指定视觉特性

接下来，指定要在分析中提取的视觉特征。 有关完整列表，请参阅 [VisualFeatureTypes](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-java-stable) 枚举。

```java
// This list defines the features to be extracted from the image.
List<VisualFeatureTypes> featuresToExtractFromLocalImage = new ArrayList<>();
featuresToExtractFromLocalImage.add(VisualFeatureTypes.DESCRIPTION);
featuresToExtractFromLocalImage.add(VisualFeatureTypes.CATEGORIES);
featuresToExtractFromLocalImage.add(VisualFeatureTypes.TAGS);
featuresToExtractFromLocalImage.add(VisualFeatureTypes.FACES);
featuresToExtractFromLocalImage.add(VisualFeatureTypes.ADULT);
featuresToExtractFromLocalImage.add(VisualFeatureTypes.COLOR);
featuresToExtractFromLocalImage.add(VisualFeatureTypes.IMAGE_TYPE);
```

### <a name="analyze"></a>分析
此方法根据每个图像分析范围将详细结果输出到控制台。 建议将此方法调用包含在 Try/Catch 块中。 **analyzeImageInStream** 方法将返回包含所有提取信息的 **ImageAnalysis** 对象。

```java
// Need a byte array for analyzing a local image.
File rawImage = new File(pathToLocalImage);
byte[] imageByteArray = Files.readAllBytes(rawImage.toPath());

// Call the Computer Vision service and tell it to analyze the loaded image.
ImageAnalysis analysis = compVisClient.computerVision().analyzeImageInStream().withImage(imageByteArray)
        .withVisualFeatures(featuresToExtractFromLocalImage).execute();
```

以下部分说明如何详细分析此信息。

### <a name="get-image-description"></a>获取图像说明

下面的代码获取为图像生成的描述文字列表。 有关详细信息，请参阅[描述图像](../../concept-describing-images.md)。

```java
// Display image captions and confidence values.
System.out.println("\nCaptions: ");
for (ImageCaption caption : analysis.description().captions()) {
    System.out.printf("\'%s\' with confidence %f\n", caption.text(), caption.confidence());
}
```

### <a name="get-image-category"></a>获取图像类别

下面的代码获取所检测到的图像类别。 有关详细信息，请参阅[对图像进行分类](../../concept-categorizing-images.md)。

```java
// Display image category names and confidence values.
System.out.println("\nCategories: ");
for (Category category : analysis.categories()) {
    System.out.printf("\'%s\' with confidence %f\n", category.name(), category.score());
}
```

### <a name="get-image-tags"></a>获取图像标记

以下代码获取图像中检测到的标记集。 有关详细信息，请参阅[内容标记](../../concept-tagging-images.md)。

```java
// Display image tags and confidence values.
System.out.println("\nTags: ");
for (ImageTag tag : analysis.tags()) {
    System.out.printf("\'%s\' with confidence %f\n", tag.name(), tag.confidence());
}
```

### <a name="detect-faces"></a>检测人脸

下面的代码返回图像中检测到的人脸及其矩形坐标，并选择人脸属性。 有关详细信息，请参阅[人脸检测](../../concept-detecting-faces.md)。

```java
// Display any faces found in the image and their location.
System.out.println("\nFaces: ");
for (FaceDescription face : analysis.faces()) {
    System.out.printf("\'%s\' of age %d at location (%d, %d), (%d, %d)\n", face.gender(), face.age(),
            face.faceRectangle().left(), face.faceRectangle().top(),
            face.faceRectangle().left() + face.faceRectangle().width(),
            face.faceRectangle().top() + face.faceRectangle().height());
}
```

### <a name="detect-adult-racy-or-gory-content"></a>检测成人、色情或血腥内容

以下代码输出图像中检测到的成人内容。 有关详细信息，请参阅[成人、色情或血腥内容](../../concept-detecting-adult-content.md)。

```java
// Display whether any adult or racy content was detected and the confidence
// values.
System.out.println("\nAdult: ");
System.out.printf("Is adult content: %b with confidence %f\n", analysis.adult().isAdultContent(),
        analysis.adult().adultScore());
System.out.printf("Has racy content: %b with confidence %f\n", analysis.adult().isRacyContent(),
        analysis.adult().racyScore());
```

### <a name="get-image-color-scheme"></a>获取图像配色方案

以下代码输出图像中检测到的颜色属性，如主色和主题色。 有关详细信息，请参阅[配色方案](../../concept-detecting-color-schemes.md)。

```java
// Display the image color scheme.
System.out.println("\nColor scheme: ");
System.out.println("Is black and white: " + analysis.color().isBWImg());
System.out.println("Accent color: " + analysis.color().accentColor());
System.out.println("Dominant background color: " + analysis.color().dominantColorBackground());
System.out.println("Dominant foreground color: " + analysis.color().dominantColorForeground());
System.out.println("Dominant colors: " + String.join(", ", analysis.color().dominantColors()));
```

### <a name="get-domain-specific-content"></a>获取特定于域的内容

计算机视觉可以使用专用模型对图像进行进一步分析。 有关详细信息，请参阅[特定于域的内容](../../concept-detecting-domain-content.md)。 

以下代码分析了图像中检测到的名人的相关数据。

```java
// Display any celebrities detected in the image and their locations.
System.out.println("\nCelebrities: ");
for (Category category : analysis.categories()) {
    if (category.detail() != null && category.detail().celebrities() != null) {
        for (CelebritiesModel celeb : category.detail().celebrities()) {
            System.out.printf("\'%s\' with confidence %f at location (%d, %d), (%d, %d)\n", celeb.name(),
                    celeb.confidence(), celeb.faceRectangle().left(), celeb.faceRectangle().top(),
                    celeb.faceRectangle().left() + celeb.faceRectangle().width(),
                    celeb.faceRectangle().top() + celeb.faceRectangle().height());
        }
    }
}
```

以下代码分析了图像中检测到的地标的相关数据。

```java
// Display any landmarks detected in the image and their locations.
System.out.println("\nLandmarks: ");
for (Category category : analysis.categories()) {
    if (category.detail() != null && category.detail().landmarks() != null) {
        for (LandmarksModel landmark : category.detail().landmarks()) {
            System.out.printf("\'%s\' with confidence %f\n", landmark.name(), landmark.confidence());
        }
    }
}
```

### <a name="get-the-image-type"></a>获取图像类型

以下代码输出有关图像类型的信息&mdash;无论它是剪贴画还是线条图。

```java
// Display what type of clip art or line drawing the image is.
System.out.println("\nImage type:");
System.out.println("Clip art type: " + analysis.imageType().clipArtType());
System.out.println("Line drawing type: " + analysis.imageType().lineDrawingType());
```

## <a name="read-printed-and-handwritten-text"></a>读取印刷体文本和手写文本

计算机视觉可以读取图像中的可见文本，并将其转换为字符流。

> [!NOTE]
> 还可以使用其 URL 读取远程图像中的文本。 请参阅 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java)上的示例代码以了解涉及远程图像的方案。

### <a name="call-the-recognize-api"></a>调用识别 API

首先，使用以下代码对给定图像调用 **recognizePrintedTextInStream** 方法。 将此代码添加到项目中时，需要将 `localTextImagePath` 的值替换为本地图像的路径。 

```java
/**
 * RECOGNIZE PRINTED TEXT: Displays text found in image with angle and orientation of
 * the block of text.
 */
private static void RecognizeTextOCRLocal(ComputerVisionClient client) {
    System.out.println("-----------------------------------------------");
    System.out.println("RECOGNIZE PRINTED TEXT");
    
    // Replace this string with the path to your own image.
    String localTextImagePath = "<local image path>";
    
    try {
        File rawImage = new File(localTextImagePath);
        byte[] localImageBytes = Files.readAllBytes(rawImage.toPath());

        // Recognize printed text in local image
        OcrResult ocrResultLocal = client.computerVision().recognizePrintedTextInStream()
                .withDetectOrientation(true).withImage(localImageBytes).withLanguage(OcrLanguages.EN).execute();
```

### <a name="print-recognize-results"></a>输出识别结果

以下代码块处理返回的文本并对其进行分析，以输出每行的第一个单词。 可以使用此代码快速了解 **OcrResult** 实例的结构。

```java
// Print results of local image
System.out.println();
System.out.println("Recognizing printed text from a local image with OCR ...");
System.out.println("\nLanguage: " + ocrResultLocal.language());
System.out.printf("Text angle: %1.3f\n", ocrResultLocal.textAngle());
System.out.println("Orientation: " + ocrResultLocal.orientation());

boolean firstWord = true;
// Gets entire region of text block
for (OcrRegion reg : ocrResultLocal.regions()) {
    // Get one line in the text block
    for (OcrLine line : reg.lines()) {
        for (OcrWord word : line.words()) {
            // get bounding box of first word recognized (just to demo)
            if (firstWord) {
                System.out.println("\nFirst word in first line is \"" + word.text()
                        + "\" with  bounding box: " + word.boundingBox());
                firstWord = false;
                System.out.println();
            }
            System.out.print(word.text() + " ");
        }
        System.out.println();
    }
}
```

最后，结束 try/catch 块和方法定义。

```java
} catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }
}
```

## <a name="run-the-application"></a>运行应用程序

可使用以下命令生成应用：

```console
gradle build
```

使用 `gradle run` 命令运行应用程序：

```console
gradle run
```

## <a name="clean-up-resources"></a>清理资源

如果想要清理并删除认知服务订阅，可以删除资源或资源组。 删除资源组同时也会删除与之相关联的任何其他资源。

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>后续步骤

本快速入门已介绍如何使用计算机视觉 Java 库执行基本任务。 接下来，请在参考文档中详细了解该库。

> [!div class="nextstepaction"]
>[计算机视觉参考 (Java)](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/computervision?view=azure-java-stable)

* [什么是计算机视觉？](../../Home.md)
* 可以在 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/ComputerVision/src/main/java/ComputerVisionQuickstart.java) 上找到此示例的源代码。