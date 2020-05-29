---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 05/19/2020
ms.author: v-junlch
ms.openlocfilehash: 6eee9d316e15a4fd921827dadec2a09ede84556b
ms.sourcegitcommit: 87e789550ea49ff77c7f19bc68fad228009fcf44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2020
ms.locfileid: "83778237"
---
若要在 Azure AD B2C 租户中注册应用程序，可以使用当前的“应用程序”体验，或者使用我们新推出的统一“应用注册(预览版)”体验。 [详细了解此新体验](/active-directory/develop/app-registrations-training-guide-for-app-registrations-legacy-users)。

#### <a name="applications"></a>[应用程序](#tab/applications/)

1. 登录到 [Azure 门户](https://portal.azure.cn)。
1. 在顶部菜单中选择“目录 + 订阅”筛选器，然后选择包含Azure AD B2C 租户的目录。
1. 在左侧菜单中，选择“Azure AD B2C”。 或者，选择“所有服务”并搜索并选择“Azure AD B2C”。
1. 选择“应用程序”，然后选择“添加” 。
1. 输入应用程序的名称。 例如，*ROPC_Auth_app*。
1. 对于**本机客户端**，选择“是”。
1. 保留其他值不变，然后选择“创建”。
1. 记录**应用程序 ID**，以便在以后的步骤中使用。

#### <a name="app-registrations-preview"></a>[应用注册（预览版）](#tab/app-reg-preview/)

1. 登录到 [Azure 门户](https://portal.azure.cn)。
1. 在顶部菜单中选择“目录 + 订阅”筛选器，然后选择包含Azure AD B2C 租户的目录。
1. 在左侧菜单中，选择“Azure AD B2C”。 或者，选择“所有服务”并搜索并选择“Azure AD B2C”。
1. 选择“应用注册(预览版)”，然后选择“新建注册” 。
1. 输入应用程序的“名称”。 例如，*ROPC_Auth_app*。
1. 保留其他值不变，然后选择“注册”。
1. 记录“应用程序(客户端) ID”，以便在后续步骤中使用。
1. 在“管理”下，选择“身份验证”。 
1. 选择“尝试新体验”（如果已显示）。
1. 在“默认客户端类型”下，对于“将应用程序视为公共客户端”，选择“是” 。 ROPC 流需要此设置。
1. 选择“保存” 。
1. 在左侧菜单中，选择“清单”打开清单编辑器。 
1. 将 oauth2AllowImplicitFlow 属性设置为 true：
    ```json
    "oauth2AllowImplicitFlow": true,
    ```
1. 选择“保存” 。

