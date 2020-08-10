---
title: include 文件
description: include 文件
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 08/07/2020
ms.topic: include
ms.custom: include file
ms.author: v-johya
origin.date: 05/28/2020
ms.openlocfilehash: 35c5951606190a60c1062b7aefd7d53d158238f1
ms.sourcegitcommit: caa18677adb51b5321ad32ae62afcf92ac00b40b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2020
ms.locfileid: "88024218"
---
使用适用于 Python 的语言理解 (LUIS) 预测客户端库可以：

* 按槽获取预测
* 按版本获取预测

[参考文档](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/index?view=azure-python) | [库源代码](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis) | [预测运行时包 (PyPi)](https://pypi.org/project/azure-cognitiveservices-language-luis/) | [示例](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py)

## <a name="prerequisites"></a>先决条件

* 语言理解 (LUIS) 门户帐户 - [免费创建](https://luis.azure.cn)
* [Python 3.x](https://www.python.org/)
* LUIS 应用 ID - 使用 `df67dcdb-c37d-46af-88e1-8b97951ca1c2` 的公共 IoT 应用 ID。 快速入门代码中使用的用户查询特定于该应用。

## <a name="setting-up"></a>设置

### <a name="get-your-language-understanding-luis-runtime-key"></a>获取语言理解 (LUIS) 运行时密钥

通过创建 LUIS 运行时资源，获取[运行时密钥](../luis-how-to-azure-subscription.md)。 保留密钥和密钥的终结点，以便用于下一步。

### <a name="create-a-new-python-file"></a>创建新的 Python 文件

在偏好的编辑器或 IDE 中创建名为 `prediction_quickstart.py` 的新 Python 文件。

### <a name="install-the-sdk"></a>安装 SDK

在应用程序目录中，使用以下命令安装适用于 Python 的语言理解 (LUIS) 预测运行时客户端库：

```python
python -m pip install azure-cognitiveservices-language-luis
```

## <a name="object-model"></a>对象模型

语言理解 (LUIS) 预测运行时客户端是对 Azure 进行身份验证的 [LUISRuntimeClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-python) 对象，其中包含资源密钥。

创建客户端后，可以使用此客户端访问如下所述的功能：

* 按[过渡槽或生产槽](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)进行预测
* 按[版本](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-version-prediction-app-id--version-id--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)进行预测

## <a name="code-examples"></a>代码示例

以下代码片段演示如何使用适用于 Python 的语言理解 (LUIS) 预测运行时客户端库来执行以下操作：

* [按槽进行预测](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>添加依赖项

在偏好的编辑器或 IDE 中打开项目目录中的 `prediction_quickstart.py` 文件。 添加以下依赖项：

```python
# Azure Language Understanding (LUIS) - Build App
#
# This script queries a public LUIS app for IoT using the Python
# LUIS SDK.
#
# This script requires the Cognitive Services LUIS Python module:
#     python -m pip install azure-cognitiveservices-language-luis
#
# This script runs under Python 3.4 or later.

# For more information about LUIS, see
#
# https://luis.azure.cn/welcome
# /cognitive-services/luis

# <Dependencies>
from azure.cognitiveservices.language.luis.runtime import LUISRuntimeClient
from msrest.authentication import CognitiveServicesCredentials

import datetime, json, os, time
# </Dependencies>

# <AuthorizationVariables>
runtime_key = 'REPLACE-WITH-YOUR-ASSIGNED-PREDICTION-KEY'
print("runtime_key: {}".format(runtime_key))

runtime_endpoint = 'https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.cn'
print("runtime_endpoint: {}".format(runtime_endpoint))
# </AuthorizationVariables>

# <OtherVariables>
# Use public app ID or replace with your own trained and published app's ID
# to query your own app
# public appID = 'df67dcdb-c37d-46af-88e1-8b97951ca1c2'
luisAppID = 'REPLACE-WITH-YOUR-LUIS_APP_ID'
print("luisAppID: {}".format(luisAppID))

# `production` or `staging`
luisSlotName = 'production'
print("luisSlotName: {}".format(luisSlotName))
# </OtherVariables>


# <Client>
# Instantiate a LUIS runtime client
clientRuntime = LUISRuntimeClient(runtime_endpoint, CognitiveServicesCredentials(runtime_key))
# </Client>


# <predict>
def predict(app_id, slot_name):

    request = { "query" : "turn on all lights" }

    # Note be sure to specify, using the slot_name parameter, whether your application is in staging or production.
    response = clientRuntime.prediction.get_slot_prediction(app_id=app_id, slot_name=slot_name, prediction_request=request)

    print("Top intent: {}".format(response.prediction.top_intent))
    print("Sentiment: {}".format (response.prediction.sentiment))
    print("Intents: ")

    for intent in response.prediction.intents:
        print("\t{}".format (json.dumps (intent)))
    print("Entities: {}".format (response.prediction.entities))
# </predict>

predict(luisAppID, luisSlotName)
```

## <a name="authenticate-the-client"></a>验证客户端

1. 为自己所需的 LUIS 信息创建变量：预测密钥和终结点。

```python
# Azure Language Understanding (LUIS) - Build App
#
# This script queries a public LUIS app for IoT using the Python
# LUIS SDK.
#
# This script requires the Cognitive Services LUIS Python module:
#     python -m pip install azure-cognitiveservices-language-luis
#
# This script runs under Python 3.4 or later.

# For more information about LUIS, see
#
# https://luis.azure.cn/welcome
# /cognitive-services/luis

# <Dependencies>
from azure.cognitiveservices.language.luis.runtime import LUISRuntimeClient
from msrest.authentication import CognitiveServicesCredentials

import datetime, json, os, time
# </Dependencies>

# <AuthorizationVariables>
runtime_key = 'REPLACE-WITH-YOUR-ASSIGNED-PREDICTION-KEY'
print("runtime_key: {}".format(runtime_key))

runtime_endpoint = 'https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.cn'
print("runtime_endpoint: {}".format(runtime_endpoint))
# </AuthorizationVariables>

# <OtherVariables>
# Use public app ID or replace with your own trained and published app's ID
# to query your own app
# public appID = 'df67dcdb-c37d-46af-88e1-8b97951ca1c2'
luisAppID = 'REPLACE-WITH-YOUR-LUIS_APP_ID'
print("luisAppID: {}".format(luisAppID))

# `production` or `staging`
luisSlotName = 'production'
print("luisSlotName: {}".format(luisSlotName))
# </OtherVariables>


# <Client>
# Instantiate a LUIS runtime client
clientRuntime = LUISRuntimeClient(runtime_endpoint, CognitiveServicesCredentials(runtime_key))
# </Client>


# <predict>
def predict(app_id, slot_name):

    request = { "query" : "turn on all lights" }

    # Note be sure to specify, using the slot_name parameter, whether your application is in staging or production.
    response = clientRuntime.prediction.get_slot_prediction(app_id=app_id, slot_name=slot_name, prediction_request=request)

    print("Top intent: {}".format(response.prediction.top_intent))
    print("Sentiment: {}".format (response.prediction.sentiment))
    print("Intents: ")

    for intent in response.prediction.intents:
        print("\t{}".format (json.dumps (intent)))
    print("Entities: {}".format (response.prediction.entities))
# </predict>

predict(luisAppID, luisSlotName)
```

1. 为设置为公共 IoT 应用 `df67dcdb-c37d-46af-88e1-8b97951ca1c2` 的应用 ID 创建变量。 创建用于设置 `production` 发布槽的变量。

```python
# Azure Language Understanding (LUIS) - Build App
#
# This script queries a public LUIS app for IoT using the Python
# LUIS SDK.
#
# This script requires the Cognitive Services LUIS Python module:
#     python -m pip install azure-cognitiveservices-language-luis
#
# This script runs under Python 3.4 or later.

# For more information about LUIS, see
#
# https://luis.azure.cn/welcome
# /cognitive-services/luis

# <Dependencies>
from azure.cognitiveservices.language.luis.runtime import LUISRuntimeClient
from msrest.authentication import CognitiveServicesCredentials

import datetime, json, os, time
# </Dependencies>

# <AuthorizationVariables>
runtime_key = 'REPLACE-WITH-YOUR-ASSIGNED-PREDICTION-KEY'
print("runtime_key: {}".format(runtime_key))

runtime_endpoint = 'https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.cn'
print("runtime_endpoint: {}".format(runtime_endpoint))
# </AuthorizationVariables>

# <OtherVariables>
# Use public app ID or replace with your own trained and published app's ID
# to query your own app
# public appID = 'df67dcdb-c37d-46af-88e1-8b97951ca1c2'
luisAppID = 'REPLACE-WITH-YOUR-LUIS_APP_ID'
print("luisAppID: {}".format(luisAppID))

# `production` or `staging`
luisSlotName = 'production'
print("luisSlotName: {}".format(luisSlotName))
# </OtherVariables>


# <Client>
# Instantiate a LUIS runtime client
clientRuntime = LUISRuntimeClient(runtime_endpoint, CognitiveServicesCredentials(runtime_key))
# </Client>


# <predict>
def predict(app_id, slot_name):

    request = { "query" : "turn on all lights" }

    # Note be sure to specify, using the slot_name parameter, whether your application is in staging or production.
    response = clientRuntime.prediction.get_slot_prediction(app_id=app_id, slot_name=slot_name, prediction_request=request)

    print("Top intent: {}".format(response.prediction.top_intent))
    print("Sentiment: {}".format (response.prediction.sentiment))
    print("Intents: ")

    for intent in response.prediction.intents:
        print("\t{}".format (json.dumps (intent)))
    print("Entities: {}".format (response.prediction.entities))
# </predict>

predict(luisAppID, luisSlotName)
```


1. 使用密钥创建 credentials 对象，并在终结点中使用该对象创建一个 [LUISRuntimeClientConfiguration](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclientconfiguration?view=azure-python) 对象。

```python
# Azure Language Understanding (LUIS) - Build App
#
# This script queries a public LUIS app for IoT using the Python
# LUIS SDK.
#
# This script requires the Cognitive Services LUIS Python module:
#     python -m pip install azure-cognitiveservices-language-luis
#
# This script runs under Python 3.4 or later.

# For more information about LUIS, see
#
# https://luis.azure.cn/welcome
# /cognitive-services/luis

# <Dependencies>
from azure.cognitiveservices.language.luis.runtime import LUISRuntimeClient
from msrest.authentication import CognitiveServicesCredentials

import datetime, json, os, time
# </Dependencies>

# <AuthorizationVariables>
runtime_key = 'REPLACE-WITH-YOUR-ASSIGNED-PREDICTION-KEY'
print("runtime_key: {}".format(runtime_key))

runtime_endpoint = 'https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.cn'
print("runtime_endpoint: {}".format(runtime_endpoint))
# </AuthorizationVariables>

# <OtherVariables>
# Use public app ID or replace with your own trained and published app's ID
# to query your own app
# public appID = 'df67dcdb-c37d-46af-88e1-8b97951ca1c2'
luisAppID = 'REPLACE-WITH-YOUR-LUIS_APP_ID'
print("luisAppID: {}".format(luisAppID))

# `production` or `staging`
luisSlotName = 'production'
print("luisSlotName: {}".format(luisSlotName))
# </OtherVariables>


# <Client>
# Instantiate a LUIS runtime client
clientRuntime = LUISRuntimeClient(runtime_endpoint, CognitiveServicesCredentials(runtime_key))
# </Client>


# <predict>
def predict(app_id, slot_name):

    request = { "query" : "turn on all lights" }

    # Note be sure to specify, using the slot_name parameter, whether your application is in staging or production.
    response = clientRuntime.prediction.get_slot_prediction(app_id=app_id, slot_name=slot_name, prediction_request=request)

    print("Top intent: {}".format(response.prediction.top_intent))
    print("Sentiment: {}".format (response.prediction.sentiment))
    print("Intents: ")

    for intent in response.prediction.intents:
        print("\t{}".format (json.dumps (intent)))
    print("Entities: {}".format (response.prediction.entities))
# </predict>

predict(luisAppID, luisSlotName)
```

## <a name="get-prediction-from-runtime"></a>从运行时获取预测

添加以下方法以创建对预测运行时的请求。

用户言语是 [prediction_request](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-python) 对象的一部分。

**[get_slot_prediction](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)** 方法需要多个参数，如应用 ID、槽名称，以及用于履行请求的预测请求对象。 其他选项（如详细、显示所有意向和日志）都是可选的。 该请求返回 [PredictionResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionresponse?view=azure-python) 对象。

```python
# Azure Language Understanding (LUIS) - Build App
#
# This script queries a public LUIS app for IoT using the Python
# LUIS SDK.
#
# This script requires the Cognitive Services LUIS Python module:
#     python -m pip install azure-cognitiveservices-language-luis
#
# This script runs under Python 3.4 or later.

# For more information about LUIS, see
#
# https://luis.azure.cn/welcome
# /cognitive-services/luis

# <Dependencies>
from azure.cognitiveservices.language.luis.runtime import LUISRuntimeClient
from msrest.authentication import CognitiveServicesCredentials

import datetime, json, os, time
# </Dependencies>

# <AuthorizationVariables>
runtime_key = 'REPLACE-WITH-YOUR-ASSIGNED-PREDICTION-KEY'
print("runtime_key: {}".format(runtime_key))

runtime_endpoint = 'https://REPLACE-WITH-YOUR-RESOURCE-NAME.cognitiveservices.azure.cn'
print("runtime_endpoint: {}".format(runtime_endpoint))
# </AuthorizationVariables>

# <OtherVariables>
# Use public app ID or replace with your own trained and published app's ID
# to query your own app
# public appID = 'df67dcdb-c37d-46af-88e1-8b97951ca1c2'
luisAppID = 'REPLACE-WITH-YOUR-LUIS_APP_ID'
print("luisAppID: {}".format(luisAppID))

# `production` or `staging`
luisSlotName = 'production'
print("luisSlotName: {}".format(luisSlotName))
# </OtherVariables>


# <Client>
# Instantiate a LUIS runtime client
clientRuntime = LUISRuntimeClient(runtime_endpoint, CognitiveServicesCredentials(runtime_key))
# </Client>


# <predict>
def predict(app_id, slot_name):

    request = { "query" : "turn on all lights" }

    # Note be sure to specify, using the slot_name parameter, whether your application is in staging or production.
    response = clientRuntime.prediction.get_slot_prediction(app_id=app_id, slot_name=slot_name, prediction_request=request)

    print("Top intent: {}".format(response.prediction.top_intent))
    print("Sentiment: {}".format (response.prediction.sentiment))
    print("Intents: ")

    for intent in response.prediction.intents:
        print("\t{}".format (json.dumps (intent)))
    print("Entities: {}".format (response.prediction.entities))
# </predict>

predict(luisAppID, luisSlotName)
```

## <a name="main-code-for-the-prediction"></a>预测的主代码

使用以下主方法将变量和方法结合在一起，以获取预测。

```python
predict(luisAppID, luisSlotName)
```
## <a name="run-the-application"></a>运行应用程序

从应用程序目录使用 `python prediction_quickstart.py` 命令运行应用程序。

```console
python prediction_quickstart.py
```

快速入门控制台将显示输出：

```console
Top intent: HomeAutomation.TurnOn
Sentiment: None
Intents:
        "HomeAutomation.TurnOn"
Entities: {'HomeAutomation.Operation': ['on']}
```

## <a name="clean-up-resources"></a>清理资源

完成预测后，请删除文件及其子目录，对此快速入门中的工作进行清理。

