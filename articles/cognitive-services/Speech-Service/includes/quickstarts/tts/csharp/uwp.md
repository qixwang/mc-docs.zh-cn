---
title: 快速入门：合成语音，C# (UWP) - 语音服务
titleSuffix: Azure Cognitive Services
description: 在本文中，请使用认知服务语音 SDK 创建 C# 通用 Windows 平台 (UWP) 应用程序。 将文本中的语音实时合成到设备的扬声器中。 该应用程序是使用语音 SDK NuGet 包和 Microsoft Visual Studio 2019 构建的。
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
origin.date: 04/04/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 40f8d8e44fbecce74e876c85351d8c13fb74d105
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82150736"
---
> [!NOTE]
> 通用 Windows 平台允许开发在支持 Windows 10 的任何设备上运行的应用，包括电脑、Xbox、Surface Hub 和其他设备。

## <a name="prerequisites"></a>先决条件

在开始之前，请务必：

> [!div class="checklist"]
> * [创建一个 Azure 搜索资源](../../../../get-started.md)
> * [设置开发环境并创建空项目](../../../../quickstarts/setup-platform.md?tabs=uwp&pivots=programming-language-csharp)
> * 需要语音 SDK 版本 1.10.0 或更高版本。

## <a name="add-sample-code"></a>添加示例代码

现在，添加定义应用程序用户界面的 XAML 代码，并添加 C# 代码隐藏实现。

1. 在“解决方案资源管理器”  中打开 `MainPage.xaml`。

1. 在设计器的 XAML 视图中，将以下 XAML 代码片段插入到“Grid”  标记中（位于 `<Grid>` 和 `</Grid>` 之间）：

    ```xml
    <StackPanel Orientation="Vertical" HorizontalAlignment="Center"  Margin="20,50,0,0" VerticalAlignment="Center" Width="800">
        <TextBox x:Name="TextForSynthesis" Text="Hi there!" Margin="0,10,10,0" />
        <Button x:Name="Speak" Content="Speak" Margin="0,10,10,0" Click="Speak_ButtonClicked" Height="35"/>
        <StackPanel x:Name="StatusPanel" Orientation="Vertical" RelativePanel.AlignBottomWithPanel="True" RelativePanel.AlignRightWithPanel="True" RelativePanel.AlignLeftWithPanel="True">
            <TextBlock x:Name="StatusLabel" Margin="0,10,10,0" TextWrapping="Wrap" Text="Status:" FontSize="20"/>
            <Border x:Name="StatusBorder" Margin="0,0,0,0">
                <ScrollViewer VerticalScrollMode="Auto"  VerticalScrollBarVisibility="Auto" MaxHeight="200">
                    <!-- Use LiveSetting to enable screen readers to announce the status update. -->
                    <TextBlock x:Name="StatusBlock" FontWeight="Bold" AutomationProperties.LiveSetting="Assertive"
                    MaxWidth="{Binding ElementName=Splitter, Path=ActualWidth}" Margin="10,10,10,20" TextWrapping="Wrap"  />
                </ScrollViewer>
            </Border>
        </StackPanel>
    </StackPanel>
    ```

1. 在“解决方案资源管理器”  中，打开代码隐藏源文件 `MainPage.xaml.cs`。 （其分组在 `MainPage.xaml` 下。）

1. 将其中的所有代码替换为以下片段：

    ```csharp
    using System;
    using System.IO;
    using System.Text;
    using Windows.Media.Core;
    using Windows.Media.Playback;
    using Windows.Storage;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Media;
    using Microsoft.CognitiveServices.Speech;
    
    namespace helloworld
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private MediaPlayer mediaPlayer;
    
            public MainPage()
            {
                this.InitializeComponent();
                this.mediaPlayer = new MediaPlayer();
            }
    
            private async void Speak_ButtonClicked(object sender, RoutedEventArgs e)
            {
                // Creates an instance of a speech config with specified subscription key and service region.
                // Replace with your own subscription key and service region (e.g., "chinaeast2").
                var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

                try
                {
                    // Creates a speech synthesizer.
                    using (var synthesizer = new SpeechSynthesizer(config, null))
                    {
                        // Receive a text from TextForSynthesis text box and synthesize it to speaker.
                        using (var result = await synthesizer.SpeakTextAsync(this.TextForSynthesis.Text).ConfigureAwait(false))
                        {
                            // Checks result.
                            if (result.Reason == ResultReason.SynthesizingAudioCompleted)
                            {
                                NotifyUser($"Speech Synthesis Succeeded.", NotifyType.StatusMessage);
    
                                // Since native playback is not yet supported on UWP (currently only supported on Windows/Linux Desktop),
                                // use the WinRT API to play audio here as a short term solution.
                                using (var audioStream = AudioDataStream.FromResult(result))
                                {
                                    // Save synthesized audio data as a wave file and use MediaPlayer to play it
                                    var filePath = Path.Combine(ApplicationData.Current.LocalFolder.Path, "outputaudio.wav");
                                    await audioStream.SaveToWaveFileAsync(filePath);
                                    mediaPlayer.Source = MediaSource.CreateFromStorageFile(await StorageFile.GetFileFromPathAsync(filePath));
                                    mediaPlayer.Play();
                                }
                            }
                            else if (result.Reason == ResultReason.Canceled)
                            {
                                var cancellation = SpeechSynthesisCancellationDetails.FromResult(result);
    
                                StringBuilder sb = new StringBuilder();
                                sb.AppendLine($"CANCELED: Reason={cancellation.Reason}");
                                sb.AppendLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                                sb.AppendLine($"CANCELED: ErrorDetails=[{cancellation.ErrorDetails}]");
    
                                NotifyUser(sb.ToString(), NotifyType.ErrorMessage);
                            }
                        }
                    }
                }
                catch (Exception ex)
                {
                    NotifyUser($"{ex.ToString()}", NotifyType.ErrorMessage);
                }
            }
    
            private enum NotifyType
            {
                StatusMessage,
                ErrorMessage
            };
    
            private void NotifyUser(string strMessage, NotifyType type)
            {
                // If called from the UI thread, then update immediately.
                // Otherwise, schedule a task on the UI thread to perform the update.
                if (Dispatcher.HasThreadAccess)
                {
                    UpdateStatus(strMessage, type);
                }
                else
                {
                    var task = Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => UpdateStatus(strMessage, type));
                }
            }
    
            private void UpdateStatus(string strMessage, NotifyType type)
            {
                switch (type)
                {
                    case NotifyType.StatusMessage:
                        StatusBorder.Background = new SolidColorBrush(Windows.UI.Colors.Green);
                        break;
                    case NotifyType.ErrorMessage:
                        StatusBorder.Background = new SolidColorBrush(Windows.UI.Colors.Red);
                        break;
                }
                StatusBlock.Text += string.IsNullOrEmpty(StatusBlock.Text) ? strMessage : "\n" + strMessage;
    
                // Collapse the StatusBlock if it has no text to conserve real estate.
                StatusBorder.Visibility = !string.IsNullOrEmpty(StatusBlock.Text) ? Visibility.Visible : Visibility.Collapsed;
                if (!string.IsNullOrEmpty(StatusBlock.Text))
                {
                    StatusBorder.Visibility = Visibility.Visible;
                    StatusPanel.Visibility = Visibility.Visible;
                }
                else
                {
                    StatusBorder.Visibility = Visibility.Collapsed;
                    StatusPanel.Visibility = Visibility.Collapsed;
                }
                // Raise an event if necessary to enable a screen reader to announce the status update.
                var peer = Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer.FromElement(StatusBlock);
                if (peer != null)
                {
                    peer.RaiseAutomationEvent(Windows.UI.Xaml.Automation.Peers.AutomationEvents.LiveRegionChanged);
                }
            }
        }
    }
    ```

1. 在源文件的 `Speak_ButtonClicked` 处理程序中，找到字符串 `YourSubscriptionKey` 并将其替换为订阅密钥。

1. 在 `Speak_ButtonClicked` 处理程序中，找到字符串 `YourServiceRegion` 并将其替换为与订阅关联的[区域](~/articles/cognitive-services/Speech-Service/regions.md)。 （例如，将 `chinaeast2` 用于试用订阅。）

1. 在菜单栏中，选择“文件”   > “全部保存”  以保存所做的更改。

## <a name="build-and-run-the-application"></a>生成并运行应用程序

现已准备好构建并测试应用程序。

1. 从菜单栏中，选择“构建”   > “构建解决方案”  以构建应用程序。 现在，编译代码时应不会提示错误。

1. 选择“调试”   > “开始调试”  （或按 F5  ）以启动应用程序。 此时将显示“helloworld”  窗口。

   ![C# 中的示例 UWP 语音合成应用程序 - 快速入门](~/articles/cognitive-services/Speech-Service/media/sdk/qs-text-to-speech-uwp-helloworld-window.png)

1. 在文本框中输入一些文本，然后单击“朗读”  。 你的文本将传输到语音服务，并合成为语音，以在扬声器上播放。

    ![语音合成用户界面](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>后续步骤

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

<!-- ## See also -->

<!-- - [Create a Custom Voice](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md) -->
<!-- - [Record custom voice samples](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md) -->
