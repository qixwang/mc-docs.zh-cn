---
title: 使用 Go 通过 REST 调用获取意向
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/19/2020
origin.date: 06/03/2020
ms.author: diberry
ms.openlocfilehash: 0e2d4301e90215e2d496a34c46da70f8b14c8129
ms.sourcegitcommit: 48b5ae0164f278f2fff626ee60db86802837b0b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85112072"
---
[参考文档](https://dev.cognitive.azure.cn/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c08) | [示例](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/go/LUIS/go-rest-predict/predict.go)

## <a name="prerequisites"></a>先决条件

* [Go](https://golang.org/) 编程语言  
* [Visual Studio Code](https://code.visualstudio.com/)
* 公共应用 ID：`df67dcdb-c37d-46af-88e1-8b97951ca1c2`

## <a name="get-luis-key"></a>获取 LUIS 密钥

[!INCLUDE [Use authoring key for endpoint](../includes/get-key-quickstart.md)]

## <a name="get-intent-programmatically"></a>以编程方式获取意向

使用 Go 查询[预测终结点](https://aka.ms/luis-apim-v3-prediction)并获取预测结果。

1. 创建名为 `predict.go` 的新文件。 添加以下代码：
    
    ```go
    package main
    
    /* Do dependencies */
    import (
        "fmt"
        "net/http"
        "net/url"
        "io/ioutil"
        "log"
    )
    func main() {
        
        // public app
        var appID = "df67dcdb-c37d-46af-88e1-8b97951ca1c2"
        
        // utterance for public app
        var utterance = "turn on all lights"
        
        // YOUR-KEY - your starter or prediction key
        var endpointKey = "YOUR-KEY"
        
        // YOUR-ENDPOINT - example is chinaeast2.api.cognitive.azure.cn
        var endpoint = "YOUR-ENDPOINT"
    
        endpointPrediction(appID, endpointKey, endpoint, utterance)
    }
    func endpointPrediction(appID string, endpointKey string, endpoint string, utterance string) {
    
        var endpointUrl = fmt.Sprintf("https://%s/luis/prediction/v3.0/apps/%s/slots/production/predict?subscription-key=%s&verbose=true&show-all-intents=true&query=%s", endpoint, appID, endpointKey, url.QueryEscape(utterance))
        
        response, err := http.Get(endpointUrl)
    
        if err!=nil {
            // handle error
            fmt.Println("error from Get")
            log.Fatal(err)
        }
        
        response2, err2 := ioutil.ReadAll(response.Body)
    
        if err2!=nil {
            // handle error
            fmt.Println("error from ReadAll")
            log.Fatal(err2)
        }
    
        fmt.Println("response")
        fmt.Println(string(response2))
    }
    ```

1. 请替换以下值：

    * 将 `YOUR-KEY` 替换为初学者密钥。
    * 将 `YOUR-ENDPOINT` 替换为终结点。 例如，`chinaeast2.api.cognitive.azure.cn`。

1. 在创建该文件的同一目录中，在命令提示符下输入以下命令来编译 Go 文件：

    ```console
    go build predict.go
    ```  

1. 通过在命令提示符下输入以下文本从命令行运行 Go 应用程序： 

    ```console
    go run predict.go
    ```
    
    命令提示符响应为： 
    
    ```console
    appID has value df67dcdb-c37d-46af-88e1-8b97951ca1c2
    endpointKey has value a7b206911f714e71a1ddae36928a61cc
    endpoint has value chinaeast2.api.cognitive.azure.cn
    utterance has value turn on all lights
    response
    {"query":"turn on all lights","prediction":{"topIntent":"HomeAutomation.TurnOn","intents":{"HomeAutomation.TurnOn":{"score":0.5375382},"None":{"score":0.08687421},"HomeAutomation.TurnOff":{"score":0.0207554}},"entities":{"HomeAutomation.Operation":["on"],"$instance":{"HomeAutomation.Operation":[{"type":"HomeAutomation.Operation","text":"on","startIndex":5,"length":2,"score":0.724984169,"modelTypeId":-1,"modelType":"Unknown","recognitionSources":["model"]}]}}}}
    ```

    JSON 格式以提高可读性：

    ```json
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ],
                "$instance": {
                    "HomeAutomation.Operation": [
                        {
                            "type": "HomeAutomation.Operation",
                            "text": "on",
                            "startIndex": 5,
                            "length": 2,
                            "score": 0.724984169,
                            "modelTypeId": -1,
                            "modelType": "Unknown",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```


## <a name="luis-keys"></a>LUIS 密钥

[!INCLUDE [Use authoring key for endpoint](../includes/starter-key-explanation.md)]

## <a name="clean-up-resources"></a>清理资源

完成本快速入门后，请从文件系统中删除该文件。 

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [添加言语并进行训练](../get-started-get-model-rest-apis.md)