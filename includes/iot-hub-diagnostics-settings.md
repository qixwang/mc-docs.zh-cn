---
ms.openlocfilehash: 3f5d598c83ba8692af65b30efa9f356ec11f20cb
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73425890"
---
### <a name="enable-logging-with-diagnostics-settings"></a>通过诊断设置启用日志记录

[!INCLUDE [updated-for-az](./updated-for-az.md)]
1. 登录 [Azure 门户][lnk-portal]并导航到 IoT 中心。
1. 选择“诊断设置”  。
1. 选择“启用诊断”  。

   ![启用诊断](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

1. 为诊断设置提供名称。
1. 选择希望将日志发送到的目标。 可选择以下三个选项的任意组合：
   * 存档到存储帐户
   * 流式传输到事件中心
   * 发送到 Log Analytics
1. 选择要监视的操作，并为这些操作启用日志。 诊断设置可以报告的操作如下：
   * 连接
   * 设备遥测
   * 云到设备的消息
   * 设备标识操作
   * 文件上传
   * 消息路由
   * 云到设备孪生操作
   * 设备到云孪生操作
   * 孪生操作
   * 作业操作
   * 直接方法  
1. 保存新设置。 

如果想要通过 PowerShell 打开诊断设置，请使用以下代码：

```
Connect-AzAccount -EnvironmentName AzureChinaCloud
Select-AzSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

新设置在大约 10 分钟后生效。 在此之后，日志将出现在“诊断设置”  边栏选项卡上配置的存档目标中。 有关配置诊断的详细信息，请参阅[从 Azure 资源收集和使用日志数据](../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)。

<!-- Images -->
[1]: ./media/iot-hub-diagnostics-settings/turnondiagnostics.png

<!-- Links -->
[lnk-portal]: https://portal.azure.cn
[lnk-diagnostics-settings]: ../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
