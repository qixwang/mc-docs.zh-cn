---
title: Azure AD 条件访问中的自定义控件
description: 了解 Azure Active Directory 条件访问中的自定义控件的工作原理。
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 03/23/2020
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0da0fc13012838e2bf6397d0b090545a6e862e3c
ms.sourcegitcommit: 6568c59433d7e80ab06e9fe76d4791f761ed6775
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2020
ms.locfileid: "80243168"
---
# <a name="custom-controls-preview"></a>自定义控件（预览版）

自定义控件是 Azure Active Directory Premium P1 版的一项功能。 使用自定义控件时，用户将被重定向至兼容服务，以满足 Azure Active Directory 之外的其他要求。 若要满足此控件要求，用户浏览器将重定向至外部服务，执行任何需要的身份验证或验证活动，然后重定向回 Azure Active Directory。 Azure Active Directory 将验证响应，如果用户已成功完成身份验证或验证，该用户将继续留在条件访问流中。

通过这些控件可以将某些外部或自定义服务用作条件访问控制，并在一般情况下扩展条件访问的功能。

提供商当前提供的兼容服务包括：

- [Duo Security](https://duo.com/docs/azure-ca)
- [Entrust Datacard](https://www.entrustdatacard.com/products/authentication/intellitrust)
- [GSMA](https://mobileconnect.io/azure/)
- [Ping 标识](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_AzureADIntegration.html)
- [RSA](https://community.rsa.com/docs/DOC-81278)
- [SecureAuth](https://docs.secureauth.com/pages/viewpage.action?pageId=47238992#)
- [Silverfort](https://www.silverfort.io/company/using-silverfort-mfa-with-azure-active-directory/)
- [Symantec VIP](https://help.symantec.com/home/VIP_Integrate_with_Azure_AD)
- [Thales (Gemalto)](https://resources.eu.safenetid.com/help/AzureMFA/Azure_Help/Index.htm)
- [Trusona](https://www.trusona.com/docs/azure-ad-integration-guide)

有关这些服务的详细信息，请直接与提供商联系。

## <a name="creating-custom-controls"></a>创建自定义控件

若要创建自定义控件，应首先联系想使用的控件的提供商。 每个非 Microsoft 提供商在注册、订阅或以其他方式加入服务以及指示想要与条件访问集成方面都有自己的进程和要求。 此时，提供商将提供采用 JSON 格式的数据块。 使用此数据可使提供商和条件访问一起服务于租户，创建新控件，并确定条件访问如何判断用户是否通过提供商成功执行了验证。

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

