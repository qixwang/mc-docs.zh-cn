---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 06/28/2020
ms.author: v-junlch
ms.openlocfilehash: 4560212caa1f188aa0adb7eb6a15bf7dc52963d6
ms.sourcegitcommit: 3a8a7d65d0791cdb6695fe6c2222a1971a19f745
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2020
ms.locfileid: "85516521"
---
要在 Azure AD B2C 租户中注册应用程序，可以使用新的统一“应用注册”体验或旧版“应用程序(旧版)”体验 。 [详细了解此新体验](/active-directory-b2c/app-registrations-training-guide)。

#### <a name="app-registrations"></a>[应用注册](#tab/app-reg-ga/)

1. 登录 [Azure 门户](https://portal.azure.cn)。
1. 在顶部菜单中选择“目录 + 订阅”筛选器，然后选择包含Azure AD B2C 租户的目录。
1. 在左侧菜单中，选择“Azure AD B2C”。 或者，选择“所有服务”并搜索并选择“Azure AD B2C”。
1. 选择“应用注册”，然后选择“新建注册” 。
1. 输入应用程序的“名称”。 例如，ROPC_Auth_app。
1. 保留其他值不变，然后选择“注册”。
1. 记录“应用程序(客户端) ID”，以便在后续步骤中使用。
1. 在“管理”下，选择“身份验证”。 
1. 选择“尝试新体验”（如果已显示）。
1. 在“默认客户端类型”下，选择“是”，将应用程序视为公共客户端。 ROPC 流需要此设置。
1. 选择“保存”。
1. 在左侧菜单中，选择“清单”以打开清单编辑器。 
1. 将“oauth2AllowImplicitFlow”属性设置为“true”：
    ```json
    "oauth2AllowImplicitFlow": true,
    ```
1. 选择“保存”。

#### <a name="applications-legacy"></a>[应用程序(旧版)](#tab/applications-legacy/)

1. 登录 [Azure 门户](https://portal.azure.cn)。
1. 在顶部菜单中选择“目录 + 订阅”筛选器，然后选择包含Azure AD B2C 租户的目录。
1. 在左侧菜单中，选择“Azure AD B2C”。 或者，选择“所有服务”并搜索并选择“Azure AD B2C”。
1. 选择“应用程序(旧版)”，然后选择“添加” 。
1. 输入应用程序的名称。 例如，ROPC_Auth_app。
1. 对于“本机客户端”，请选择“是” 。
1. 保留其他值不变，然后选择“创建”。
1. 记录**应用程序 ID**，以便在以后的步骤中使用。

