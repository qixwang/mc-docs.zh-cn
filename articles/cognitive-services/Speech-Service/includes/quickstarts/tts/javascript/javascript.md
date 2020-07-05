---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
origin.date: 04/03/2020
ms.date: 06/19/2019
ms.author: v-tawe
ms.openlocfilehash: d9a983a069213e2f22bb5433f9b03bd909e31ad2
ms.sourcegitcommit: d24e12d49708bbe78db450466eb4fccbc2eb5f99
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85613377"
---
## <a name="prerequisites"></a>先决条件

准备工作：

> [!div class="checklist"]
> * <a href="https://portal.azure.cn/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">创建 Azure 语音资源<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md)
> * 需要语音 SDK 版本 1.11.0 或更高版本。

## <a name="create-a-new-website-folder"></a>新建网站文件夹

新建空文件夹。 如果要在 web 服务器上承载示例，请确保 web 服务器可访问文件夹。

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>将 JavaScript 的语音 SDK 解压缩到文件夹

将语音 SDK 作为 [.zip 包](https://aka.ms/csspeech/jsbrowserpackage)下载，并将其解压缩到新建文件夹。 这导致两个文件（`microsoft.cognitiveservices.speech.sdk.bundle.js` 和 `microsoft.cognitiveservices.speech.sdk.bundle.js.map`）被解压缩。
后一个文件是可选的，可用于调试到 SDK 代码中。

## <a name="create-an-indexhtml-page"></a>创建 index.html 页面

在文件夹中创建名为 `index.html` 的新文件，使用文本编辑器打开此文件。

1. 创建以下 HTML 框架：

    ```html
    <!DOCTYPE html>
    <html>
    <head>
      <title>Microsoft Cognitive Services Speech SDK JavaScript Quickstart</title>
      <meta charset="utf-8" />
    </head>
    <body style="font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px;">
      <!-- <uidiv> -->
      <div id="warning">
        <h1 style="font-weight:500;">Speech Recognition Speech SDK not found (microsoft.cognitiveservices.speech.sdk.bundle.js missing).</h1>
      </div>
      
      <div id="content" style="display:none">
        <table width="100%">
          <tr>
            <td></td>
            <td><h1 style="font-weight:500;">Microsoft Cognitive Services Speech SDK JavaScript Quickstart</h1></td>
          </tr>
          <tr>
            <td align="right"><a href="https://docs.azure.cn/cognitive-services/speech-service/get-started" target="_blank">Subscription</a>:</td>
            <td><input id="subscriptionKey" type="text" size="40" value="subscription"></td>
          </tr>
          <tr>
            <td align="right">Region</td>
            <td><input id="serviceRegion" type="text" size="40" value="YourServiceRegion"></td>
          </tr>
          <tr>
            <td></td>
            <td><button id="startSpeakTextAsyncButton">Start text to speech</button></td>
          </tr>
          <tr>
            <td align="right" valign="top">Input Text</td>
            <td><textarea id="phraseDiv" style="display: inline-block;width:500px;height:200px"></textarea></td>
          </tr>
          <tr>
            <td align="right" valign="top">Result</td>
            <td><textarea id="resultDiv" style="display: inline-block;width:500px;height:100px"></textarea></td>
          </tr>
        </table>
      </div>
      <!-- </uidiv> -->
    
      <!-- <speechsdkref> -->
      <!-- Speech SDK reference sdk. -->
      <script src="microsoft.cognitiveservices.speech.sdk.bundle.js"></script>
      <!-- </speechsdkref> -->
    
      <!-- <authorizationfunction> -->
      <!-- Speech SDK Authorization token -->
      <script>
      // Note: Replace the URL with a valid endpoint to retrieve
      //       authorization tokens for your subscription.
      var authorizationEndpoint = "token.php";
    
      function RequestAuthorizationToken() {
        if (authorizationEndpoint) {
          var a = new XMLHttpRequest();
          a.open("GET", authorizationEndpoint);
          a.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
          a.send("");
          a.onload = function() {
            var token = JSON.parse(atob(this.responseText.split(".")[1]));
            serviceRegion.value = token.region;
            authorizationToken = this.responseText;
            subscriptionKey.disabled = true;
            subscriptionKey.value = "using authorization token (hit F5 to refresh)";
            console.log("Got an authorization token: " + token);
          }
        }
      }
      </script>
      <!-- </authorizationfunction> -->
    
      <!-- <quickstartcode> -->
      <!-- Speech SDK USAGE -->
      <script>
            
        // status fields and start button in UI
        var phraseDiv;
        var resultDiv;
        var startSpeakTextAsyncButton;
    
        // subscription key and region for speech services.
        var subscriptionKey, serviceRegion;
        var authorizationToken;
        var SpeechSDK;
        var synthesizer;
    
        document.addEventListener("DOMContentLoaded", function () {
          startSpeakTextAsyncButton = document.getElementById("startSpeakTextAsyncButton");
          subscriptionKey = document.getElementById("subscriptionKey");
          serviceRegion = document.getElementById("serviceRegion");
          phraseDiv = document.getElementById("phraseDiv");
          resultDiv = document.getElementById("resultDiv");
    
          startSpeakTextAsyncButton.addEventListener("click", function () {
            var soundContext = undefined;
            try {
              var AudioContext = window.AudioContext || window.webkitAudioContext || false;
              if (AudioContext) {
                soundContext = new AudioContext();
              } else {
                alert("AudioContext not supported");
              }
            }
            catch(e){
              window.console.log("no sound context found, no audio output. " + e);
            }
       
            startSpeakTextAsyncButton.disabled = true;
            phraseDiv.innerHTML = "";
    
            // if we got an authorization token, use the token. Otherwise use the provided subscription key
            var speechConfig;
            if (authorizationToken) {
              speechConfig = SpeechSDK.SpeechConfig.fromAuthorizationToken(authorizationToken, serviceRegion.value);
            } else {
              if (subscriptionKey.value === "" || subscriptionKey.value === "subscription") {
                alert("Please enter your Microsoft Cognitive Services Speech subscription key!");
                startSpeakTextAsyncButton.disabled = false;
                return;
              }
              speechConfig = SpeechSDK.SpeechConfig.fromSubscription(subscriptionKey.value, serviceRegion.value);
            }
    
            synthesizer = new SpeechSDK.SpeechSynthesizer(speechConfig);
    
            var inputText = phraseDiv.value;
            synthesizer.speakTextAsync(
              inputText,      
              function (result) {
                startSpeakTextAsyncButton.disabled = false;
                resultDiv.innerHTML += "Result: ";
                resultDiv.innerHTML += result.text;
                resultDiv.innerHTML += "\n";
    
                window.console.log(result);
                if (result.audioData && soundContext) {
                  var source = soundContext.createBufferSource();
                  soundContext.decodeAudioData(result.audioData, function (newBuffer) {
                    source.buffer = newBuffer;
                    source.connect(soundContext.destination);
                    source.start(0);
                  });
                }
    
                synthesizer.close();
                synthesizer = undefined;
              },
              function (err) {
                startSpeakTextAsyncButton.disabled = false;
                resultDiv.innerHTML += "Error: ";
                resultDiv.innerHTML += err;
                resultDiv.innerHTML += "\n";
                window.console.log(err);
    
                synthesizer.close();
                synthesizer = undefined;
              });
          });
    
          if (!!window.SpeechSDK) {
            SpeechSDK = window.SpeechSDK;
            startSpeakTextAsyncButton.disabled = false;
    
            document.getElementById('content').style.display = 'block';
            document.getElementById('warning').style.display = 'none';
    
            // in case we have a function for getting an authorization token, call it.
            if (typeof RequestAuthorizationToken === "function") {
              RequestAuthorizationToken();
            }
          }
        });
      
      
      </script>
      <!-- </quickstartcode> -->
    </body>
    </html>
    ```

## <a name="create-the-token-source-optional"></a>创建令牌源（可选）

如果要在 web 服务器上承载网页，可以为演示应用程序提供令牌源。
这样一来，订阅密钥永远不会离开服务器，并且用户可以在不输入任何授权代码的情况下使用语音功能。

创建名为 `token.php` 的新文件。 此示例假设 web 服务器支持 PHP 脚本语言。 输入以下代码：

```php
<?php
header('Access-Control-Allow-Origin: ' . $_SERVER['SERVER_NAME']);

// Replace with your own subscription key and service region (e.g., "chinaeast2").
$subscriptionKey = 'YourSubscriptionKey';
$region = 'YourServiceRegion';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'https://' . $region . '.api.cognitive.azure.cn/sts/v1.0/issueToken');
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, '{}');
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: application/json', 'Ocp-Apim-Subscription-Key: ' . $subscriptionKey));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
echo curl_exec($ch);
?>
```

> [!NOTE]
> 授权令牌仅具有有限的生存期。
> 此简化示例不显示如何自动刷新授权令牌。 作为用户，你可以手动重载页面或点击 F5 刷新。

## <a name="build-and-run-the-sample-locally"></a>在本地生成和运行示例

要启动应用，双击 index.html 文件或使用你喜欢的 web 浏览器打开 index.html。 它会提供一个简单的 GUI，允许你输入订阅密钥和[区域](../../../../regions.md)，并触发输入文本的合成。

## <a name="build-and-run-the-sample-via-a-web-server"></a>通过 web 服务器生成并运行示例

若要启动应用，请打开你喜爱的 Web 浏览器并将其指向托管文件夹的公共 URL，输入[区域](../../../../regions.md)，然后触发输入文本的合成。 配置后，它将获取令牌源中的令牌。


## <a name="next-steps"></a>后续步骤

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]