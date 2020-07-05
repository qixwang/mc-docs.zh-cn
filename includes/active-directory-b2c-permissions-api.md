---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 06/28/2020
ms.author: v-junlch
ms.openlocfilehash: 3a139773b62a2e97326031a7ff9058a2102045eb
ms.sourcegitcommit: 3a8a7d65d0791cdb6695fe6c2222a1971a19f745
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2020
ms.locfileid: "85516463"
---
#### <a name="app-registrations"></a>[应用注册](#tab/app-reg-ga/) 

1. 选择“应用注册”，然后选择应该有权访问 API 的 Web 应用程序。 例如，“webapp1”。
1. 在“管理”下选择“API 权限”。
1. 在“已配置权限”下，选择“添加权限”。
1. 选择“我的 API”选项卡。
1. 选择应授予 Web 应用程序对其的访问权限的 API。 例如，“webapi1”。
1. 在“权限”下展开“演示”，然后选择前面定义的范围。  例如，*demo.read* 和 *demo.write*。
1. 选择“添加权限”。
1. 选择“向(租户名称)授予管理员许可”。
1. 如果系统提示你选择一个帐户，请选择当前登录的管理员帐户，或者使用至少分配了“云应用程序管理员”角色的 Azure AD B2C 租户中的帐户登录。
1. 请选择“是”。
1. 选择“刷新”，然后确认两个范围的“状态”下是否均显示“已授予...”。 

#### <a name="applications-legacy"></a>[应用程序(旧版)](#tab/applications-legacy/)

1. 选择“应用程序(旧版)”，然后选择应该有权访问 API 的 Web 应用程序。 例如，“webapp1”。
1. 选择“API 访问”，然后选择“添加” 。
1. 在“选择 API”下拉列表中，选择应授予 Web 应用程序访问权限的 API。 例如，“webapi1”。
1. 在“选择范围”下拉列表中，选择先前定义的范围。 例如，*demo.read* 和 *demo.write*。
1. 选择“确定”。

