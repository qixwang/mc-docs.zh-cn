---
title: 渗透测试 | Microsoft Docs
description: 本文概述了渗透测试（简称 pentest）过程，以及如何对运行在 Azure 基础结构中的应用进行渗透测试。
services: security
documentationcenter: na
author: lingliw
manager: digimobile
editor: ''
ms.assetid: 695d918c-a9ac-4eba-8692-af4526734ccc
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
origin.date: 08/13/2018
ms.date: 03/02/2020
ms.author: v-lingwu
ms.openlocfilehash: 795287c22a03253e3fecd17ae6f4fc7b713ab411
ms.sourcegitcommit: 2b4507745b98b45f1ce3f3d30f397521148ef35a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2020
ms.locfileid: "78213787"
---
# <a name="penetration-testing"></a>渗透测试
使用 Azure 进行应用程序测试和部署的一个优点是可快速创建环境。 用户不需担心如何请求、获取和安装自己的本地硬件。

此功能很有用，但用户仍需确保执行正常的安全防护措施。 你可能想要做的事情之一就是对部署在 Azure 中的应用程序进行渗透测试。

用户可能已经知道，Microsoft 将执行[对 Azure 环境的渗透测试](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)。 这有助于改进 Azure。

我们不会为你对应用程序进行渗透测试，但我们确实了解你想要并需要对自己的应用程序进行渗透测试。 这是好事，因为改进自己的应用程序的安全性可以加强整个 Azure 生态系统的安全性。

自 2017 年 6 月 15 日起，Microsoft 不再需要预先批准即可针对 Azure 资源进行渗透测试。 愿意正式记录即将进行的针对 Microsoft Azure 的渗透测试活动的用户，请填写 [Azure 服务渗透测试通知表](https://portal.msrc.microsoft.com/en-us/engage/pentest)。 本流程仅与 Microsoft Azure 相关，并不适用于任何其他 Microsoft 云服务。

>[!IMPORTANT]
>虽然参加渗透测试时无需再通知 Microsoft，客户仍须遵守 [Microsoft 云统一渗透测试参与规则](https://technet.microsoft.com/mt784683)。

可以执行的标准测试包括：

* 对终结点进行测试，以发现[开放 Web 应用程序安全项目 (OWASP) 的前 10 个漏洞](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
* 终结点的[模糊测试](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/)
* 终结点的[端口扫描](https://en.wikipedia.org/wiki/Port_scanner)

用户不能执行的一类测试是任何类型的 [拒绝服务 (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) 攻击。 其中包括：发起 DoS 攻击，或者执行相关的测试，以便确定、演示或模拟任何类型的 DoS 攻击。

## <a name="next-steps"></a>后续步骤

- 如果你想正式记录即将针对 Microsoft Azure 中托管的应用程序进行的渗透测试，请继续阅读[渗透测试参与规则](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=2)并填写测试通知表单。
