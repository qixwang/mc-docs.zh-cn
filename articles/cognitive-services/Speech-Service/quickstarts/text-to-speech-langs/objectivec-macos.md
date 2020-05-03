---
title: 快速入门：合成语音，Objective-C - 语音服务
titleSuffix: Azure Cognitive Services
description: 了解如何在 macOS 上使用语音 SDK 通过 Objective-C 合成语音
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
origin.date: 12/09/2019
ms.date: 01/27/2020
ms.author: v-tawe
ms.openlocfilehash: 8460e5e2ba31317c4e0ca42c31c247940564fafc
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134470"
---
# <a name="quickstart-synthesize-speech-in-objective-c-on-macos-using-the-speech-sdk"></a>快速入门：在 macOS 上使用语音 SDK 通过 Objective-C 合成语音

本文介绍如何使用认知服务语音 SDK 在Objective-C 中创建一个 macOS 应用，以便从文本合成语音并使用默认音频输出播放它。

## <a name="prerequisites"></a>先决条件

在开始之前，请满足以下一系列先决条件：

* 语音服务的[订阅密钥](~/articles/cognitive-services/Speech-Service/get-started.md)。
* 装有 [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) 或更高版本以及 macOS 10.13 或更高版本的 macOS 计算机
* 需要语音 SDK 版本 1.10.0 或更高版本。

## <a name="get-the-speech-sdk-for-macos"></a>获取适用于 macOS 的语音 SDK

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

请注意，本教程不适用于 1.7.0 之前的 SDK 版本。

适用于 Mac 的认知服务语音 SDK 目前以框架捆绑包的形式分发。
可在 Xcode 项目将它作为 [CocoaPod](https://cocoapods.org/) 使用，或者从 https://aka.ms/csspeech/macosbinary 下载，然后手动与它建立链接。 本指南使用 CocoaPod。

## <a name="create-an-xcode-project"></a>创建 Xcode 项目

启动 Xcode，然后通过单击“文件” > “新建” > “项目”来启动新项目。   
在模板选择对话框中，选择“Cocoa 应用”模板。

在随后的对话框中，进行以下选择：

1. 项目选项对话框
    1. 为快速入门应用输入一个名称，例如 `helloworld`。
    1. 如果已经有 Apple 开发人员帐户，请输入相应的组织名称和组织标识符。 可以直接选取任意名称（例如 `testorg`）进行测试。 若要对应用进行签名，需要适当的预配配置文件。 有关详细信息，请参阅 [Apple 开发人员站点](https://developer.apple.com/)。
    1. 确保选择 Objective-C 作为项目的语言。
    1. 禁用使用情节提要和创建基于文档的应用程序的复选框。 将以编程方式创建示例应用的简单 UI。
    1. 禁用所有用于测试和核心数据的复选框。
    ![项目设置](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-project-settings.png)
1. 选择项目目录
    1. 选择用于放置该项目的目录。 这样会在主目录中创建一个 `helloworld` 目录，其中包含 Xcode 项目的所有文件。
    1. 禁止创建适用于此示例项目的 Git 存储库。
1. 设置网络访问的权利。 单击左侧概述中第一行内的应用名称转到应用配置，然后选择“功能”选项卡。
    1. 为该应用启用“应用沙盒”设置。
    1. 启用“传出连接”访问权限对应的复选框。
    ![沙盒设置](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-sandbox-tts.png)
1. 关闭 Xcode 项目。 稍后在设置 CocoaPods 后，将使用该项目的另一个实例。

## <a name="install-the-sdk-as-a-cocoapod"></a>安装用作 CocoaPod 的 SDK

1. 根据[安装说明](https://guides.cocoapods.org/using/getting-started.html)中所述，安装 CocoaPod 依赖项管理器。
1. 导航到示例应用所在的目录 (`helloworld`)。 在该目录中添加一个包含以下内容的名为 `Podfile` 的文本文件：  

    ```objectivec
    target 'helloworld' do
      platform :osx, '10.13'
      pod 'MicrosoftCognitiveServicesSpeech-macOS', '~> 1.7.0'
    end
    ```

1. 在终端中导航到 `helloworld` 目录并运行命令 `pod install`。 这会生成一个 `helloworld.xcworkspace` Xcode 工作区，其中包含示例应用以及用作依赖项的语音 SDK。 在后续步骤中将使用此工作区。

## <a name="add-the-sample-code"></a>添加示例代码

1. 在 Xcode 中打开 `helloworld.xcworkspace` 工作区。
1. 通过以下方式替换自动生成的 `AppDelegate.m` 文件的内容：  

    ```objectivec
    #import "AppDelegate.h"
    #import <MicrosoftCognitiveServicesSpeech/SPXSpeechApi.h>
    
    @interface AppDelegate ()
    
    @property (weak) IBOutlet NSWindow *window;
    @property (weak) NSButton *button;
    @property (strong) NSTextField *textField;
    @end
    
    @implementation AppDelegate
    
    - (void)applicationDidFinishLaunching:(NSNotification *)aNotification {
        self.button = [NSButton buttonWithTitle:@"Synthesize" target:nil action:nil];
        [self.button setTarget:self];
        [self.button setAction:@selector(buttonPressed:)];
        [self.window.contentView addSubview:self.button];
        
        self.textField = [[NSTextField alloc] initWithFrame:NSMakeRect(100, 200, 200, 20)];
        [self.textField setPlaceholderString:@"Text something to synthesize..."];
        [self.window.contentView addSubview:self.textField];
    }
    
    - (void)buttonPressed:(NSButton *)button {
        // Creates an instance of a speech config with specified subscription key and service region.
        // Replace with your own subscription key and service region (e.g., "chinaeast2").
        NSString *speechHost = @"https://YourServiceRegion.tts.speech.azure.cn/";
        NSString *speechKey = @"YourSubscriptionKey";

        SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithHost:speechHost subscription:speechKey];
        SPXSpeechSynthesizer *speechSynthesizer = [[SPXSpeechSynthesizer alloc] init:speechConfig];
        
        NSLog(@"Start synthesizing...");
        
        SPXSpeechSynthesisResult *speechResult = [speechSynthesizer speakText:[self.textField stringValue]];
        
        // Checks result.
        if (SPXResultReason_Canceled == speechResult.reason) {
            SPXSpeechSynthesisCancellationDetails *details = [[SPXSpeechSynthesisCancellationDetails alloc] initFromCanceledSynthesisResult:speechResult];
            NSLog(@"Speech synthesis was canceled: %@. Did you pass the correct key/region combination?", details.errorDetails);
        } else if (SPXResultReason_SynthesizingAudioCompleted == speechResult.reason) {
            NSLog(@"Speech synthesis was completed");
        } else {
            NSLog(@"There was an error.");
        }
    }
    
    - (void)applicationWillTerminate:(NSNotification *)aNotification {
        // Insert code here to tear down your application
    }
    
    - (BOOL)applicationShouldTerminateAfterLastWindowClosed:(NSApplication *)sender {
        return true;
    }
    
    @end
    ```

1. 将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。
1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)（例如，对于试用订阅，为 `chinaeast2`）。

## <a name="build-and-run-the-sample"></a>生成并运行示例

1. 使调试输出可见（“视图”   > “调试区域”   >   “激活控制台”）。
1. 在菜单中选择“产品” -> “运行”，或者单击“播放”按钮，以生成并运行示例代码。   
1. 在输入一些文本并单击应用中的按钮后，你应该会听到播放的合成音频。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [浏览 GitHub 上的 Objective-C 示例](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

