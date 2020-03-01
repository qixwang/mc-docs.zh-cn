---
title: 提取文本匹配实体 - LUIS
titleSuffix: Azure Cognitive Services
description: 了解如何添加有助于 LUIS 标记字词或短语变体的列表实体。
services: cognitive-services
author: lingliw
manager: digimobile
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
origin.date: 09/05/2019
ms.date: 09/23/2019
ms.author: v-lingwu
ms.openlocfilehash: 10d7371c693ab52ae2066e586cd434b0e0d9e14d
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78154862"
---
# <a name="use-a-list-entity-to-increase-entity-detection"></a>使用列表实体提升实体检测 
本文展示了如何使用[列表实体](luis-concept-entity-types.md)提升实体检测。 无需标记列表实体，因为它们与术语完全匹配。  

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

在本文中，学习如何：

> [!div class="checklist"]
> * 创建列表实体 
> * 添加规范化值和同义词
> * 验证改进后的实体标识

## <a name="prerequisites"></a>先决条件

> [!div class="checklist"]
> * 最新版 [Node.js](https://nodejs.org)
> * [HomeAutomation LUIS 应用](luis-get-started-create-app.md)。 如果未创建主自动化应用，请新建一个，并添加预生成的域 HomeAutomation  。 定型并发布应用。 
> * LUIS 应用的 [AuthoringKey](luis-concept-keys.md#authoring-key)、[EndpointKey](luis-concept-keys.md#endpoint-key)（若要多次查询的话）、应用 ID、版本 ID 和[区域](luis-reference-regions.md)。

> [!Tip]
> 如果还没有订阅，可以注册一个[试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)。

本文中的所有代码都可在 [Azure-Samples GitHub 存储库](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-list-entity)中找到。 

## <a name="use-homeautomation-app"></a>使用 HomeAutomation 应用
使用 HomeAutomation 应用，可以控制灯等设备、娱乐系统和供热制冷等环境控制系统。 这些系统有多个不同名称，包括制造商名称、别名、首字母缩略词和行话。 

一种跨不同文化和受众有多个名称的系统就是恒温调节器。 恒温调节器可以控制房屋或建筑物的供热制冷系统。

理想情况下，以下陈述应当会解析为预生成实体 HomeAutomation.Device  ：

|#|话语|标识的实体|score|
|--|--|--|--|
|1|turn on the ac（打开空调）|HomeAutomation.Device -“ac（空调）”|0.8748562|
|2|turn up the heat（打开供热）|HomeAutomation.Device -“heat（供热）”|0.784990132|
|3|make it colder（降温）|||

前两个陈述映射到不同的设备。 第三个陈述“make it colder（降温）”不映射到设备，而是请求获取结果。 LUIS 并不知道，术语“colder（降温）”意味着恒温调节器就是请求获取的设备。 理想情况下，LUIS 应将所有这些陈述都解析为同一设备。 

## <a name="use-a-list-entity"></a>使用列表实体
HomeAutomation.Device 实体非常适用于数量较少的设备或几乎没有名称变体的设备。 对于办公楼或校园，设备名称有很多，HomeAutomation.Device 实体就不适用了。 

在这种情况下，列表实体  很适用，因为办公楼或校园中设备的术语集是已知的，即使这个集合很大，也不例外。 使用列表实体，LUIS 可以接收恒温调节器术语集中的任何可取值，并将它解析为同一个设备“恒温调节器”。 

本文将创建包含恒温调节器的实体列表。 在本文中，恒温调节器的可选名称包括： 

|恒温调节器的可选名称|
|--|
| ac（空调） |
| a/c（空调）|
| a-c（空调）|
|heater（加热器）|
|hot（热）|
|hotter（升温）|
|cold（冷）|
|colder（降温）|

如果 LUIS 需要经常确定新可选名称，最好使用[短语列表](luis-concept-feature.md#how-to-use-phrase-lists)。

## <a name="create-a-list-entity"></a>创建列表实体
创建 Node.js 文件，并将下面的代码复制到其中。 更改 authoringKey、appId、versionId 和 region 值。

```
    /*-----------------------------------------------------------------------------
    This template demonstrates how to use list entities.
    . 
    For a complete walkthrough, see the article at
    https://aka.ms/luis-tutorial-list-entity
    -----------------------------------------------------------------------------*/

    // NPM Dependencies
    var request = require('request-promise');

    // To run this sample, change these constants.

    // Authoring/starter key, available in docs.azure.cn under Account Settings
    const authoringKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

    // ID of your LUIS app to which you want to add an utterance
    const appId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

    // Version number of your LUIS app
    const versionId = "0.1";

    // Region of app
    const region = "chinaeast";

    // Construct HTTP uri
    const uri= `https://${region}.api.cognitive.azure.cn/luis/api/v2.0/apps/${appId}/versions/${versionId}/closedlists`;

    // create list entity
    var addListEntity = async () => {

        try {

            // LUIS model definition for list entity
            var body = {
                "name": "DevicesList",
                "sublists": 
                [
                    {
                        "canonicalForm": "Thermostat",
                        "list": [ "ac", "a/c", "a-c","heater","hot","hotter","cold","colder" ]
                    }
                ]
            }

            // HTTP request
            var myHTTPrequest = request({
                uri: uri,
                method: 'POST',
                headers: {
                    'Ocp-Apim-Subscription-Key': authoringKey
                },
                json: true,
                body: body
            });

            return await myHTTPrequest;

        } catch (err) {
            throw err;
        }
    }
    // MAIN
    addListEntity().then( (response) => {
        // return GUID of list entity model
        console.log(response);
    }).catch(err => {
        console.log(`Error adding list entity:  ${err.message} `);
    });
  ```

运行下面的命令，安装 NPM 依赖项，并通过运行代码来创建列表实体：

```console
npm install && node add-entity-list.js
```

代码运行输出的是列表实体 ID：

```console
026e92b3-4834-484f-8608-6114a83b03a6
```

## <a name="train-the-model"></a>定型模型
定型 LUIS，让新列表能够影响查询结果。 定型过程分为两部分，然后在定型完成后检查状态。 有多个模型的应用可能需要一段时间才能完成定型。 下面的代码先定型应用，然后等到定型成功完成。 此代码使用等待并重试策略，以免发生 429“请求次数过多”错误。 

创建 Node.js 文件，并将下面的代码复制到其中。 更改 authoringKey、appId、versionId 和 region 值。

  ```
    /*-----------------------------------------------------------------------------
    This template demonstrates how to use list entities.
    . 
    For a complete walkthrough, see the article at
    https://aka.ms/luis-tutorial-list-entity
    -----------------------------------------------------------------------------*/

    // NPM Dependencies
    var request = require('request-promise');

    // To run this sample, change these constants.

    // Authoring/starter key, available in docs.azure.cn under Account Settings
    const authoringKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

    // ID of your LUIS app to which you want to add an utterance
    const appId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

    // Version number of your LUIS app
    const versionId = "0.1";

    // Region of app
    const region = "chinaeast";

    // Construct HTTP uri
    const uri= `https://${region}.api.cognitive.azure.cn/luis/api/v2.0/apps/${appId}/versions/${versionId}/closedlists`;

    // create list entity
    var addListEntity = async () => {

        try {

            // LUIS model definition for list entity
            var body = {
                "name": "DevicesList",
                "sublists": 
                [
                    {
                        "canonicalForm": "Thermostat",
                        "list": [ "ac", "a/c", "a-c","heater","hot","hotter","cold","colder" ]
                    }
                ]
            }

            // HTTP request
            var myHTTPrequest = request({
                uri: uri,
                method: 'POST',
                headers: {
                    'Ocp-Apim-Subscription-Key': authoringKey
                },
                json: true,
                body: body
            });

            return await myHTTPrequest;

        } catch (err) {
            throw err;
        }
    }
    // MAIN
    addListEntity().then( (response) => {
        // return GUID of list entity model
        console.log(response);
    }).catch(err => {
        console.log(`Error adding list entity:  ${err.message} `);
    });
  ```

运行下面的命令，通过运行代码来定型应用：

```console
node train.js
```

代码运行输出的是，LUIS 模型的每次定型迭代状态。 执行下面的代码只需要检查一次定型：

```console
1 trained = true
[ { modelId: '2c549f95-867a-4189-9c35-44b95c78b70f',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '5530e900-571d-40ec-9c78-63e66b50c7d4',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '519faa39-ae1a-4d98-965c-abff6f743fe6',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '9671a485-36a9-46d5-aacd-b16d05115415',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '9ef7d891-54ab-48bf-8112-c34dcd75d5e2',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '8e16a660-8781-4abf-bf3d-f296ebe1bf2d',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } } ]

  ```
  ## <a name="publish-the-model"></a>发布模型
  发布模型，让用户可通过终结点使用列表实体。

  创建 Node.js 文件，并将下面的代码复制到其中。 更改 endpointKey、appId 和 region 值。 如果没有计划在配额限制外调用此文件，可以使用 authoringKey。

  ```
  /*-----------------------------------------------------------------------------
  This template demonstrates how to use list entities.
  . 
  For a complete walkthrough, see the article at
  https://aka.ms/luis-tutorial-list-entity
  -----------------------------------------------------------------------------*/

  // NPM Dependencies
  var request = require('request-promise');

  // To run this sample, change these constants.

  // Authoring/starter key, available in docs.azure.cn under Account Settings
  const authoringKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

  // ID of your LUIS app to which you want to add an utterance
  const appId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

  // Version number of your LUIS app
  const versionId = "0.1";

  // Region of app
  const region = "chinaeast";

  // Construct HTTP uri
  const uri= `https://${region}.api.cognitive.azure.cn/luis/api/v2.0/apps/${appId}/publish`;

  // publish
  var publish = async () => {

      try {

          // LUIS publish definition
          var body = {
              "versionId": "0.1",
              "isStaging": false,
              "region": "chinaeast"
          };

          // HTTP request
          var myHTTPrequest = request({
              uri: uri,
              method: 'POST',
              headers: {
                  'Ocp-Apim-Subscription-Key': authoringKey
              },
              json: true,
              body: body
          });

          return await myHTTPrequest;

      } catch (err) {
          throw err;
      }
  }
  // MAIN
  publish().then( (response) => {
      // return GUID of list entity model
      console.log(response);
  }).catch(err => {
      console.log(`Error adding list entity:  ${err.message} `);
  });
```

运行下面的命令，通过运行代码来查询应用：

```console
node publish.js
```

下面的输出包括任何查询的终结点 URL。 实际 JSON 结果包括实际 appID。 

```json
{ 
  versionId: null,
  isStaging: false,
  endpointUrl: 'https://chinaeast2.api.cognitive.azure.cn/luis/v2.0//apps/<appID>',
  region: null,
  assignedEndpointKey: null,
  endpointRegion: 'chinaeast',
  publishedDateTime: '2018-01-29T22:17:38Z' }
}
```

## <a name="query-the-app"></a>查询应用 
通过终结点查询应用，以证明列表实体有助于 LUIS 确定设备类型。

创建 Node.js 文件，并将下面的代码复制到其中。 更改 endpointKey、appId 和 region 值。 如果没有计划在配额限制外调用此文件，可以使用 authoringKey。

```
  /*-----------------------------------------------------------------------------
  This template demonstrates how to use list entities.
  . 
  For a complete walkthrough, see the article at
  https://aka.ms/luis-tutorial-list-entity
  -----------------------------------------------------------------------------*/

  // NPM Dependencies
  var argv = require('yargs').argv;
  var request = require('request-promise');

  // To run this sample, change these constants.

  // endpointKey key - if using a few times - use authoring key, otherwise use endpoint key
  const endpointKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" || argv.endpointKey;

  // ID of your LUIS app to which you want to add an utterance
  const appId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" || argv.appId;

  // Region of app
  const region = "chinaeast" || argv.region;

  // Get query from command line
  // you do NOT need to add quotes around query phrase
  // example: node query.js turn on the lights
  // q: "turn on the lights"
  // verbose: true means results return all intents, not just top intent
  const q = argv._.join(" ");
  const uri= `https://${region}.api.cognitive.azure.cn/luis/v2.0/apps/${appId}?q=${q}&verbose=true`;

  // query endpoint with endpoint key
  var query = async () => {

      try {

          var myHTTPrequest = request({
              uri: uri,
              method: 'GET',
              headers: {
                  'Ocp-Apim-Subscription-Key': endpointKey
              }
          });

          return await myHTTPrequest;

      } catch (err) {
          throw err;
      }
  }

  // MAIN
  query().then( (response) => {
      // return verbose results (all intents, all entities)
      console.log(response);
  }).catch(err => {
      console.log(`Error querying:  ${err.message} `);
  });
```

运行下面的命令，通过运行代码来查询应用：

```console
node train.js
```

输出的是查询结果。 因为此代码向查询字符串添加详细  名称/值对，所以输出包括所有意向及其分数：

```json
{
  "query": "turn up the heat",
  "topScoringIntent": {
    "intent": "HomeAutomation.TurnOn",
    "score": 0.139018849
  },
  "intents": [
    {
      "intent": "HomeAutomation.TurnOn",
      "score": 0.139018849
    },
    {
      "intent": "None",
      "score": 0.120624863
    },
    {
      "intent": "HomeAutomation.TurnOff",
      "score": 0.06746891
    }
  ],
  "entities": [
    {
      "entity": "heat",
      "type": "HomeAutomation.Device",
      "startIndex": 12,
      "endIndex": 15,
      "score": 0.784990132
    },
    {
      "entity": "heat",
      "type": "DevicesList",
      "startIndex": 12,
      "endIndex": 15,
      "resolution": {
        "values": [
          "Thermostat"
        ]
      }
    }
  ]
}
```

特定设备“恒温调节器”  是通过面向结果的“turn up the heat（打开供热）”查询进行标识。 由于应用中仍有原始 HomeAutomation.Device 实体，因此还可以看到它的结果。 

尝试其他两个陈述，看看它们是否也作为“恒温调节器”返回。 

|#|话语|实体|type|value|
|--|--|--|--|--|
|1|turn on the ac（打开空调）| ac（空调） | DevicesList | 恒温调节器|
|2|turn up the heat（打开供热）|heat（供热）| DevicesList |恒温调节器|
|3|make it colder（降温）|colder（降温）|DevicesList|恒温调节器|

## <a name="next-steps"></a>后续步骤

可以创建其他列表实体，将设备位置扩展到房间、楼层或建筑物。 




