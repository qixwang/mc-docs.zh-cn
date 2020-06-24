---
title: 快速入门：适用于 Node.js 的计算机视觉客户端库
description: 通过本快速入门开始使用适用于 Node.js 的计算机视觉客户端库
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: include
ms.date: 06/08/2020
ms.author: v-tawe
origin.date: 01/22/2020
ms.openlocfilehash: 6d891b2d93d779bb5603de58857a03c60e77b909
ms.sourcegitcommit: 8dae792aefbe44e8388f961b813e3da6564423ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84654991"
---
<a name="HOLTop"></a>

[参考文档](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest) | [库源代码](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-computervision) | [包 (npm)](https://www.npmjs.com/package/@azure/cognitiveservices-computervision) | [示例](https://azure.microsoft.com/resources/samples/?service=cognitive-services&term=vision&sort=0)

## <a name="prerequisites"></a>先决条件

* Azure 订阅 - [创建试用订阅](https://wd.azure.cn/pricing/1rmb-trial-full)
* 最新版本的 [Node.js](https://nodejs.org/)

## <a name="setting-up"></a>设置

### <a name="create-a-computer-vision-azure-resource"></a>创建计算机视觉 Azure 资源

Azure 认知服务由你订阅的 Azure 资源表示。 在本地计算机上使用 [Azure 门户](/cognitive-services/cognitive-services-apis-create-account)或 [Azure CLI](/cognitive-services/cognitive-services-apis-create-account-cli) 创建计算机视觉的资源。 也可执行以下操作：

<!-- trial key not available-->
* 在 [Azure 门户](https://portal.azure.cn/)上查看资源。

获取试用订阅或资源的密钥后，请为该密钥和终结点 URL [创建环境变量](https://docs.azure.cn/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)，分别名为 `COMPUTER_VISION_SUBSCRIPTION_KEY` 和 `COMPUTER_VISION_ENDPOINT`。

### <a name="create-a-new-nodejs-application"></a>创建新的 Node.js 应用程序

在控制台窗口（例如 cmd、PowerShell 或 Bash）中，为应用创建一个新目录并导航到该目录。

```console
mkdir myapp && cd myapp
```

运行 `npm init` 命令以使用 `package.json` 文件创建一个 node 应用程序。

```console
npm init
```

### <a name="install-the-client-library"></a>安装客户端库

安装 `ms-rest-azure` 和 `@azure/cognitiveservices-computervision` NPM 包:

```console
npm install @azure/cognitiveservices-computervision
```

应用的 `package.json` 文件将使用依赖项进行更新。

### <a name="prepare-the-nodejs-script"></a>准备 Node.js 脚本

创建新文件 *index.js*，将其在文本编辑器中打开。 添加以下 import 语句。
```javascript
'use strict';

const async = require('async');
const fs = require('fs');
const path = require("path");
const createReadStream = require('fs').createReadStream
const sleep = require('util').promisify(setTimeout);
const ComputerVisionClient = require('@azure/cognitiveservices-computervision').ComputerVisionClient;
const ApiKeyCredentials = require('@azure/ms-rest-js').ApiKeyCredentials;
```

然后定义函数 `computerVision`，并声明一个包含主函数和回调函数的异步系列。 我们会将快速入门代码添加到主函数中，并调用脚本底部的 `computerVision`。
```javascript
function computerVision() {
  async.series([
    async function () {},
    function () {
      return new Promise((resolve) => {
        resolve();
      })
    }
  ], (err) => {
    throw (err);
  });
}

computerVision();
```

## <a name="object-model"></a>对象模型

以下类和接口将处理计算机视觉 Node.js SDK 的某些主要功能。

|名称|说明|
|---|---|
| [ComputerVisionClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/computervisionclient?view=azure-node-latest) | 所有计算机视觉功能都需要此类。 可以使用订阅信息实例化此类，然后使用它来执行大多数图像操作。|
|[VisualFeatureTypes](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/visualfeaturetypes?view=azure-node-latest)| 此枚举定义可在标准分析操作中执行的不同类型的图像分析。 请根据需求指定一组 **VisualFeatureTypes** 值。 |

## <a name="code-examples"></a>代码示例

这些代码片段演示如何使用适用于 Node.js 的计算机视觉客户端库执行以下任务：

* [对客户端进行身份验证](#authenticate-the-client)
* [分析图像](#analyze-an-image)
* [读取印刷体文本和手写文本](#read-printed-and-handwritten-text)

## <a name="authenticate-the-client"></a>验证客户端

为资源的 Azure 终结点和密钥创建变量。 如果在启动应用程序后创建了环境变量，则需要关闭再重新打开运行该应用程序的编辑器、IDE 或 shell 才能访问该变量。
```javascript
/**
 * AUTHENTICATE
 * This single client is used for all examples.
 */
let key = process.env['COMPUTER_VISION_SUBSCRIPTION_KEY'];
let endpoint = process.env['COMPUTER_VISION_ENDPOINT']
if (!key) { throw new Error('Set your environment variables for your subscription key and endpoint.'); }
```


使用终结点和密钥实例化某个客户端。 使用密钥和终结点创建 [ApiKeyCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.apikeycredentials?view=azure-python) 对象，然后使用它创建 [ComputerVisionClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/computervisionclient?view=azure-node-latest) 对象。

```javascript
let computerVisionClient = new ComputerVisionClient(
    new ApiKeyCredentials({inHeader: {'Ocp-Apim-Subscription-Key': key}}), endpoint);
```

## <a name="analyze-an-image"></a>分析图像

此部分的代码通过分析远程图像来提取各种视觉特征。 可以在客户端对象的 **analyzeImage** 方法中执行这些操作，也可以使用单个方法来调用它们。 有关详细信息，请参阅[参考文档](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest)。

> [!NOTE]
> 还可以分析本地图像。 请参阅 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js) 上的示例代码以了解涉及本地图像的方案。

### <a name="get-image-description"></a>获取图像说明

下面的代码获取为图像生成的描述文字列表。 有关更多详细信息，请参阅[描述图像](../../concept-describing-images.md)。

首先，定义要分析的图像的 URL：

```javascript
var describeURL = 'https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg';
```

然后添加以下代码，用于获取图像详细信息并将其输出到控制台。

```javascript
// Analyze URL image
console.log('Analyzing URL image to describe...', describeURL.split('/').pop());
var caption = (await computerVisionClient.describeImage(describeURL)).captions[0];
console.log(`This may be ${caption.text} (${caption.confidence.toFixed(2)} confidence)`);
```

### <a name="get-image-category"></a>获取图像类别

下面的代码获取所检测到的图像类别。 有关更多详细信息，请参阅[对图像进行分类](../../concept-categorizing-images.md)。

```javascript
const categoryURLImage = 'https://moderatorsampleimages.blob.core.windows.net/samples/sample16.png';

// Analyze URL image
console.log('Analyzing category in image...', categoryURLImage.split('/').pop());
let categories = (await computerVisionClient.analyzeImage(categoryURLImage)).categories;
console.log(`Categories: ${formatCategories(categories)}`);
```

定义帮助程序函数 `formatCategories`：

```javascript
// Formats the image categories
function formatCategories(categories) {
  categories.sort((a, b) => b.score - a.score);
  return categories.map(cat => `${cat.name} (${cat.score.toFixed(2)})`).join(', ');
}
```

### <a name="get-image-tags"></a>获取图像标记

以下代码获取图像中检测到的标记集。 有关更多详细信息，请参阅[内容标记](../../concept-tagging-images.md)。

```javascript
console.log('-------------------------------------------------');
console.log('DETECT TAGS');
console.log();

// Image of different kind of dog.
const tagsURL = 'https://moderatorsampleimages.blob.core.windows.net/samples/sample16.png';

// Analyze URL image
console.log('Analyzing tags in image...', tagsURL.split('/').pop());
let tags = (await computerVisionClient.analyzeImage(tagsURL, {visualFeatures: ['Tags']})).tags;
console.log(`Tags: ${formatTags(tags)}`);
```

定义帮助程序函数 `formatTags`：

```javascript
// Format tags for display
function formatTags(tags) {
  return tags.map(tag => (`${tag.name} (${tag.confidence.toFixed(2)})`)).join(', ');
}
```

### <a name="detect-objects"></a>检测物体

以下代码检测图像中的常见物体并将其输出到控制台。 有关更多详细信息，请参阅[物体检测](../../concept-object-detection.md)。

```javascript
// Image of a dog
const objectURL = 'https://raw.githubusercontent.com/Azure-Samples/cognitive-services-node-sdk-samples/master/Data/image.jpg';

// Analyze a URL image
console.log('Analyzing objects in image...', objectURL.split('/').pop());
let objects = (await computerVisionClient.analyzeImage(objectURL, {visualFeatures: ['Objects']})).objects;
console.log();

// Print objects bounding box and confidence
if (objects.length) {
    console.log(`${objects.length} object${objects.length == 1 ? '' : 's'} found:`);
    for (let obj of objects) { console.log(`    ${obj.object} (${obj.confidence.toFixed(2)}) at ${formatRectObjects(obj.rectangle)}`); }
} else { console.log('No objects found.'); }
```

定义帮助程序函数 `formatRectObjects`：

```javascript
// Formats the bounding box
function formatRectObjects(rect) {
  return `top=${rect.y}`.padEnd(10) + `left=${rect.x}`.padEnd(10) + `bottom=${rect.y + rect.h}`.padEnd(12) 
  + `right=${rect.x + rect.w}`.padEnd(10) + `(${rect.w}x${rect.h})`;
}
```

### <a name="detect-brands"></a>检测品牌

以下代码检测图像中的公司品牌和徽标，并将其输出到控制台。 有关更多详细信息，请参阅[品牌检测](../../concept-brand-detection.md)。

```javascript
const brandURLImage = 'https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/images/red-shirt-logo.jpg';

// Analyze URL image
console.log('Analyzing brands in image...', brandURLImage.split('/').pop());
let brands = (await computerVisionClient.analyzeImage(brandURLImage, {visualFeatures: ['Brands']})).brands;

// Print the brands found
if (brands.length) {
    console.log(`${brands.length} brand${brands.length != 1 ? 's' : ''} found:`);
    for (let brand of brands) {
        console.log(`    ${brand.name} (${brand.confidence.toFixed(2)} confidence)`);
    }
} else { console.log(`No brands found.`); }
```

### <a name="detect-faces"></a>检测人脸

下面的代码返回图像中检测到的人脸及其矩形坐标，以及选择面属性。 有关更多详细信息，请参阅[人脸检测](../../concept-detecting-faces.md)。

```javascript
const facesImageURL = 'https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/faces.jpg';

// Analyze URL image.
console.log('Analyzing faces in image...', facesImageURL.split('/').pop());
// Get the visual feature for 'Faces' only.
let faces = (await computerVisionClient.analyzeImage(facesImageURL, {visualFeatures: ['Faces']})).faces;

// Print the bounding box, gender, and age from the faces.
if (faces.length) {
  console.log(`${faces.length} face${faces.length == 1 ? '' : 's'} found:`);
  for (let face of faces) { console.log(`    Gender: ${face.gender}`.padEnd(20) 
    + ` Age: ${face.age}`.padEnd(10) + `at ${formatRectFaces(face.faceRectangle)}`); }
} else { console.log('No faces found.'); }
```

定义帮助程序函数 `formatRectFaces`：

```javascript
// Formats the bounding box
function formatRectFaces(rect) {
  return `top=${rect.top}`.padEnd(10) + `left=${rect.left}`.padEnd(10) + `bottom=${rect.top + rect.height}`.padEnd(12) 
    + `right=${rect.left + rect.width}`.padEnd(10) + `(${rect.width}x${rect.height})`;
}
```

### <a name="detect-adult-racy-or-gory-content"></a>检测成人、色情或血腥内容

以下代码输出图像中检测到的成人内容。 有关更多详细信息，请参阅[成人、色情或血腥内容](../../concept-detecting-adult-content.md)。

定义要使用的图像的 URL：

```javascript
// The URL image and local images are not racy/adult. 
// Try your own racy/adult images for a more effective result.
const adultURLImage = 'https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/celebrities.jpg';
```

然后添加以下代码来检测成人内容，并将结果输出到控制台。

```javascript
// Function to confirm racy or not
const isIt = flag => flag ? 'is' : "isn't";

// Analyze URL image
console.log('Analyzing image for racy/adult content...', adultURLImage.split('/').pop());
var adult = (await computerVisionClient.analyzeImage(adultURLImage, {
  visualFeatures: ['Adult']
})).adult;
console.log(`This probably ${isIt(adult.isAdultContent)} adult content (${adult.adultScore.toFixed(4)} score)`);
console.log(`This probably ${isIt(adult.isRacyContent)} racy content (${adult.racyScore.toFixed(4)} score)`);
```

### <a name="get-image-color-scheme"></a>获取图像配色方案

以下代码输出图像中检测到的颜色属性，如主色和主题色。 有关更多详细信息，请参阅[配色方案](../../concept-detecting-color-schemes.md)。

```javascript
const colorURLImage = 'https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/celebrities.jpg';

// Analyze URL image
console.log('Analyzing image for color scheme...', colorURLImage.split('/').pop());
console.log();
let color = (await computerVisionClient.analyzeImage(colorURLImage, {visualFeatures: ['Color']})).color;
printColorScheme(color);
```

定义帮助程序函数 `printColorScheme`，将颜色方案的详细信息输出到控制台。

```javascript
// Print a detected color scheme
function printColorScheme(colors){
  console.log(`Image is in ${colors.isBwImg ? 'black and white' : 'color'}`);
  console.log(`Dominant colors: ${colors.dominantColors.join(', ')}`);
  console.log(`Dominant foreground color: ${colors.dominantColorForeground}`);
  console.log(`Dominant background color: ${colors.dominantColorBackground}`);
  console.log(`Suggested accent color: #${colors.accentColor}`);
}
```

### <a name="get-domain-specific-content"></a>获取特定于域的内容

计算机视觉可以使用专用模型对图像进行进一步分析。 有关更多详细信息，请参阅[特定于域的内容](../../concept-detecting-domain-content.md)。

首先，定义要分析的图像的 URL：

```javascript
const domainURLImage = 'https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/landmark.jpg';
```

以下代码分析了图像中检测到的地标的相关数据。

```javascript
// Analyze URL image
console.log('Analyzing image for landmarks...', domainURLImage.split('/').pop());
let domain = (await computerVisionClient.analyzeImageByDomain('landmarks', domainURLImage)).result.landmarks;

// Prints domain-specific, recognized objects
if (domain.length) {
  console.log(`${domain.length} ${domain.length == 1 ? 'landmark' : 'landmarks'} found:`);
  for (let obj of domain) {
    console.log(`    ${obj.name}`.padEnd(20) + `(${obj.confidence.toFixed(2)} confidence)`.padEnd(20) + `${formatRectDomain(obj.faceRectangle)}`);
  }
} else {
  console.log('No landmarks found.');
}
```

定义帮助程序函数 `formatRectDomain`，分析有关检测到的地标的位置数据。

```javascript
// Formats bounding box
function formatRectDomain(rect) {
  if (!rect) return '';
  return `top=${rect.top}`.padEnd(10) + `left=${rect.left}`.padEnd(10) + `bottom=${rect.top + rect.height}`.padEnd(12) +
    `right=${rect.left + rect.width}`.padEnd(10) + `(${rect.width}x${rect.height})`;
}
```

### <a name="get-the-image-type"></a>获取图像类型

以下代码输出有关图像类型的信息&mdash;无论它是剪贴画还是线条图。

```javascript
const typeURLImage = 'https://raw.githubusercontent.com/Azure-Samples/cognitive-services-python-sdk-samples/master/samples/vision/images/make_things_happen.jpg';

 // Analyze URL image
console.log('Analyzing type in image...', typeURLImage.split('/').pop());
let types = (await computerVisionClient.analyzeImage(typeURLImage, {visualFeatures: ['ImageType']})).imageType;
console.log(`Image appears to be ${describeType(types)}`);
```

定义帮助程序函数 `describeType`：

```javascript
function describeType(imageType) {
  if (imageType.clipArtType && imageType.clipArtType > imageType.lineDrawingType) return 'clip art';
  if (imageType.lineDrawingType && imageType.clipArtType < imageType.lineDrawingType) return 'a line drawing';
  return 'a photograph';
}
```

## <a name="read-printed-and-handwritten-text"></a>读取印刷体文本和手写文本

计算机视觉可以读取图像中的可见文本，并将其转换为字符流。

> [!NOTE]
> 还可以从本地图像读取文本。 请参阅 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js) 上的示例代码以了解涉及本地图像的方案。

### <a name="set-up-test-images"></a>设置测试图像

保存要从中提取文本的图像的 URL 的引用。

```javascript
// URL images containing printed and handwritten text
 const printedText     = 'https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg';
 const handwrittenText = 'https://raw.githubusercontent.com/MicrosoftDocs/azure-docs/master/articles/cognitive-services/Computer-vision/Images/readsample.jpg';
```

### <a name="call-the-recognize-api"></a>调用识别 API

添加以下代码，该代码针对给定图像调用 `recognizeText` 函数。

```javascript
// Recognize text in printed image
console.log('Recognizing printed text...', printedText.split('/').pop());
var printed = await recognizeText(computerVisionClient, 'Printed', printedText);
printRecText(printed);

// Recognize text in handwritten image
console.log('\nRecognizing handwritten text...', handwrittenText.split('/').pop());
var handwriting = await recognizeText(computerVisionClient, 'Handwritten', handwrittenText);
printRecText(handwriting);
```

定义 `recognizeText` 函数。 这会在客户端对象上调用 **recognizeText** 方法，该方法返回一个操作 ID 并启动异步进程来读取图像的内容。 然后，它会使用操作 ID 每隔一秒钟检查一次操作，直到返回结果。 然后它会返回提取的结果。

```javascript
// Perform text recognition and await the result
async function recognizeText(client, mode, url) {
  // To recognize text in a local image, replace client.recognizeText() with recognizeTextInStream() as shown:
  // result = await client.recognizeTextInStream(mode, () => createReadStream(localImagePath));
  let result = await client.recognizeText(mode, url);
  // Operation ID is last path segment of operationLocation (a URL)
  let operation = result.operationLocation.split('/').slice(-1)[0];

  // Wait for text recognition to complete
  // result.status is initially undefined, since it's the result of recognizeText
  while (result.status !== 'Succeeded') { await sleep(1000); result = await client.getTextOperationResult(operation); }
  return result.recognitionResult;
}
```

然后定义帮助程序函数 `printRecText`，该函数将某项识别操作的结果输出到控制台。

```javascript
// Prints all text from OCR result
function printRecText(ocr) {
  if (ocr.lines.length) {
      console.log('Recognized text:');
      for (let line of ocr.lines) {
          console.log(line.words.map(w => w.text).join(' '));
      }
  }
  else { console.log('No recognized text.'); }
}
```

## <a name="run-the-application"></a>运行应用程序

在快速入门文件中使用 `node` 命令运行应用程序。

```console
node index.js
```

## <a name="clean-up-resources"></a>清理资源

如果想要清理并删除认知服务订阅，可以删除资源或资源组。 删除资源组同时也会删除与之相关联的任何其他资源。

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
>[计算机视觉 API 参考 (Node.js)](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-computervision/?view=azure-node-latest)

* [什么是计算机视觉？](../../Home.md)
* 可以在 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/ComputerVision/ComputerVisionQuickstart.js) 上找到此示例的源代码。
