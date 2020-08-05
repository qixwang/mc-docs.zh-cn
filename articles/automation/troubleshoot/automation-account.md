---
title: 排查 Azure 自动化帐户问题
description: 本文介绍如何排查和解决 Azure 帐户问题。
services: automation
author: WenJason
ms.author: v-jay
origin.date: 03/24/2020
ms.date: 08/10/2020
ms.topic: conceptual
ms.service: automation
manager: digimobile
ms.openlocfilehash: 1bfc77732e28ec012a114717b2abebd024c6bc08
ms.sourcegitcommit: e6b216b180734783219378410e13192e314a4497
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87788295"
---
# <a name="troubleshoot-azure-automation-account-issues"></a>排查 Azure 自动化帐户问题

本文介绍使用 Azure 自动化帐户时可能会遇到的问题的解决方案。 有关自动化帐户的常规信息，请参阅 [Azure 自动化帐户身份验证概述](../automation-security-overview.md)。

## <a name="scenario-unable-to-register-automation-resource-provider-for-subscriptions"></a><a name="rp-register"></a>场景：无法为订阅注册自动化资源提供程序

### <a name="issue"></a>问题

在自动化帐户中使用管理功能（例如更新管理）时，遇到以下错误：

```error
Error details: Unable to register Automation Resource Provider for subscriptions:
```

### <a name="cause"></a>原因

未在订阅中注册自动化资源提供程序。

### <a name="resolution"></a>解决方法

若要注册自动化资源提供程序，请在 Azure 门户中执行以下步骤：

1. 在浏览器中转到 [Azure 门户](https://portal.azure.cn)。

2. 转到“订阅”，选择你的订阅。   

3. 在“设置”下，选择“资源提供程序”。

4. 在资源提供程序列表中，验证是否注册了 Microsoft.Automation 资源提供程序。

5. 如果未列出提供程序，请按照[解决资源提供程序注册错误](../../azure-resource-manager/templates/error-register-resource-provider.md)中所述的方式进行注册。

## <a name="next-steps"></a>后续步骤

如果本文未解决你的问题，请尝试通过以下渠道之一获取更多支持：

* 提出 Azure 支持事件。 请转到 [Azure 支持站点](https://support.azure.cn/zh-cn/support/contact/)以获取支持。