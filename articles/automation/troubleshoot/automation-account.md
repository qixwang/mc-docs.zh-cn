---
title: Azure 自动化帐户故障排除
description: 了解如何排查和解决 Azure 帐户的问题。
services: automation
author: WenJason
ms.author: v-jay
origin.date: 03/24/2020
ms.date: 05/25/2020
ms.topic: conceptual
ms.service: automation
manager: digimobile
ms.openlocfilehash: 54ba7ae7007efe71058bcb2d89284051c873ea68
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801165"
---
# <a name="troubleshoot-an-azure-automation-account"></a>Azure 自动化帐户故障排除

本文介绍在使用 Azure 自动化帐户时可能会遇到的问题的解决方案。 有关自动化帐户的常规信息，请参阅[创建 Azure 帐户](../automation-quickstart-create-account.md)。

## <a name="scenario-unable-to-register-automation-resource-provider-for-subscriptions"></a><a name="rp-register"></a>场景：无法为订阅注册自动化资源提供程序

### <a name="issue"></a>问题

在自动化帐户中使用管理解决方案时，遇到以下错误：

```error
Error details: Unable to register Automation Resource Provider for subscriptions:
```

### <a name="cause"></a>原因

自动化资源提供程序未在订阅中注册。

### <a name="resolution"></a>解决方法

若要注册自动化资源提供程序，请在 Azure 门户中执行以下步骤：

1. 在浏览器中转到 [Azure 门户](https://portal.azure.cn)。

2. 转到“订阅”并选择你的订阅。   

3. 在“设置”下，选择“资源提供程序”。 

4. 在资源提供程序列表中，验证 Microsoft.Automation 资源提供程序是否已注册。

5. 如果该提供程序未列出，请按照[解决资源提供程序注册的错误](/azure-resource-manager/resource-manager-register-provider-errors)中所述进行注册。

## <a name="next-steps"></a>后续步骤

如果本文未解决你的问题，请尝试通过以下渠道之一获取更多支持：

* 提出 Azure 支持事件。 请转到 [Azure 支持站点](https://support.azure.cn/zh-cn/support/contact/)以获取支持。