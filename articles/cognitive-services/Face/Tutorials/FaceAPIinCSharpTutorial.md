---
title: 教程：通过 .NET SDK 检测和显示图像中的人脸数据
titleSuffix: Azure Cognitive Services
description: 本教程将创建一个 Windows 应用，以便使用人脸 API 来检测和定格图像中的人脸。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
origin.date: 07/03/2019
ms.date: 07/10/2019
ms.author: v-junlch
ms.openlocfilehash: 1ddc07de1e341a0091872980e92cf8fc1897d5c0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67844697"
---
# <a name="tutorial-create-a-wpf-app-to-display-face-data-in-an-image"></a>教程：创建一个用于显示图像中人脸数据的 WPF 应用

本教程介绍如何通过 .NET 客户端 SDK 使用 Azure 人脸 API 检测图像中的人脸，然后在 UI 中显示该数据。 你将创建一个 Windows Presentation Framework (WPF) 应用程序，用于检测人脸，围绕每张脸绘制一个框架，并在状态栏中显示人脸描述。 

本教程演示如何：

> [!div class="checklist"]
> - 创建 WPF 应用程序
> - 安装人脸 API 客户端库
> - 使用客户端库检测图像中的人脸
> - 围绕每个检测到的人脸绘制一个框架
> - 在状态栏上显示被框出的人脸的描述

![显示使用矩形将检测到的人脸定格的屏幕截图](../Images/getting-started-cs-detected.png)

完整的示例代码在 GitHub 上的[认知人脸 CSharp 示例](https://github.com/Azure-Samples/Cognitive-Face-CSharp-sample)存储库中提供。

如果没有 Azure 订阅，请在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。 


## <a name="prerequisites"></a>必备条件

- 人脸 API 订阅密钥。 可以按照[创建认知服务帐户](/cognitive-services/cognitive-services-apis-create-account)中的说明订阅人脸 API 服务并获取密钥。
- 任何版本的 [Visual Studio 2015 或 2017](https://www.visualstudio.com/downloads/)。

## <a name="create-the-visual-studio-project"></a>创建 Visual Studio 项目

执行以下步骤，以便创建新的 WPF 应用程序项目。

1. 在 Visual Studio 中打开“新建项目”对话框。 展开“已安装”，接着展开“Visual C#”，然后选择“WPF 应用(.NET Framework)”。   
1. 将应用程序命名为“FaceTutorial”，然后单击“确定”   。
1. 获取所需的 NuGet 包。 右键单击解决方案资源管理器中的项目，选择“管理 NuGet 包”，然后找到并安装以下包： 
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)

## <a name="add-the-initial-code"></a>添加初始代码

在此部分，需添加应用的基本框架，没有特定于人脸的功能。

### <a name="create-the-ui"></a>创建 UI

打开 *MainWindow.xaml*，将其中的内容替换为以下代码&mdash;此代码将创建 UI 窗口。 请注意，`FacePhoto_MouseMove` 和 `BrowseButton_Click` 方法是将在稍后定义的事件处理程序。

```csharp
<Window x:Class="FaceTutorial.MainWindow"
         xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
         xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
         Title="MainWindow" Height="700" Width="960">
    <Grid x:Name="BackPanel">
        <Image x:Name="FacePhoto" Stretch="Uniform" Margin="0,0,0,50" MouseMove="FacePhoto_MouseMove" />
        <DockPanel DockPanel.Dock="Bottom">
            <Button x:Name="BrowseButton" Width="72" Height="20" VerticalAlignment="Bottom" HorizontalAlignment="Left"
                     Content="Browse..."
                     Click="BrowseButton_Click" />
            <StatusBar VerticalAlignment="Bottom">
                <StatusBarItem>
                    <TextBlock Name="faceDescriptionStatusBar" />
                </StatusBarItem>
            </StatusBar>
        </DockPanel>
    </Grid>
</Window>
```

### <a name="create-the-main-class"></a>创建主类

打开 *MainWindow.xaml.cs*，添加客户端库命名空间和其他必需的命名空间。 

```csharp
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
```

接下来，在 **MainWindow** 类中插入以下代码。 此代码将使用订阅密钥创建 **FaceClient** 实例，该密钥必须由你自己输入。 你需要在 `faceEndpoint` 中将区域字符串设置为适用于你的订阅的正确区域（有关所有区域终结点的列表，请参阅[人脸 API 文档](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)）。

```csharp
// Replace <SubscriptionKey> with your valid subscription key.
// For example, subscriptionKey = "0123456789abcdef0123456789ABCDEF"
private const string subscriptionKey = "<SubscriptionKey>";

private const string faceEndpoint = "https://api.cognitive.azure.cn";

private readonly IFaceClient faceClient = new FaceClient(
    new ApiKeyServiceClientCredentials(subscriptionKey),
    new System.Net.Http.DelegatingHandler[] { });

// The list of detected faces.
private IList<DetectedFace> faceList;
// The list of descriptions for the detected faces.
private string[] faceDescriptions;
// The resize factor for the displayed image.
private double resizeFactor;

private const string defaultStatusBarText =
    "Place the mouse pointer over a face to see the face description.";
```

接下来，将以下代码粘贴到 **MainWindow** 方法中。

```csharp
InitializeComponent();

if (Uri.IsWellFormedUriString(faceEndpoint, UriKind.Absolute))
{
    faceClient.Endpoint = faceEndpoint;
}
else
{
    MessageBox.Show(faceEndpoint,
        "Invalid URI", MessageBoxButton.OK, MessageBoxImage.Error);
    Environment.Exit(0);
}
```

最后，向类添加 **BrowseButton_Click** 和 **FacePhoto_MouseMove** 方法。 这两个方法对应于在 *MainWindow.xaml* 中声明的事件处理程序。 **BrowseButton_Click** 方法创建 **OpenFileDialog**，供用户选择 .jpg 图像。 然后，它会在主窗口中显示图像。 将会在后面的步骤中插入 **BrowseButton_Click** 和 **FacePhoto_MouseMove** 的剩余代码。 另请注意 `faceList` 引用&mdash;**DetectedFace** 对象的列表。 此引用是应用存储和调用实际人脸数据的位置。

```csharp
// Displays the image and calls UploadAndDetectFaces.
private async void BrowseButton_Click(object sender, RoutedEventArgs e)
{
    // Get the image file to scan from the user.
    var openDlg = new Microsoft.Win32.OpenFileDialog();

    openDlg.Filter = "JPEG Image(*.jpg)|*.jpg";
    bool? result = openDlg.ShowDialog(this);

    // Return if canceled.
    if (!(bool)result)
    {
        return;
    }

    // Display the image file.
    string filePath = openDlg.FileName;

    Uri fileUri = new Uri(filePath);
    BitmapImage bitmapSource = new BitmapImage();

    bitmapSource.BeginInit();
    bitmapSource.CacheOption = BitmapCacheOption.None;
    bitmapSource.UriSource = fileUri;
    bitmapSource.EndInit();

    FacePhoto.Source = bitmapSource;
}
```

```csharp
// Displays the face description when the mouse is over a face rectangle.
private void FacePhoto_MouseMove(object sender, MouseEventArgs e)
{
}
```

### <a name="try-the-app"></a>试用应用

按菜单上的“启动”，对应用进行测试。  应用窗口打开后，单击左下角的“浏览”。  此时会打开“打开的文件”对话框。  从文件系统中选择一个图像，验证它是否显示在窗口中。 然后，关闭应用并转到下一步。

![显示未修改的人脸图像的屏幕截图](../Images/getting-started-cs-ui.png)

## <a name="upload-image-and-detect-faces"></a>上传图像并检测人脸

应用会通过调用 **FaceClient.Face.DetectWithStreamAsync** 方法来检测人脸，该方法可包装用于上传本地图像的[检测](https://dev.cognitive.azure.cn/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) REST API。

在 **MainWindow** 类中 **FacePhoto_MouseMove** 方法下面插入以下方法。 此方法会定义一系列人脸特性，用于检索提交的图像文件并将其读取到**流**中。 然后，它将这两个对象传递到 **DetectWithStreamAsync** 方法调用。

```csharp
// Uploads the image file and calls DetectWithStreamAsync.
private async Task<IList<DetectedFace>> UploadAndDetectFaces(string imageFilePath)
{
    // The list of Face attributes to return.
    IList<FaceAttributeType> faceAttributes =
        new FaceAttributeType[]
        {
            FaceAttributeType.Gender, FaceAttributeType.Age,
            FaceAttributeType.Smile, FaceAttributeType.Emotion,
            FaceAttributeType.Glasses, FaceAttributeType.Hair
        };

    // Call the Face API.
    try
    {
        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            // The second argument specifies to return the faceId, while
            // the third argument specifies not to return face landmarks.
            IList<DetectedFace> faceList =
                await faceClient.Face.DetectWithStreamAsync(
                    imageFileStream, true, false, faceAttributes);
            return faceList;
        }
    }
    // Catch and display Face API errors.
    catch (APIErrorException f)
    {
        MessageBox.Show(f.Message);
        return new List<DetectedFace>();
    }
    // Catch and display all other errors.
    catch (Exception e)
    {
        MessageBox.Show(e.Message, "Error");
        return new List<DetectedFace>();
    }
}
```

## <a name="draw-rectangles-around-faces"></a>在人脸周围绘制矩形

接下来，将添加用于在图像中每个检测到的人脸周围绘制矩形的代码。 在 **MainWindow** 类的 **BrowseButton_Click** 方法末尾的 `FacePhoto.Source = bitmapSource` 行后插入以下代码。 此代码会通过调用 **UploadAndDetectFaces** 填充检测到的人脸的列表。 然后，它会围绕每个人脸绘制一个矩形，并将修改的图像显示在主窗口中。

```csharp
// Detect any faces in the image.
Title = "Detecting...";
faceList = await UploadAndDetectFaces(filePath);
Title = String.Format(
    "Detection Finished. {0} face(s) detected", faceList.Count);

if (faceList.Count > 0)
{
    // Prepare to draw rectangles around the faces.
    DrawingVisual visual = new DrawingVisual();
    DrawingContext drawingContext = visual.RenderOpen();
    drawingContext.DrawImage(bitmapSource,
        new Rect(0, 0, bitmapSource.Width, bitmapSource.Height));
    double dpi = bitmapSource.DpiX;
    // Some images don't contain dpi info.
    resizeFactor = (dpi == 0) ? 1 : 96 / dpi;
    faceDescriptions = new String[faceList.Count];

    for (int i = 0; i < faceList.Count; ++i)
    {
        DetectedFace face = faceList[i];

        // Draw a rectangle on the face.
        drawingContext.DrawRectangle(
            Brushes.Transparent,
            new Pen(Brushes.Red, 2),
            new Rect(
                face.FaceRectangle.Left * resizeFactor,
                face.FaceRectangle.Top * resizeFactor,
                face.FaceRectangle.Width * resizeFactor,
                face.FaceRectangle.Height * resizeFactor
                )
        );

        // Store the face description.
        faceDescriptions[i] = FaceDescription(face);
    }

    drawingContext.Close();

    // Display the image with the rectangle around the face.
    RenderTargetBitmap faceWithRectBitmap = new RenderTargetBitmap(
        (int)(bitmapSource.PixelWidth * resizeFactor),
        (int)(bitmapSource.PixelHeight * resizeFactor),
        96,
        96,
        PixelFormats.Pbgra32);

    faceWithRectBitmap.Render(visual);
    FacePhoto.Source = faceWithRectBitmap;

    // Set the status bar text.
    faceDescriptionStatusBar.Text = defaultStatusBarText;
}
```

## <a name="describe-the-faces"></a>描述人脸

将以下方法添加到 **MainWindow** 类的 **UploadAndDetectFaces** 方法下面。 此方法会将检索到的人脸特性转换成描述人脸的字符串。

```csharp
// Creates a string out of the attributes describing the face.
private string FaceDescription(DetectedFace face)
{
    StringBuilder sb = new StringBuilder();

    sb.Append("Face: ");

    // Add the gender, age, and smile.
    sb.Append(face.FaceAttributes.Gender);
    sb.Append(", ");
    sb.Append(face.FaceAttributes.Age);
    sb.Append(", ");
    sb.Append(String.Format("smile {0:F1}%, ", face.FaceAttributes.Smile * 100));

    // Add the emotions. Display all emotions over 10%.
    sb.Append("Emotion: ");
    Emotion emotionScores = face.FaceAttributes.Emotion;
    if (emotionScores.Anger >= 0.1f) sb.Append(
        String.Format("anger {0:F1}%, ", emotionScores.Anger * 100));
    if (emotionScores.Contempt >= 0.1f) sb.Append(
        String.Format("contempt {0:F1}%, ", emotionScores.Contempt * 100));
    if (emotionScores.Disgust >= 0.1f) sb.Append(
        String.Format("disgust {0:F1}%, ", emotionScores.Disgust * 100));
    if (emotionScores.Fear >= 0.1f) sb.Append(
        String.Format("fear {0:F1}%, ", emotionScores.Fear * 100));
    if (emotionScores.Happiness >= 0.1f) sb.Append(
        String.Format("happiness {0:F1}%, ", emotionScores.Happiness * 100));
    if (emotionScores.Neutral >= 0.1f) sb.Append(
        String.Format("neutral {0:F1}%, ", emotionScores.Neutral * 100));
    if (emotionScores.Sadness >= 0.1f) sb.Append(
        String.Format("sadness {0:F1}%, ", emotionScores.Sadness * 100));
    if (emotionScores.Surprise >= 0.1f) sb.Append(
        String.Format("surprise {0:F1}%, ", emotionScores.Surprise * 100));

    // Add glasses.
    sb.Append(face.FaceAttributes.Glasses);
    sb.Append(", ");

    // Add hair.
    sb.Append("Hair: ");

    // Display baldness confidence if over 1%.
    if (face.FaceAttributes.Hair.Bald >= 0.01f)
        sb.Append(String.Format("bald {0:F1}% ", face.FaceAttributes.Hair.Bald * 100));

    // Display all hair color attributes over 10%.
    IList<HairColor> hairColors = face.FaceAttributes.Hair.HairColor;
    foreach (HairColor hairColor in hairColors)
    {
        if (hairColor.Confidence >= 0.1f)
        {
            sb.Append(hairColor.Color.ToString());
            sb.Append(String.Format(" {0:F1}% ", hairColor.Confidence * 100));
        }
    }

    // Return the built string.
    return sb.ToString();
}
```

## <a name="display-the-face-description"></a>显示人脸描述

将以下代码添加到 **FacePhoto_MouseMove** 方法。 当光标悬停在检测的人脸矩形上时，此事件处理程序会显示 `faceDescriptionStatusBar` 中的人脸描述字符串。

```csharp
// If the REST call has not completed, return.
if (faceList == null)
    return;

// Find the mouse position relative to the image.
Point mouseXY = e.GetPosition(FacePhoto);

ImageSource imageSource = FacePhoto.Source;
BitmapSource bitmapSource = (BitmapSource)imageSource;

// Scale adjustment between the actual size and displayed size.
var scale = FacePhoto.ActualWidth / (bitmapSource.PixelWidth / resizeFactor);

// Check if this mouse position is over a face rectangle.
bool mouseOverFace = false;

for (int i = 0; i < faceList.Count; ++i)
{
    FaceRectangle fr = faceList[i].FaceRectangle;
    double left = fr.Left * scale;
    double top = fr.Top * scale;
    double width = fr.Width * scale;
    double height = fr.Height * scale;

    // Display the face description if the mouse is over this face rectangle.
    if (mouseXY.X >= left && mouseXY.X <= left + width &&
        mouseXY.Y >= top && mouseXY.Y <= top + height)
    {
        faceDescriptionStatusBar.Text = faceDescriptions[i];
        mouseOverFace = true;
        break;
    }
}

// String to display when the mouse is not over a face rectangle.
if (!mouseOverFace) faceDescriptionStatusBar.Text = defaultStatusBarText;
```


## <a name="run-the-app"></a>运行应用

运行此应用程序并通过浏览方式查找包含人脸的图像。 等待几秒钟，以便人脸服务响应。 此时会在图像中的每个人脸上看到一个红色矩形。 如果将鼠标移到人脸矩形上，状态栏中会显示该人脸的描述。

![显示使用矩形将检测到的人脸定格的屏幕截图](../Images/getting-started-cs-detected.png)


## <a name="next-steps"></a>后续步骤

本教程介绍了人脸服务 .NET SDK 的基本使用过程，并创建了一个应用程序来检测并定格图像中的人脸。 接下来，请深入了解人脸检测的详细信息。

> [!div class="nextstepaction"]
> [如何检测图像中的人脸](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)

<!-- Update_Description: wording update -->
