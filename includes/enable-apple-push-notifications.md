---
ms.openlocfilehash: e6a07b714828cad80cee80858c4431d72aa90a19
ms.sourcegitcommit: d468b4e0c3288555b1ac5aec43c228328f49b60c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2020
ms.locfileid: "63860258"
---
* [为应用注册应用 ID](https://developer.apple.com/zh-cnlibrary/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingProfiles/MaintainingProfiles.html#//apple_ref/doc/uid/TP40012582-CH30-SW991)。 创建显式应用 ID（不是通配符应用 ID），对于“绑定 ID”，使用与 Xcode 快速入门项目中完全相同的“绑定 ID”   。 此外，必须选中“推送通知”选项  。 

* 接下来，[启用应用的推送通知](https://developer.apple.com/zh-cn/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringPushNotifications/ConfiguringPushNotifications.html#//apple_ref/doc/uid/TP40012582-CH32-SW6)。 用户可以创建“开发”或“分发”SSL 证书（请记住稍后在 Azure 门户中选择相应的选项）。

* 然后，[验证应用 ID 是否启用了推送通知](https://developer.apple.com/zh-cn/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringPushNotifications/ConfiguringPushNotifications.html#//apple_ref/doc/uid/TP40012582-CH32-SW8)。

* 最后，[刷新 Xcode 快速入门项目中的预配配置文件](https://developer.apple.com/zh-cn/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringPushNotifications/ConfiguringPushNotifications.html#//apple_ref/doc/uid/TP40012582-CH32-SW10)并[验证是否已创建或重新生成预配配置文件来启用推送通知](https://developer.apple.com/zh-cn/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringPushNotifications/ConfiguringPushNotifications.html#//apple_ref/doc/uid/TP40012582-CH32-SW12)。