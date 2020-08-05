---
title: 快速入门：上使用语音 SDK 通过 Objective-C 合成语音
titleSuffix: Azure Cognitive Services
description: 了解如何在 iOS 上使用语音 SDK 通过 Objective-C 合成语音
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
origin.date: 06/25/2020
ms.date: 08/03/2020
ms.author: v-tawe
ms.openlocfilehash: a73de3afcf238f0c0ee7522942407243f1ce2ffd
ms.sourcegitcommit: 3821704fee67315badba49cf628af2aa68d98f28
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/03/2020
ms.locfileid: "87516128"
---
# <a name="quickstart-synthesize-speech-in-objective-c-on-ios-using-the-speech-sdk"></a>快速入门：在 iOS 上使用语音 SDK 通过 Objective-C 合成语音

本文介绍如何使用认知服务语音 SDK 从文本合成语音，从而通过 Objective-C 创建 iOS 应用。

## <a name="prerequisites"></a>先决条件

在开始之前，请满足以下一系列先决条件：

* 语音服务的[订阅密钥](~/articles/cognitive-services/Speech-Service/get-started.md)。
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) 或更高版本的 macOS 计算机
* 目标设置为 iOS 9.3 版或更高版本
* 需要语音 SDK 1.11.0 或更高版本。

## <a name="get-the-speech-sdk-for-ios"></a>获取用于 iOS 的语音 SDK

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]
用于 iOS 的认知服务语音 SDK 目前以 Cocoa Framework 形式分发。
可在 Xcode 项目将它作为 [CocoaPod](https://cocoapods.org/) 使用，或者从 https://aka.ms/csspeech/iosbinary 下载，然后手动与它建立链接。 本指南使用 CocoaPod。

## <a name="create-an-xcode-project"></a>创建 Xcode 项目

启动 Xcode，然后通过单击“文件” > “新建” > “项目”来启动新项目。
在模板选择对话框中，选择“iOS 单一视图应用”模板。

在随后的对话框中，进行以下选择：

1. 项目选项对话框
    1. 为快速入门应用输入一个名称，例如 `helloworld`。
    1. 如果已经有 Apple 开发人员帐户，请输入相应的组织名称和组织标识符。 可以直接选取任意名称（例如 `testorg`）进行测试。 若要对应用进行签名，需要适当的预配配置文件。 有关详细信息，请参阅 [Apple 开发人员站点](https://developer.apple.com/)。
    1. 确保选择 Objective-C 作为项目的语言。
    1. 禁用所有用于测试和核心数据的复选框。
    ![项目设置](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-project-settings.png)
1. 选择项目目录
    1. 选择用于放置项目的主目录。 这样会在主目录中创建一个 `helloworld` 目录，其中包含 Xcode 项目的所有文件。
    1. 禁止创建适用于此示例项目的 Git 存储库。

## <a name="install-the-sdk-as-a-cocoapod"></a>安装用作 CocoaPod 的 SDK

1. 根据[安装说明](https://guides.cocoapods.org/using/getting-started.html)中所述，安装 CocoaPod 依赖项管理器。
1. 导航到示例应用所在的目录 (`helloworld`)。 在该目录中添加一个包含以下内容的名为 `Podfile` 的文本文件：  

    ```objectivec
    target 'helloworld' do
        platform :ios, '9.3'
        pod 'MicrosoftCognitiveServicesSpeech-iOS', '~> 1.7'
        use_frameworks!
    end
    ```

1. 在终端中导航到 `helloworld` 目录并运行命令 `pod install`。 这会生成一个 `helloworld.xcworkspace` Xcode 工作区，其中包含示例应用以及用作依赖项的语音 SDK。 在后续步骤中将使用此工作区。

## <a name="add-the-sample-code"></a>添加示例代码

1. 在 Xcode 中打开 `helloworld.xcworkspace` 工作区。
1. 通过以下方式替换自动生成的 `AppDelegate.m` 文件的内容：  

    ```objectivec
    #import "AppDelegate.h"
    #import "ViewController.h"
    
    @interface AppDelegate ()
    
    @end
    
    @implementation AppDelegate
    
    
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
        ViewController *controller = [[ViewController alloc] init];
        self.window.rootViewController = controller;
        [self.window makeKeyAndVisible];
        
        return YES;
    }
    
    @end
    ```

1. 通过以下方式替换自动生成的 `ViewController.m` 文件的内容：  

    ```objectivec
    #import "ViewController.h"
    #import <AVFoundation/AVFoundation.h>
    #import <MicrosoftCognitiveServicesSpeech/SPXSpeechApi.h>
    
    @interface ViewController () {
        NSString *inputText;
        
        NSString *speechKey;
        NSString *serviceRegion;
    }
    
    @property (strong, nonatomic) IBOutlet UITextField *inputField;
    @property (strong, nonatomic) IBOutlet UIButton *synthesisButton;
    @property (strong, nonatomic) IBOutlet UILabel *resultLabel;
    
    @property (nonatomic, strong) AVAudioPlayer *player;
    
    - (IBAction)synthesisButtonTapped:(UIButton *)sender;
    
    @end
    
    @implementation ViewController
    
    - (void)viewDidLoad {
        [super viewDidLoad];
        
        inputText = @"";
        speechHost = @"https://YourServiceRegion.tts.speech.azure.cn/";
        speechKey = @"YourSubscriptionKey";
        
        [self.view setBackgroundColor:[UIColor whiteColor]];
        
        self.inputField = [[UITextField alloc] initWithFrame:CGRectMake(50, 150, 300, 50)];
        self.inputField.placeholder = @"Input something to synthesize...";
        self.inputField.borderStyle = UITextBorderStyleRoundedRect;
        self.inputField.delegate = self;
        self.inputField.accessibilityIdentifier = @"input_text_field";
        [self.view addSubview:self.inputField];
        
        self.synthesisButton = [UIButton buttonWithType:UIButtonTypeSystem];
        [self.synthesisButton addTarget:self action:@selector(synthesisButtonTapped:) forControlEvents:UIControlEventTouchUpInside];
        [self.synthesisButton setTitle:@"Synthesis" forState:UIControlStateNormal];
        [self.synthesisButton setFrame:CGRectMake(80, 350, 200, 50)];
        self.synthesisButton.titleLabel.font = [UIFont systemFontOfSize:36.0];
        self.synthesisButton.accessibilityIdentifier = @"synthesis_button";
        [self.view addSubview:self.synthesisButton];
        
        self.resultLabel = [[UILabel alloc] initWithFrame:CGRectMake(50.0, 400.0, 300.0, 200.0)];
        self.resultLabel.lineBreakMode = NSLineBreakByWordWrapping;
        self.resultLabel.numberOfLines = 0;
        self.resultLabel.accessibilityIdentifier = @"result_label";
        [self.view addSubview:self.resultLabel];
    }
    
    - (IBAction)synthesisButtonTapped:(UIButton *)sender {
        dispatch_async(dispatch_get_global_queue(QOS_CLASS_DEFAULT, 0), ^{
            [self synthesis];
        });
    }
    
    - (void)synthesis {
        SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithHost:speechHost subscription:speechKey];
        [speechConfig setSpeechSynthesisOutputFormat:SPXSpeechSynthesisOutputFormat_Audio16Khz32KBitRateMonoMp3];
        SPXSpeechSynthesizer *speechSynthesizer = [[SPXSpeechSynthesizer alloc] initWithSpeechConfiguration:speechConfig audioConfiguration:nil];
        
        NSLog(@"Start synthesizing...");
        [self updateText:@"Start synthesizing..." color:UIColor.grayColor];
        
        SPXSpeechSynthesisResult *speechResult = [speechSynthesizer speakText:inputText];
        
        // Checks result.
        if (SPXResultReason_Canceled == speechResult.reason) {
            SPXSpeechSynthesisCancellationDetails *details = [[SPXSpeechSynthesisCancellationDetails alloc] initFromCanceledSynthesisResult:speechResult];
            NSLog(@"Speech synthesis was canceled: %@. Did you pass the correct key/region combination?", details.errorDetails);
            [self updateText:[NSString stringWithFormat:@"Speech synthesis was canceled: %@. Did you pass the correct key/region combination?", details.errorDetails] color:UIColor.redColor];
        } else if (SPXResultReason_SynthesizingAudioCompleted == speechResult.reason) {
            NSLog(@"Speech synthesis was completed");
            // Play audio.
            self.player = [[AVAudioPlayer alloc] initWithData:[speechResult audioData] error:nil];
            [self.player prepareToPlay];
            [self.player play];
            [self updateText:@"The synthesis is completed." color:UIColor.blueColor];
        } else {
            NSLog(@"There was an error.");
            [self updateText:@"Synthesis error" color:UIColor.redColor];
        }
    }
    
    - (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string {;
        inputText = [textField.text stringByReplacingCharactersInRange:range withString:string];
        return true;
    }
    
    - (void)updateText:(NSString *) resultText color:(UIColor *)color{
        dispatch_async(dispatch_get_main_queue(), ^{
            self.resultLabel.textColor = color;
            self.resultLabel.text = resultText;
        });
    }
     
    @end
    ```

1. 将字符串 `YourSubscriptionKey` 替换为你的订阅密钥。
1. 将字符串 `YourServiceRegion` 替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)（例如，对于试用订阅，为 `chinaeast2`）。

## <a name="build-and-run-the-sample"></a>生成并运行示例

1. 使调试输出可见（“视图” > “调试区域” > “激活控制台”）。
1. 从“产品” > “目标”菜单中的列表中，选择 iOS 模拟器或连接到开发计算机的 iOS 设备作为应用的目标位置 。
1. 在 iOS 模拟器中生成并运行示例代码，方法是在菜单中选择“产品” > “运行”，或者单击“播放”按钮。

   ![模拟的 iOS 应用](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-simulated-app-tts.png)

1. 在输入一些文本并单击应用中的按钮后，你应该会听到播放的合成音频。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [浏览 GitHub 上的 Objective-C 示例](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

