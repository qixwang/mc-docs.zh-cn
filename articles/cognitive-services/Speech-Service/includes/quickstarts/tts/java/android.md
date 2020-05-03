---
title: 快速入门：合成语音，Java (Android) - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何在 Android 上使用语音 SDK 通过 Java 合成语音
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 04/04/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 3de3e1b900191ac95cbf29fee3d83ae2e14fc10b
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82150997"
---
## <a name="prerequisites"></a>先决条件

在开始之前，请务必：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?tabs=android&pivots=programming-language-java)
> * 需要语音 SDK 版本 1.10.0 或更高版本。

## <a name="create-user-interface"></a>创建用户界面

我们将创建应用程序的基本用户界面。 编辑主活动的布局 `activity_main.xml` 。 一开始的布局包含一个带应用程序名称的标题栏，以及包含“Hello World!”文本的 TextView。

1. 单击 TextView 元素。 将右上角的 ID 属性更改为 `outputMessage`，然后将其拖到下方屏幕。 删除其文本。

1. 从 `activity_main.xml` 窗口左上角的调色板中，将“按钮”拖到文本上方的空白区域。

1. 在右侧的按钮属性中，在 `onClick` 属性的值中输入 `onSpeechButtonClicked`。 我们将使用此名称编写一个方法来处理按钮事件。  在右上角将其 ID 属性更改为 `button`。

1. 将纯文本拖到按钮上方的空间中；将其 ID 属性更改为 `speakText`，并将文本属性更改为 `Hi there!`。

1. 若要推断布局约束，请使用设计器顶部的魔术棒图标。


    ![魔术棒图标的屏幕截图](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-10-infer-layout-constraints.png)

现在，UI 的文本和图形表示形式应如下所示：

![](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-11-2-tts-gui.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/outputMessage"
        android:layout_width="352dp"
        android:layout_height="293dp"
        android:layout_marginStart="16dp"
        android:layout_marginEnd="8dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.0"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.74" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="48dp"
        android:layout_marginStart="16dp"
        android:layout_marginTop="8dp"
        android:onClick="onSpeechButtonClicked"
        android:text="Speak"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/speakText" />

    <EditText
        android:id="@+id/speakText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:layout_marginTop="16dp"
        android:ems="10"
        android:inputType="text"
        android:text="Hi there!"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</android.support.constraint.ConstraintLayout>
```

## <a name="add-sample-code"></a>添加示例代码

1. 打开源文件 `MainActivity.java`。 将此文件中的所有代码替换为以下内容。

    ```java
    package com.microsoft.cognitiveservices.speech.samples.quickstart;
    
    import android.support.v4.app.ActivityCompat;
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import android.util.Log;
    import android.view.View;
    import android.widget.EditText;
    import android.widget.TextView;
    
    import com.microsoft.cognitiveservices.speech.ResultReason;
    import com.microsoft.cognitiveservices.speech.SpeechConfig;
    import com.microsoft.cognitiveservices.speech.SpeechSynthesisCancellationDetails;
    import com.microsoft.cognitiveservices.speech.SpeechSynthesisResult;
    import com.microsoft.cognitiveservices.speech.SpeechSynthesizer;
    
    import static android.Manifest.permission.*;
    
    public class MainActivity extends AppCompatActivity {
    
        // Replace below with your own subscription key
        private static String speechSubscriptionKey = "YourSubscriptionKey";
    
        private SpeechConfig speechConfig;
        private SpeechSynthesizer synthesizer;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            // Note: we need to request the permissions
            int requestCode = 5; // unique code for the permission request
            ActivityCompat.requestPermissions(MainActivity.this, new String[]{INTERNET}, requestCode);
    
            // Initialize speech synthesizer and its dependencies
            // Replace below with your own service region (e.g., "chinaeast2").
            speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
            
            assert(speechConfig != null);
    
            synthesizer = new SpeechSynthesizer(speechConfig);
            assert(synthesizer != null);
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
    
            // Release speech synthesizer and its dependencies
            synthesizer.close();
            speechConfig.close();
        }
    
        public void onSpeechButtonClicked(View v) {
            TextView outputMessage = this.findViewById(R.id.outputMessage);
            EditText speakText = this.findViewById(R.id.speakText);
    
            try {
                // Note: this will block the UI thread, so eventually, you want to register for the event 
                SpeechSynthesisResult result = synthesizer.SpeakText(speakText.getText().toString());
                assert(result != null);
    
                if (result.getReason() == ResultReason.SynthesizingAudioCompleted) {
                    outputMessage.setText("Speech synthesis succeeded.");
                }
                else if (result.getReason() == ResultReason.Canceled) {
                    String cancellationDetails =
                            SpeechSynthesisCancellationDetails.fromResult(result).toString();
                    outputMessage.setText("Error synthesizing. Error detail: " +
                            System.lineSeparator() + cancellationDetails +
                            System.lineSeparator() + "Did you update the subscription info?");
                }
    
                result.close();
            } catch (Exception ex) {
                Log.e("SpeechSDKDemo", "unexpected " + ex.getMessage());
                assert(false);
            }
        }
    }
    ```

   * 如前所述，`onSpeechButtonClicked` 方法是按钮单击处理程序。 按下某个按钮会触发语音合成。

1. 在同一文件中，将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。

1. 还将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)（例如，对于试用订阅，为 `chinaeast2`）。

## <a name="build-and-run-the-app"></a>生成并运行应用

1. 将 Android 设备连接到开发电脑。 确保已在设备上启用[开发模式和 USB 调试](https://developer.android.com/studio/debug/dev-options)。 或者，创建一个 [Android 仿真器](https://developer.android.com/studio/run/emulator)。

1. 若要生成应用程序，请按 Ctrl+F9，或者从菜单栏中选择“生成” > “生成项目”   。

1. 若要启动应用程序，请按 Shift+F10 或选择“运行” > “运行‘应用’”   。

1. 在出现的部署目标窗口中，选择你的 Android 设备或仿真器。

   ![“选择部署目标”窗口的屏幕截图](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-12-deploy.png)

输入文本，然后按应用程序中的按钮进入语音合成环节。 你将听到默认讲述人的合成音频，并在屏幕上看到 `speech synthesis succeeded` 信息。

![Android 应用程序的屏幕截图](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-13-2-gui-on-device-tts.png)

## <a name="next-steps"></a>后续步骤

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

<!-- ## See also -->

<!-- - [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) -->
<!-- - [Record custom voice samples](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md) -->
