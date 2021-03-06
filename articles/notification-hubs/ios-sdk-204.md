---
title: 使用 Azure 通知中心和 iOS SDK 版本 2.0.4 向 iOS 发送推送通知
description: 本教程介绍如何使用 Azure 通知中心和 Apple 推送通知服务向 iOS 设备发送推送通知。
author: sethmanheim
ms.author: v-tawe
origin.date: 06/19/2020
ms.date: 07/21/2020
ms.topic: tutorial
ms.service: notification-hubs
ms.reviewer: thsomasu
ms.lastreviewed: 06/01/2020
ms.openlocfilehash: 2da3651922c47487288f4fd04276b8f83a3e891a
ms.sourcegitcommit: 5656c18d7d2faa09329b1a15e352d1622e252d5f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2020
ms.locfileid: "86863055"
---
# <a name="tutorial-send-push-notifications-to-ios-apps-using-azure-notification-hubs-version-204"></a>教程：使用 Azure 通知中心（版本 2.0.4）向 iOS 应用发送推送通知

本教程演示如何使用 Azure 通知中心 SDK 版本 2.0.4 和 Azure 通知中心向 iOS 应用程序发送推送通知。

本教程涵盖以下步骤：

- 创建示例 iOS 应用。
- 将 iOS 应用连接到 Azure 通知中心。
- 发送测试推送通知。
- 验证应用是否可以接收通知。

可以从 [GitHub](https://github.com/Azure/azure-notificationhubs-ios/tree/v3-preview2/Samples) 下载本教程的完整代码。

## <a name="prerequisites"></a>先决条件

要完成本教程，需要具备以下先决条件：

- 运行 [Xcode](https://go.microsoft.com/fwLink/p/?LinkID=266532) 的 Mac，以及安装到密钥链中的有效开发人员证书。
- 运行 iOS 版本 10 或更高版本的 iPhone 或 iPad。
- 在 [Apple 门户](https://developer.apple.com/)中注册并与证书关联的物理设备。

在继续操作之前，请务必完成上一篇教程，了解如何开始使用[适用于 iOS 应用的 Azure 通知中心](ios-sdk-get-started.md)，并在通知中心设置和配置推送凭据。 即使你没有 iOS 开发经验，也可以按照这些步骤操作。

> [!NOTE]
> 由于推送通知的配置要求，必须在物理 iOS 设备（iPhone 或 iPad）而不是在 iOS 仿真器上部署和测试推送通知。

## <a name="connect-your-ios-app-to-notification-hubs"></a>将 iOS 应用连接到通知中心

1. 在 Xcode 中，创建新的 iOS 项目，并选择“单视图应用程序”模板。 ****  

   :::image type="content" source="media/ios-sdk/image1.png" alt-text="选择模板":::

2. 设置新项目的选项时，请务必使用在 Apple 开发人员门户中设置捆绑标识符时使用的同一产品名称和组织标识符。 ****   ****  

3. 在“项目导航器”的“目标”下选择项目名称，然后选择“签名和功能”选项卡。 ****  ****   确保为 Apple 开发人员帐户选择适当的“团队”。 ****   XCode 会根据捆绑标识符自动下拉以前创建的预配配置文件。

   如果屏幕未显示在 Xcode 中创建的新预配配置文件，请尝试刷新签名标识的配置文件。 单击菜单栏上的“Xcode”，然后依次单击“首选项”、“帐户”选项卡、“查看详细信息”按钮、签名标识和右下角的刷新按钮。 ****   ****  ****   ****  

   :::image type="content" source="media/ios-sdk/image2.png" alt-text="查看详细信息":::

4. 在“签名和功能”选项卡中，选择“+ 功能”。 ****   **** 双击“推送通知”以启用它。 ****  

   :::image type="content" source="media/ios-sdk/image3.png" alt-text="功能":::

5. 添加 Azure 通知中心 SDK 模块。

   可以使用 [Cocoapods](https://cocoapods.org/) 或手动将二进制文件添加到项目中，从而将 Azure 通知中心 SDK 集成到应用中。

   - 通过 Cocoapods 集成：将以下依赖项添加到 podfile，以便将 Azure 通知中心 SDK 包含到应用中：

      ```ruby
      pod 'AzureNotificationHubs-iOS'
      ```

      - 运行 pod 安装，安装新定义的 pod 并打开 .xcworkspace。

         如果在运行 pod 安装时看到错误（例如找不到 AzureNotificationHubs-iOS 的规范），请运行 `pod repo update` 以从 Cocoapods 存储库获取最新的 pod，然后运行 pod 安装。

   - 通过 Carthage 集成：将以下依赖项添加到 Cartfile，以便将 Azure 通知中心 SDK 包含到应用中：

      ```ruby
      github "Azure/azure-notificationhubs-ios"
      ```

      - 接下来，更新生成依赖项：

      ```shell
      $ carthage update
      ```

      有关使用 Carthage 的详细信息，请参阅 [Carthage GitHub 存储库](https://github.com/Carthage/Carthage)。

   - 通过将二进制文件复制到项目中进行集成：可以通过将二进制文件复制到项目中来进行集成，如下所示：

        - 下载以 zip 文件形式提供的 [Azure 通知中心 SDK](https://github.com/Azure/azure-notificationhubs-android/releases) 框架并将其解压缩。

        - 在 Xcode 中，右键单击项目，然后单击“将文件添加到”选项，将WindowsAzureMessaging.framework 文件夹添加到 Xcode 项目。 ****   ****   选择“选项”，确保选中“根据需要复制项目”，然后单击“添加”。 ****   ****   ****

          :::image type="content" source="media/ios-sdk/image4.png" alt-text="添加框架":::

6. 将新的头文件添加到名为 Constants.h 的项目。 **** 为此，请右键单击项目名称并选择“新建文件…”。 **** 然后选择“头文件”。 **** 此文件保存着通知中心的常量。 然后选择“下一步” **** 。 将文件命名为 Constants.h **** 。

7. 将以下代码添加到 Constants.h 文件中：

   ```objc
   #ifndef Constants_h
   #define Constants_h
   extern NSString* const NHInfoConnectionString;
   extern NSString* const NHInfoHubName;
   extern NSString* const NHUserDefaultTags;
   #endif /* Constants_h */
   ```

8. 添加 Constants.h 的实现文件。 为此，请右键单击项目名称并选择“新建文件…”。 **** 选择“Objective-C 文件”，然后选择“下一步”。 ****  **** 将文件命名为 Constants.m。 ****

   :::image type="content" source="media/ios-sdk/image5.png" alt-text="添加实现文件":::

9. 打开 Constants.m 文件并将其内容替换为以下代码。 ****   将字符串文本占位符 `NotificationHubConnectionString` 和 `NotificationHubConnectionString` 分别替换为之前从门户中获取的中心名称和 DefaultListenSharedAccessSignature： ****   

   ```objc
   #import <Foundation/Foundation.h>
   #import "Constants.h"

   NSString* const NHInfoConnectionString = @"NotificationHubConnectionString";
   NSString* const NHInfoHubName = @"NotificationHubName";NSString* const NHUserDefaultTags = @"notification_tags";
   ```

10. 打开项目的 AppDelegate.h 文件，并将其内容替换为以下代码： ****  

    ```objc
    #import <UIKit/UIKit.h>
    #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
    #import <UserNotifications/UserNotifications.h> 

    @interface AppDelegate : UIResponder <UIApplicationDelegate,   UNUserNotificationCenterDelegate>

    @property (strong, nonatomic) UIWindow *window;

    - (void)handleRegister;
    - (void)handleUnregister;

    @end
    ```

11. 在项目 AppDelegate.m 文件中，添加以下 `import` 语句： ****   

    ```objc
    #import "Constants.h"
    #import "NotificationDetailViewController.h"
    ```

12. 此外，在 AppDelegate.m 文件中，根据 iOS 版本在 `didFinishLaunchingWithOptions` 方法中添加以下代码行。 ****    此代码将向 APNs 注册设备句柄：

    ```objc
    [[UNUserNotificationCenter currentNotificationCenter] setDelegate:self];
    ```

13. 在同一 AppDelegate.m 文件中，将 `didFinishLaunchingWithOptions` 后面的所有代码替换为以下代码： ****   

    ```objc
    // Tells the app that a remote notification arrived that indicates there is data to be fetched.

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler {
    NSLog(@"Received remote (silent) notification");
    [self logNotificationDetails:userInfo];

    //
    // Let the system know the silent notification has been processed.
    //
    completionHandler(UIBackgroundFetchResultNoData);
    }

    // Tells the delegate that the app successfully registered with Apple Push Notification service (APNs).

    - (void) application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    NSMutableSet *tags = [[NSMutableSet alloc] init];

    // Load and parse stored tags
    NSString *unparsedTags = [[NSUserDefaults standardUserDefaults] valueForKey:NHUserDefaultTags];
    if (unparsedTags.length > 0) {
        NSArray *tagsArray = [unparsedTags componentsSeparatedByString: @","];
        [tags addObjectsFromArray:tagsArray];
    }

    // Register the device with the Notification Hub.
    // If the device has not already been registered, this will create the registration.
    // If the device has already been registered, this will update the existing registration.
    //
    SBNotificationHub* hub = [self getNotificationHub];
    [hub registerNativeWithDeviceToken:deviceToken tags:tags completion:^(NSError* error) {
        if (error != nil) {
            NSLog(@"Error registering for notifications: %@", error);
        } else {
            [self showAlert:@"Registered" withTitle:@"Registration Status"];
        }
    }];
    }

    // UNUserNotificationCenterDelegate methods
    //
    // Asks the delegate how to handle a notification that arrived while the app was running in the  foreground.

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler {
    NSLog(@"Received notification while the application is in the foreground");

    // The system calls this delegate method when the app is in the foreground. This allows the app to handle the notification
    // itself (and potentially modify the default system behavior).

    // Handle the notification by displaying custom UI.
    //
    [self showNotification:notification.request.content.userInfo];

    // Use 'options' to specify which default behaviors to enable.
    // - UNAuthorizationOptionBadge: Apply the notification's badge value to the app’s icon.
    // - UNAuthorizationOptionSound: Play the sound associated with the notification.
    // - UNAuthorizationOptionAlert: Display the alert using the content provided by the notification.
    //
    // In this case, do not pass UNAuthorizationOptionAlert because the notification was handled by the app.
    //
    completionHandler(UNAuthorizationOptionBadge | UNAuthorizationOptionSound);
    }

    // Asks the delegate to process the user's response to a delivered notification.
    //

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)(void))completionHandler {
    NSLog(@"Received notification while the application is in the background");

    // The system calls this delegate method when the user taps or responds to the system notification.

    // Handle the notification response by displaying custom UI
    //
    [self showNotification:response.notification.request.content.userInfo];

    // Let the system know the response has been processed.
    //
    completionHandler();
    }

    // App logic and helpers

    - (SBNotificationHub *)getNotificationHub {
    NSString *hubName = [[NSBundle mainBundle] objectForInfoDictionaryKey:NHInfoHubName];
    NSString *connectionString = [[NSBundle mainBundle] objectForInfoDictionaryKey:NHInfoConnectionString];

    return [[SBNotificationHub alloc] initWithConnectionString:connectionString notificationHubPath:hubName];
    }

    - (void)handleRegister {
    UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];

    UNAuthorizationOptions options =  UNAuthorizationOptionAlert | UNAuthorizationOptionSound | UNAuthorizationOptionBadge;
    [center requestAuthorizationWithOptions:(options) completionHandler:^(BOOL granted, NSError * _Nullable error) {
        if (error != nil) {
            NSLog(@"Error requesting for authorization: %@", error);
        }
    }];
    [[UIApplication sharedApplication] registerForRemoteNotifications];
    }

    - (void)handleUnregister {
    //
    // Unregister the device with the Notification Hub.
    //
    SBNotificationHub *hub = [self getNotificationHub];
    [hub unregisterNativeWithCompletion:^(NSError* error) {
        if (error != nil) {
            NSLog(@"Error unregistering for push: %@", error);
        } else {
            [self showAlert:@"Unregistered" withTitle:@"Registration Status"];
        }
    }];
    }

    - (void)logNotificationDetails:(NSDictionary *)userInfo {
    if (userInfo != nil) {
        UIApplicationState state = [UIApplication sharedApplication].applicationState;
        BOOL background = state != UIApplicationStateActive;
        NSLog(@"Received %@notification: \n%@", background ? @"(background) " : @"", userInfo);
    }
    }

    - (void)showAlert:(NSString *)message withTitle:(NSString *)title {
    if (title == nil) {
        title = @"Alert";
    }
    UIAlertController *alert = [UIAlertController alertControllerWithTitle:title message:message preferredStyle:UIAlertControllerStyleAlert];
    [alert addAction:[UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:nil]];
    [[[[UIApplication sharedApplication] keyWindow] rootViewController] presentViewController:alert animated:YES completion:nil];
    }

    - (void)showNotification:(NSDictionary *)userInfo {
    [self logNotificationDetails:userInfo];

    NotificationDetailViewController *notificationDetail = [[NotificationDetailViewController alloc] initWithUserInfo:userInfo];
    [[[[UIApplication sharedApplication] keyWindow] rootViewController] presentViewController:notificationDetail animated:YES completion:nil];
    }

    @end
    ```

    此代码使用在 Constants.h 中指定的连接信息连接到通知中心。 **** 然后，它向通知中心提供设备令牌，使中心能够发送通知。

### <a name="create-notificationdetailviewcontroller-header-file"></a>创建 NotificationDetailViewController 头文件

1. 与之前的说明类似，添加另一个名为 NotificationDetailViewController.h 的头文件。 **** 将新的头文件的内容替换为以下代码：

   ```objc
   #import <UIKit/UIKit.h>

   NS_ASSUME_NONNULL_BEGIN

   @interface NotificationDetailViewController : UIViewController

   @property (strong, nonatomic) IBOutlet UILabel *titleLabel;
   @property (strong, nonatomic) IBOutlet UILabel *bodyLabel;
   @property (strong, nonatomic) IBOutlet UIButton *dismissButton;

   @property (strong, nonatomic) NSDictionary *userInfo;

   - (id)initWithUserInfo:(NSDictionary *)userInfo;

   @end

   NS_ASSUME_NONNULL_END
   ```

2. 添加实现文件 NotificationDetailViewController.m。 **** 将该文件的内容替换为以下代码，此代码可实现 UIViewController 方法：

   ```objc
   #import "NotificationDetailViewController.h"

   @interface NotificationDetailViewController ()

   @end

   @implementation NotificationDetailViewController

   - (id)initWithUserInfo:(NSDictionary *)userInfo {
    self = [super initWithNibName:@"NotificationDetail" bundle:nil];
    if (self) {
        _userInfo = userInfo;
    }
    return self;
   }

   - (void)viewDidLayoutSubviews {
    [self.titleLabel sizeToFit];
    [self.bodyLabel sizeToFit];
   }

   - (void)viewDidLoad {
    [super viewDidLoad];

    NSString *title = nil;
    NSString *body = nil;

    NSDictionary *aps = [_userInfo valueForKey:@"aps"];
    NSObject *alertObject = [aps valueForKey:@"alert"];
    if (alertObject != nil) {
        if ([alertObject isKindOfClass:[NSDictionary class]]) {
            NSDictionary *alertDict = (NSDictionary *)alertObject;
            title = [alertDict valueForKey:@"title"];
            body = [alertObject valueForKey:@"body"];
        } else if ([alertObject isKindOfClass:[NSString class]]) {
            body = (NSString *)alertObject;
        } else {
            NSLog(@"Unable to parse notification content. Unexpected format: %@", alertObject);
        }
    }

    if (title == nil) {
        title = @"<unset>";
    }

    if (body == nil) {
        body = @"<unset>";
    }

    self.titleLabel.text = title;
    self.bodyLabel.text = body;
   }

   - (IBAction)handleDismiss:(id)sender {
    [self dismissViewControllerAnimated:YES completion:nil];
   }

   @end
   ```

### <a name="viewcontroller"></a>ViewController

1. 在项目 ViewController.h 文件中，添加以下 `import` 语句： ****   

   ```objc
   #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
   #import <UserNotifications/UserNotifications.h>
   ```

2. 此外，在 ViewController.h 中，在 `@interface` 声明后添加以下属性声明： ****  

   ```objc
   @property (strong, nonatomic) IBOutlet UITextField *tagsTextField;
   @property (strong, nonatomic) IBOutlet UIButton *registerButton;
   @property (strong, nonatomic) IBOutlet UIButton *unregisterButton;
   ```

3. 在项目的 ViewController.m 实现文件中，将该文件的内容替换为以下代码： ****  

   ```objc
   #import "ViewController.h"
   #import "Constants.h"
   #import "AppDelegate.h"

   @interface ViewController ()

   @end

   @implementation ViewController

   // UIViewController methods

   - (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    // Simple method to dismiss keyboard when user taps outside of the UITextField.
    [self.view endEditing:YES];
   }

   - (void)viewDidLoad {
    [super viewDidLoad];

    // Load raw tags text from storage and initialize the text field
    self.tagsTextField.text = [[NSUserDefaults standardUserDefaults] valueForKey:NHUserDefaultTags];
   }

   - (IBAction)handleRegister:(id)sender {
    // Save raw tags text in storage
    [[NSUserDefaults standardUserDefaults] setValue:self.tagsTextField.text forKey:NHUserDefaultTags];

   // Delegate processing the register action to the app delegate.
   [[[UIApplication sharedApplication] delegate] performSelector:@selector(handleRegister)];
   }

   - (IBAction)handleUnregister:(id)sender {
    [[[UIApplication sharedApplication] delegate] performSelector:@selector(handleUnregister)];
   }

   @end
   ```

4. 若要验证是否没有故障，请在设备上生成并运行应用。

## <a name="send-test-push-notifications"></a>发送测试推送通知

可以在 [Azure 门户](https://portal.azure.cn/)中使用“测试发送”选项，在应用中测试通知的接收。 ****   它会向设备发送测试性的推送通知。

:::image type="content" source="media/ios-sdk/image6.png" alt-text="测试发送":::

通常，推送通知是在后端服务（例如，移动应用，或者使用兼容库的 ASP.NET）中发送的。 如果后端没有可用库，也可使用 REST API 直接发送通知消息。

下面是可能需要查看的有关发送通知的其他教程列表：

- Azure 移动应用：有关如何从与通知中心集成的移动应用后端发送通知的示例，请参阅 [向 iOS 应用添加推送通知](/azure/app-service-mobile/app-service-mobile-ios-get-started-push)。
- ASP.NET： [使用通知中心向用户发送推送通知](notification-hubs-aspnet-backend-ios-apple-apns-notification.md)。
- Azure 通知中心 Java SDK：有关从 Java 发送通知的信息，请参阅 [如何通过 Java 使用通知中心](notification-hubs-java-push-notification-tutorial.md) 。 这种方法已在 Eclipse for Android 开发环境中进行测试。
- PHP： [如何通过 PHP 使用通知中心](notification-hubs-php-push-notification-tutorial.md)。

## <a name="verify-that-your-app-receives-push-notifications"></a>验证应用可以接收推送通知

要在 iOS 上测试推送通知，必须将应用部署到物理 iOS 设备。 不能使用 iOS 模拟器发送 Apple 推送通知。

1. 运行应用并验证注册是否成功，然后按“确定” **** 。

   :::image type="content" source="media/ios-sdk/image7.png" alt-text="注册":::

2. 如上一部分所述，接下来可以从 [Azure 门户](https://portal.azure.cn/)发送测试推送通知。

3. 该推送通知会从给定通知中心发送到所有已注册接收通知的设备。

   :::image type="content" source="media/ios-sdk/image8.png" alt-text="发送测试":::

## <a name="next-steps"></a>后续步骤

在这个简单的示例中，已将推送通知广播到所有已注册的 iOS 设备。 要了解如何向特定 iOS 设备发送推送通知，请转到以下教程：

[教程：向特定设备推送通知](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

有关详细信息，请参阅以下文章：

- [Azure 通知中心概述](notification-hubs-push-notification-overview.md)
- [通知中心 REST API](https://docs.microsoft.com/rest/api/notificationhubs/)
- [用于后端操作的通知中心 SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [GitHub 上的通知中心 SDK](https://github.com/Azure/azure-notificationhubs)
- [使用应用程序后端注册](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [注册管理](notification-hubs-push-notification-registration-management.md)
- [使用标记](notification-hubs-tags-segment-push-message.md)
- [使用自定义模板](notification-hubs-templates-cross-platform-push-messages.md)
- [使用共享访问签名进行服务总线访问控制](/azure/service-bus-messaging/service-bus-sas)
- [以编程方式生成 SAS 令牌](https://docs.microsoft.com/rest/api/eventhub/generate-sas-token)
- [Apple 安全性：常见加密](https://developer.apple.com/security/)
