---
title: Azure AD 条件访问中的自定义控件
description: 了解 Azure Active Directory 条件访问中的自定义控件的工作原理。
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 04/24/2020
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: inbarc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 65c3ddbc3aabae60db938573bf1136c04bb7c285
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126288"
---
# <a name="custom-controls-preview"></a>自定义控件（预览版）

自定义控件是 Azure Active Directory 的预览功能。 使用自定义控件时，用户将被重定向到兼容服务，以满足 Azure Active Directory 之外的身份验证要求。 若要满足此控件要求，用户浏览器将重定向到外部服务，执行任何所需的身份验证，然后重定向回 Azure Active Directory。 Azure Active Directory 将验证响应，如果用户已成功完成身份验证或验证，该用户将继续留在条件访问流中。


## <a name="creating-custom-controls"></a>创建自定义控件

自定义控件使用有限的一组已批准身份验证提供程序。 若要创建自定义控件，应首先联系想使用的控件的提供商。 每个非 Microsoft 提供商在注册、订阅或以其他方式加入服务以及指示想要与条件访问集成方面都有自己的进程和要求。 此时，提供商将提供采用 JSON 格式的数据块。 使用此数据可使提供商和条件访问一起服务于租户，创建新控件，并确定条件访问如何判断用户是否通过提供商成功执行了验证。

复制 JSON 数据，然后将其粘贴到相关文本框中。 不要对 JSON 做任何更改，除非用户明确理解所做的更改。 做出任何更改可能中断提供商和 Microsoft 之间的联系，并且有可能将你和你的用户锁定在帐户之外。

创建自定义控件的选项位于“条件访问”  页的“管理”  部分中。

![控制](./media/controls/82.png)

单击“新建自定义控件”  ，打开包含控件 JSON 数据文本框的边栏选项卡。  

![控制](./media/controls/81.png)

## <a name="deleting-custom-controls"></a>删除自定义控件

若要删除自定义控件，必须先确定它未在任何条件访问策略中使用。 完成后：

1. 转到“自定义控件”列表
1. 单击...  
1. 选择“删除”  。

## <a name="editing-custom-controls"></a>编辑自定义控件

若要编辑自定义控件，必须删除当前控件，然后使用更新的信息创建新控件。

## <a name="next-steps"></a>后续步骤

- [条件访问常见策略](concept-conditional-access-policy-common.md)

- [使用条件访问 What If 工具模拟登录行为](troubleshoot-conditional-access-what-if.md)

