---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
origin.date: 04/03/2020
ms.date: 06/19/2019
ms.author: v-tawe
ms.openlocfilehash: 07fdd7419c4069075ec89868e5d5bf5ec5f29520
ms.sourcegitcommit: d24e12d49708bbe78db450466eb4fccbc2eb5f99
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85613367"
---
## <a name="prerequisites"></a>先决条件

准备工作：

> [!div class="checklist"]
>
> - <a href="https://portal.azure.cn/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">创建 Azure 语音资源<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> - [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md)
> - 需要语音 SDK 版本 1.11.0 或更高版本。

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
        <title>
            Microsoft Cognitive Services Speech SDK JavaScript Quickstart
        </title>
        <meta charset="utf-8" />
        </head>
        <body
        style="font-family:'Helvetica Neue',Helvetica,Arial,sans-serif; font-size:13px;"
        >
        <!-- <uidiv> -->
        <div id="warning">
            <h1 style="font-weight:500;">
            Speech Recognition Speech SDK not found
            (microsoft.cognitiveservices.speech.sdk.bundle.js missing).
            </h1>
        </div>

        <div id="content" style="display:none">
        <table width="100%">
            <tr>
            <td></td>
            <td><h1 style="font-weight:500;">Microsoft Cognitive Services Speech SDK JavaScript Quickstart</h1></td>
            </tr>
            <tr>
            <td align="right"><a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started" target="_blank">Subscription</a>:</td>
            <td><input id="subscriptionKey" type="text" size="40" value="subscription"></td>
            </tr>
            <tr>
            <td align="right">Region</td>
            <td><input id="serviceRegion" type="text" size="40" value="YourServiceRegion"></td>
            </tr>
            <tr>
            <td align="right">Source Language</td>
            <td><select id="languageSourceOptions">
                <option value="ar-EG">Arabic - EG</option>
                <option selected="selected" value="de-DE">German - DE</option>
                <option value="en-US">English - US</option>
                <option value="es-ES">Spanish - ES</option>
                <option value="fr-FR">French - FR</option>
                <option value="hi-IN">Hindi - IN</option>
                <option value="ja-JP">Japanese - JP</option>
                <option value="ko-KR">Korean - KR</option>
                <option value="ru-RU">Russian - RU</option>
                <option value="zh-CN">Chinese - CN</option>
            </select></td>
            </tr>
            <tr>
            <td align="right">Target Language 1</td>
            <td><select id="languageTargetOptions1">
                <option value="ar-EG">Arabic - EG</option>
                <option selected="selected" value="de-DE">German - DE</option>
                <option value="en-US">English - US</option>
                <option value="es-ES">Spanish - ES</option>
                <option value="fr-FR">French - FR</option>
                <option value="hi-IN">Hindi - IN</option>
                <option value="ja-JP">Japanese - JP</option>
                <option value="ko-KR">Korean - KR</option>
                <option value="ru-RU">Russian - RU</option>
                <option value="zh-CN">Chinese - CN</option>
            </select></td>
            </tr>
            <tr>
            <td align="right">Target Language 2</td>
            <td><select id="languageTargetOptions2">
                <option value="ar-EG">Arabic - EG</option>
                <option selected="selected" value="de-DE">German - DE</option>
                <option value="en-US">English - US</option>
                <option value="es-ES">Spanish - ES</option>
                <option value="fr-FR">French - FR</option>
                <option value="hi-IN">Hindi - IN</option>
                <option value="ja-JP">Japanese - JP</option>
                <option value="ko-KR">Korean - KR</option>
                <option value="ru-RU">Russian - RU</option>
                <option value="zh-CN">Chinese - CN</option>
            </select></td>
            </tr>
            <tr>
            <td></td>
            <td><button id="startRecognizeOnceAsyncButton">Start recognition</button></td>
            </tr>
            <tr>
            <td align="right" valign="top">Language 1 Translation</td>
            <td><textarea id="phraseDiv1" style="display: inline-block;width:500px;height:150px"></textarea></td>
            </tr>
            <tr>
            <td align="right" valign="top">Language 2 Translation</td>
            <td><textarea id="phraseDiv2" style="display: inline-block;width:500px;height:150px"></textarea></td>
            </tr>
        </table>
        </div>
  <!-- </uidiv> -->

  <!-- <speechsdkdiv> -->
  <!-- Speech SDK reference sdk. -->
  <script src="microsoft.cognitiveservices.speech.sdk.bundle.js"></script>
  <!-- </speechsdkdiv> -->

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
    var resultsDivs;
    var startRecognizeOnceAsyncButton;

    // subscription key and region for speech services.
    var subscriptionKey, serviceRegion, languageTargetOptions;
    var authorizationToken;
    var SpeechSDK;
    var recognizer;

    document.addEventListener("DOMContentLoaded", function () {
      startRecognizeOnceAsyncButton = document.getElementById("startRecognizeOnceAsyncButton");
      subscriptionKey = document.getElementById("subscriptionKey");
      serviceRegion = document.getElementById("serviceRegion");
      languageSourceOptions = document.getElementById("languageSourceOptions");
            languageTargetOptions = [
                document.getElementById("languageTargetOptions1"),
                document.getElementById("languageTargetOptions2")
            ];
            resultsDivs = [
                document.getElementById("phraseDiv1"),
                document.getElementById("phraseDiv2")
            ]

            startRecognizeOnceAsyncButton.addEventListener("click", function () {
                startRecognizeOnceAsyncButton.disabled = true;
                resultsDivs.forEach(function (elem) {
                    elem.innerHTML = "";
                })

                // if we got an authorization token, use the token. Otherwise use the provided subscription key
                var speechConfig;
                if (authorizationToken) {
                    speechConfig = SpeechSDK.SpeechTranslationConfig.fromAuthorizationToken(authorizationToken, serviceRegion.value);
                } else {
                    if (subscriptionKey.value === "" || subscriptionKey.value === "subscription") {
                        alert("Please enter your Microsoft Cognitive Services Speech subscription key!");
                        startRecognizeOnceAsyncButton.disabled = false;
                        return;
                    }
                    speechConfig = SpeechSDK.SpeechTranslationConfig.fromSubscription(subscriptionKey.value, serviceRegion.value);
                }

                speechConfig.speechRecognitionLanguage = languageSourceOptions.value;
                var languageKeys = {};
                languageTargetOptions.forEach(function (langElem, index) {
                    var language = langElem.value;
                    languageKeys[language.substring(0, 2)] = resultsDivs[index];
                    speechConfig.addTargetLanguage(language);
                });

                var audioConfig = SpeechSDK.AudioConfig.fromDefaultMicrophoneInput();
                recognizer = new SpeechSDK.TranslationRecognizer(speechConfig, audioConfig);

                recognizer.recognizeOnceAsync(
                    function (result) {
                        startRecognizeOnceAsyncButton.disabled = false;
                        for (var key in languageKeys) {
                            var translation = result.translations.get(key);
                            window.console.log(key + ": " + translation);
                            languageKeys[key].innerHTML += translation;
                        }

                        recognizer.close();
                        recognizer = undefined;
                    },
                    function (err) {
                        startRecognizeOnceAsyncButton.disabled = false;
                        resultsDiv[0].innerHTML += err;
                        window.console.log(err);

                        recognizer.close();
                        recognizer = undefined;
                    });
            });

      if (!!window.SpeechSDK) {
        SpeechSDK = window.SpeechSDK;
        startRecognizeOnceAsyncButton.disabled = false;

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

[!INCLUDE [footer](./footer.md)]
