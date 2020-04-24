---
author: rockboyfor
ms.service: site-recovery
ms.topic: include
origin.date: 10/26/2018
ms.date: 12/10/2018
ms.author: v-yeche
ms.openlocfilehash: b3ca10b6b0c5763da951cbe61b5645238afa936f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63825797"
---
### <a name="installation-failures"></a>安装失败
| **示例错误消息** | **建议的操作** |
|--------------------------|------------------------|
|错误...未能加载帐户。 错误: System.IO.IOException: 安装和注册 CS 服务器时无法从传输连接读取数据。| 确保在计算机上启用 TLS 1.0。 |

### <a name="registration-failures"></a>注册失败
可以通过查看 **%ProgramData%\ASRLogs** 文件夹中的日志来调试注册失败。

| **示例错误消息** | **建议的操作** |
|--------------------------|------------------------|
|**09:20:06**:InnerException.Type: SrsRestApiClientLib.AcsException,InnerException。<br>消息: ACS50008: SAML 令牌无效。<br>跟踪 ID: 1921ea5b-4723-4be7-8087-a75d3f9e1072<br>相关 ID: 62fea7e6-2197-4be4-a2c0-71ceb7aa2d97><br>时间戳: **2016-12-12 14:50:08Z<br>** | 确保系统时钟上的时间与本地时间之间的偏差不超过 15 分钟。 重新运行安装程序完成注册。|
|**09:35:27**: 尝试获取选定证书的所有灾难恢复保管库时发生 DRRegistrationException:: 引发了 Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException，Exception.Message: ACS50008: SAML 令牌无效。<br>跟踪 ID: e5ad1af1-2d39-4970-8eef-096e325c9950<br>相关 ID: abe9deb8-3e64-464d-8375-36db9816427a<br>时间戳: **2016-05-19 01:35:39Z**<br> | 确保系统时钟上的时间与本地时间之间的偏差不超过 15 分钟。 重新运行安装程序完成注册。|
|06:28:45: 未能创建证书<br>06:28:45: 安装无法继续。 无法创建用于在 Site Recovery 中进行身份验证的证书。 重新运行安装程序 | 确保以本地管理员的身份运行安装程序。 |