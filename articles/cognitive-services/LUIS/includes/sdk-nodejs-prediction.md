---
title: include 文件
description: include 文件
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 08/07/2020
origin.date: 05/28/2020
ms.topic: include
ms.custom: include file, devx-track-javascript
ms.author: v-johya
ms.openlocfilehash: 8e3efec92bc856fbf7a959ed9342f4533d12ad61
ms.sourcegitcommit: caa18677adb51b5321ad32ae62afcf92ac00b40b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2020
ms.locfileid: "88024217"
---
可以使用适用于 Node.js 的语言理解 (LUIS) 运行时客户端库执行以下操作：

* 按槽进行预测
* 按版本进行预测

[参考文档](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/?view=azure-node-latest) | [库源代码](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-luis-runtime) | [运行时包 (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime) | [示例](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/LUIS/node-sdk-authoring-prediction/luis_prediction.js)

## <a name="prerequisites"></a>先决条件

* 语言理解运行时资源：[在 Azure 门户中创建一个](https://portal.azure.cn/#create/Microsoft.CognitiveServicesLUISAllInOne)
* [Node.js](https://nodejs.org)
* LUIS 应用 ID - 使用 `df67dcdb-c37d-46af-88e1-8b97951ca1c2` 的公共 IoT 应用 ID。 快速入门代码中使用的用户查询特定于该应用。

## <a name="setting-up"></a>设置

### <a name="get-your-language-understanding-luis-runtime-key"></a>获取语言理解 (LUIS) 运行时密钥

通过创建 LUIS 运行时资源，获取[运行时密钥](../luis-how-to-azure-subscription.md)。 保留密钥和密钥的终结点，以便用于下一步。

### <a name="create-a-new-javascript-nodejs-file"></a>创建新的 JavaScript (Node.js) 文件

在你喜欢使用的编辑器或 IDE 中创建名为 `luis_prediction.js` 的新 JavaScript 文件。

### <a name="install-the-npm-library-for-the-luis-runtime"></a>安装适用于 LUIS 运行时的 NPM 库

在应用程序目录中，使用以下命令安装依赖项：

```console
npm install @azure/cognitiveservices-luis-runtime @azure/ms-rest-js
```

## <a name="object-model"></a>对象模型

语言理解 (LUIS) 创作客户端是对 Azure 进行身份验证的 [LUISAuthoringClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient?view=azure-node-latest) 对象，其中包含创作密钥。

创建客户端后，可以使用此客户端访问如下所述的功能：

* 按 `staging` 或 `production` 槽进行[预测](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-)
* [按版本进行预测](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getversionprediction-string--string--predictionrequest--models-predictiongetversionpredictionoptionalparams-)

## <a name="code-examples"></a>代码示例

这些代码片段演示如何使用语言理解 (LUIS) 预测运行时客户端库执行以下操作：

* [按槽进行预测](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>添加依赖项

在偏好的编辑器或 IDE 中打开项目目录中的 `luis_prediction.js` 文件。 添加以下依赖项：

```javascript 
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Query by slot name
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-runtime
 */

// For more information about LUIS, see
//
// LUIS portal - https://luis.azure.cn/welcome
// LUIS conceptual docs - /cognitive-services/luis
// LUIS JS SDK docs for prediction - https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest
// LUIS Runtime NPM - https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime


// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-runtime");
// </Dependencies>

// <Variables>
const key = "REPLACE-WITH-YOUR-ASSIGNED-PREDICTION-KEY"

const endpoint = "https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.cn"
// </Variables>

// <AuthoringCreateClient>
const luisRuntimeClient = new LUIS.LUISRuntimeClient(
    new msRest.ApiKeyCredentials({
        inHeader: { "Ocp-Apim-Subscription-Key": key }
    }),
    endpoint
);
// </AuthoringCreateClient>

// <OtherVariables>
// Use public app ID or replace with your own trained and published app's ID
// to query your own app
// public appID = `df67dcdb-c37d-46af-88e1-8b97951ca1c2`
// with slot of `production`
const luisAppID = "REPLACE-WITH-YOUR-LUIS_APP_ID"

// `production` or `staging`
const luisSlotName = "production"
// </OtherVariables>

// <predict>
const predict = async (app_id, slot_name) => {

    const predictionRequest = {
        query: "turn on all lights",
        options: {
            datetimeReference: new Date(),
            preferExternalEntities: false
        },
        externalEntities: [],
        dynamicLists: []
    };
    const verbose = true;
    const showAllIntents = true;

    // Note be sure to specify, using the slot_name parameter, whether your application is in staging or production.
    const predictionResult = await luisRuntimeClient.prediction
        .getSlotPrediction(luisAppID, luisSlotName, predictionRequest, { verbose, showAllIntents });

    console.log(JSON.stringify(predictionResult));
}
// </predict>

// <Main>
const quickstart = async () => {
    await predict();
}

try {
    quickstart();
} catch (error) {
    console.log(error);
}
// </Main>
```

## <a name="authenticate-the-client"></a>验证客户端

1. 为自己所需的 LUIS 信息创建变量：

    添加用于管理预测密钥和终结点密钥的变量。 
    
```javascript 
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Query by slot name
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-runtime
 */

// For more information about LUIS, see
//
// LUIS portal - https://luis.azure.cn/welcome
// LUIS conceptual docs - /cognitive-services/luis
// LUIS JS SDK docs for prediction - https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest
// LUIS Runtime NPM - https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime


// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-runtime");
// </Dependencies>

// <Variables>
const key = "REPLACE-WITH-YOUR-ASSIGNED-PREDICTION-KEY"

const endpoint = "https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.cn"
// </Variables>

// <AuthoringCreateClient>
const luisRuntimeClient = new LUIS.LUISRuntimeClient(
    new msRest.ApiKeyCredentials({
        inHeader: { "Ocp-Apim-Subscription-Key": key }
    }),
    endpoint
);
// </AuthoringCreateClient>

// <OtherVariables>
// Use public app ID or replace with your own trained and published app's ID
// to query your own app
// public appID = `df67dcdb-c37d-46af-88e1-8b97951ca1c2`
// with slot of `production`
const luisAppID = "REPLACE-WITH-YOUR-LUIS_APP_ID"

// `production` or `staging`
const luisSlotName = "production"
// </OtherVariables>

// <predict>
const predict = async (app_id, slot_name) => {

    const predictionRequest = {
        query: "turn on all lights",
        options: {
            datetimeReference: new Date(),
            preferExternalEntities: false
        },
        externalEntities: [],
        dynamicLists: []
    };
    const verbose = true;
    const showAllIntents = true;

    // Note be sure to specify, using the slot_name parameter, whether your application is in staging or production.
    const predictionResult = await luisRuntimeClient.prediction
        .getSlotPrediction(luisAppID, luisSlotName, predictionRequest, { verbose, showAllIntents });

    console.log(JSON.stringify(predictionResult));
}
// </predict>

// <Main>
const quickstart = async () => {
    await predict();
}

try {
    quickstart();
} catch (error) {
    console.log(error);
}
// </Main>
```

1. 为名为 `LUIS_APP_ID` 的应用程序 ID 创建变量。 将变量设置为公共 IoT 应用 `df67dcdb-c37d-46af-88e1-8b97951ca1c2`。 创建用于设置 `production` 发布槽的变量。

```javascript 
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Query by slot name
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-runtime
 */

// For more information about LUIS, see
//
// LUIS portal - https://luis.azure.cn/welcome
// LUIS conceptual docs - /cognitive-services/luis
// LUIS JS SDK docs for prediction - https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest
// LUIS Runtime NPM - https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime


// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-runtime");
// </Dependencies>

// <Variables>
const key = "REPLACE-WITH-YOUR-ASSIGNED-PREDICTION-KEY"

const endpoint = "https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.cn"
// </Variables>

// <AuthoringCreateClient>
const luisRuntimeClient = new LUIS.LUISRuntimeClient(
    new msRest.ApiKeyCredentials({
        inHeader: { "Ocp-Apim-Subscription-Key": key }
    }),
    endpoint
);
// </AuthoringCreateClient>

// <OtherVariables>
// Use public app ID or replace with your own trained and published app's ID
// to query your own app
// public appID = `df67dcdb-c37d-46af-88e1-8b97951ca1c2`
// with slot of `production`
const luisAppID = "REPLACE-WITH-YOUR-LUIS_APP_ID"

// `production` or `staging`
const luisSlotName = "production"
// </OtherVariables>

// <predict>
const predict = async (app_id, slot_name) => {

    const predictionRequest = {
        query: "turn on all lights",
        options: {
            datetimeReference: new Date(),
            preferExternalEntities: false
        },
        externalEntities: [],
        dynamicLists: []
    };
    const verbose = true;
    const showAllIntents = true;

    // Note be sure to specify, using the slot_name parameter, whether your application is in staging or production.
    const predictionResult = await luisRuntimeClient.prediction
        .getSlotPrediction(luisAppID, luisSlotName, predictionRequest, { verbose, showAllIntents });

    console.log(JSON.stringify(predictionResult));
}
// </predict>

// <Main>
const quickstart = async () => {
    await predict();
}

try {
    quickstart();
} catch (error) {
    console.log(error);
}
// </Main>
```


1. 使用密钥创建 msRest.ApiKeyCredentials 对象，并在终结点中使用该对象创建一个 [LUIS.LUISRuntimeClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient?view=azure-node-latest) 对象。

```javascript 
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Query by slot name
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-runtime
 */

// For more information about LUIS, see
//
// LUIS portal - https://luis.azure.cn/welcome
// LUIS conceptual docs - /cognitive-services/luis
// LUIS JS SDK docs for prediction - https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest
// LUIS Runtime NPM - https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime


// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-runtime");
// </Dependencies>

// <Variables>
const key = "REPLACE-WITH-YOUR-ASSIGNED-PREDICTION-KEY"

const endpoint = "https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.cn"
// </Variables>

// <AuthoringCreateClient>
const luisRuntimeClient = new LUIS.LUISRuntimeClient(
    new msRest.ApiKeyCredentials({
        inHeader: { "Ocp-Apim-Subscription-Key": key }
    }),
    endpoint
);
// </AuthoringCreateClient>

// <OtherVariables>
// Use public app ID or replace with your own trained and published app's ID
// to query your own app
// public appID = `df67dcdb-c37d-46af-88e1-8b97951ca1c2`
// with slot of `production`
const luisAppID = "REPLACE-WITH-YOUR-LUIS_APP_ID"

// `production` or `staging`
const luisSlotName = "production"
// </OtherVariables>

// <predict>
const predict = async (app_id, slot_name) => {

    const predictionRequest = {
        query: "turn on all lights",
        options: {
            datetimeReference: new Date(),
            preferExternalEntities: false
        },
        externalEntities: [],
        dynamicLists: []
    };
    const verbose = true;
    const showAllIntents = true;

    // Note be sure to specify, using the slot_name parameter, whether your application is in staging or production.
    const predictionResult = await luisRuntimeClient.prediction
        .getSlotPrediction(luisAppID, luisSlotName, predictionRequest, { verbose, showAllIntents });

    console.log(JSON.stringify(predictionResult));
}
// </predict>

// <Main>
const quickstart = async () => {
    await predict();
}

try {
    quickstart();
} catch (error) {
    console.log(error);
}
// </Main>
```

## <a name="get-prediction-from-runtime"></a>从运行时获取预测

添加以下方法以创建对预测运行时的请求。

用户言语是 [predictionRequest](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionrequest?view=azure-node-latest) 对象的一部分。

**[luisRuntimeClient.prediction.getSlotPrediction](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-)** 方法需要多个参数，如应用 ID、槽名称、用于满足请求的预测请求对象。 其他选项（如详细、显示所有意向和日志）都是可选的。

```javascript 
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Query by slot name
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-runtime
 */

// For more information about LUIS, see
//
// LUIS portal - https://luis.azure.cn/welcome
// LUIS conceptual docs - /cognitive-services/luis
// LUIS JS SDK docs for prediction - https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest
// LUIS Runtime NPM - https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime


// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-runtime");
// </Dependencies>

// <Variables>
const key = "REPLACE-WITH-YOUR-ASSIGNED-PREDICTION-KEY"

const endpoint = "https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.cn"
// </Variables>

// <AuthoringCreateClient>
const luisRuntimeClient = new LUIS.LUISRuntimeClient(
    new msRest.ApiKeyCredentials({
        inHeader: { "Ocp-Apim-Subscription-Key": key }
    }),
    endpoint
);
// </AuthoringCreateClient>

// <OtherVariables>
// Use public app ID or replace with your own trained and published app's ID
// to query your own app
// public appID = `df67dcdb-c37d-46af-88e1-8b97951ca1c2`
// with slot of `production`
const luisAppID = "REPLACE-WITH-YOUR-LUIS_APP_ID"

// `production` or `staging`
const luisSlotName = "production"
// </OtherVariables>

// <predict>
const predict = async (app_id, slot_name) => {

    const predictionRequest = {
        query: "turn on all lights",
        options: {
            datetimeReference: new Date(),
            preferExternalEntities: false
        },
        externalEntities: [],
        dynamicLists: []
    };
    const verbose = true;
    const showAllIntents = true;

    // Note be sure to specify, using the slot_name parameter, whether your application is in staging or production.
    const predictionResult = await luisRuntimeClient.prediction
        .getSlotPrediction(luisAppID, luisSlotName, predictionRequest, { verbose, showAllIntents });

    console.log(JSON.stringify(predictionResult));
}
// </predict>

// <Main>
const quickstart = async () => {
    await predict();
}

try {
    quickstart();
} catch (error) {
    console.log(error);
}
// </Main>
```

## <a name="main-code-for-the-prediction"></a>预测的主代码

使用以下主方法将变量和方法结合在一起，以获取预测。

```javascript 
"use strict";

/* This sample for the Azure Cognitive Services LUIS API shows how to:
 * - Query by slot name
 */

/* To run this sample, install the following modules.
 * npm install @azure/ms-rest-js
 * npm install @azure/cognitiveservices-luis-runtime
 */

// For more information about LUIS, see
//
// LUIS portal - https://luis.azure.cn/welcome
// LUIS conceptual docs - /cognitive-services/luis
// LUIS JS SDK docs for prediction - https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest
// LUIS Runtime NPM - https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime


// <Dependencies>
const msRest = require("@azure/ms-rest-js");
const LUIS = require("@azure/cognitiveservices-luis-runtime");
// </Dependencies>

// <Variables>
const key = "REPLACE-WITH-YOUR-ASSIGNED-PREDICTION-KEY"

const endpoint = "https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.cn"
// </Variables>

// <AuthoringCreateClient>
const luisRuntimeClient = new LUIS.LUISRuntimeClient(
    new msRest.ApiKeyCredentials({
        inHeader: { "Ocp-Apim-Subscription-Key": key }
    }),
    endpoint
);
// </AuthoringCreateClient>

// <OtherVariables>
// Use public app ID or replace with your own trained and published app's ID
// to query your own app
// public appID = `df67dcdb-c37d-46af-88e1-8b97951ca1c2`
// with slot of `production`
const luisAppID = "REPLACE-WITH-YOUR-LUIS_APP_ID"

// `production` or `staging`
const luisSlotName = "production"
// </OtherVariables>

// <predict>
const predict = async (app_id, slot_name) => {

    const predictionRequest = {
        query: "turn on all lights",
        options: {
            datetimeReference: new Date(),
            preferExternalEntities: false
        },
        externalEntities: [],
        dynamicLists: []
    };
    const verbose = true;
    const showAllIntents = true;

    // Note be sure to specify, using the slot_name parameter, whether your application is in staging or production.
    const predictionResult = await luisRuntimeClient.prediction
        .getSlotPrediction(luisAppID, luisSlotName, predictionRequest, { verbose, showAllIntents });

    console.log(JSON.stringify(predictionResult));
}
// </predict>

// <Main>
const quickstart = async () => {
    await predict();
}

try {
    quickstart();
} catch (error) {
    console.log(error);
}
// </Main>
```

## <a name="run-the-application"></a>运行应用程序

从应用程序目录使用 `node luis_prediction.js` 命令运行应用程序。

```console
node luis_prediction.js
```

预测结果返回一个 JSON 对象：

```console
{
   "query":"turn on all lights",
   "prediction":{
      "topIntent":"HomeAutomation.TurnOn",
      "intents":{
         "HomeAutomation.TurnOn":{
            "score":0.5375382
         },
         "None":{
            "score":0.08687421
         },
         "HomeAutomation.TurnOff":{
            "score":0.0207554
         }
      },
      "entities":{
         "HomeAutomation.Operation":[
            "on"
         ],
         "$instance":{
            "HomeAutomation.Operation":[
               {
                  "type":"HomeAutomation.Operation",
                  "text":"on",
                  "startIndex":5,
                  "length":2,
                  "score":0.724984169,
                  "modelTypeId":-1,
                  "modelType":"Unknown",
                  "recognitionSources":[
                     "model"
                  ]
               }
            ]
         }
      }
   }
}
```


## <a name="clean-up-resources"></a>清理资源

完成预测后，请删除文件及其子目录，对此快速入门中的工作进行清理。

