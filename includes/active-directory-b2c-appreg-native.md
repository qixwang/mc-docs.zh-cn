---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
origin.date: 10/16/2019
ms.date: 11/25/2019
ms.author: v-junlch
ms.openlocfilehash: 5ec41f3ec78fe7d85c6531ac70bc62da3fdd05a8
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74461593"
---
若要在 Azure AD B2C 租户中注册应用程序，可以使用当前“应用程序”体验  。

#### <a name="applications"></a>[应用程序](#tab/applications/)

1. 登录到 [Azure 门户](https://portal.azure.cn)。
1. 在顶部菜单中选择“目录 + 订阅”  筛选器，然后选择包含Azure AD B2C 租户的目录。
1. 在左侧菜单中，选择“Azure AD B2C”  。 或者，选择“所有服务”  并搜索并选择“Azure AD B2C”  。
1. 选择“应用程序”，然后选择“添加”   。
1. 输入应用程序的名称。 例如，“nativeapp1”  。
1. 对于**本机客户端**，选择“是”  。
1. 输入使用唯一方案的**自定义重定向 URI**。 例如，`com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`。 选择重定向 URI 时，有两个重要的注意事项：
    * **唯一**：每个应用程序的重定向 URI 的方案必须是唯一的。 在示例 `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect` 中，`com.onmicrosoft.contosob2c.exampleapp` 为方案。 应遵循此模式。 如果两个应用程序共享同一方案，则用户应选择一个应用程序。 如果用户选择不正确，登录会失败。
    * **完整**：重定向 URI 必须同时包含方案和路径。 路径必须在域之后包含至少一个正斜杠。 例如，`//oauth/` 有效，而 `//oauth` 会失败。 不要在 URI 中包含特殊字符，例如，下划线。
1. 选择“创建”  。

