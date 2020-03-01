---
title: Azure Stack Hub 日志和客户数据处理
description: 了解 Azure Stack Hub 如何收集客户数据和信息。
author: WenJason
ms.topic: article
origin.date: 06/10/2019
ms.date: 02/24/2020
ms.author: v-jay
ms.reviewer: chengwei
ms.lastreviewed: 06/10/2019
ms.openlocfilehash: 6fa9e8fd487605bc8c42f4438184558f20cbf713
ms.sourcegitcommit: afe972418a883551e36ede8deae32ba6528fb8dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77541003"
---
# <a name="azure-stack-hub-log-and-customer-data-handling"></a>Azure Stack Hub 日志和客户数据处理 

在某种程度上 Azure 是 Azure Stack Hub 相关个人数据的处理方或辅助处理方，Azure 对所有客户提供以下承诺（从 2018 年 5 月 25 日开始生效）：

- [联机服务条款](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)的“数据保护条款”部分中的“个人数据的处理；GDPR”条款。
- [联机服务条款](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)附件 4 中的“欧盟一般数据保护条例条款”。

由于 Azure Stack Hub 驻留在客户数据中心，因此，对于通过[诊断](azure-stack-configure-on-demand-diagnostic-log-collection.md#use-the-privileged-endpoint-pep-to-collect-diagnostic-logs)和[遥测](azure-stack-telemetry.md)与 Azure 共享的数据，Azure 是唯一的数据控制方。  

## <a name="data-access-controls"></a>数据访问控制 
Azure 员工受派调查特定的支持案例时，将获得加密数据的只读访问权限。 如果需要，Azure 员工还有权访问用于删除数据的工具。 对客户数据的所有访问都会受到审核和记录。  

数据访问控制：
- 在结案后，数据最多只保留 90 天。
- 在 90 天期限内，客户随时可以删除数据。
- Azure 员工获得的数据访问权限根据不同的案例而定，并且只在需要帮助解决支持问题时才能获得该访问权限。 
- 如果 Azure 必须与 OEM 合作伙伴共享客户数据，必须经得客户的同意。  

### <a name="what-data-subject-requests-dsr-controls-do-customers-have"></a>客户可以实施哪些数据主题请求 (DSR) 控制？
Azure 根据客户请求提供按需删除数据的支持。 客户可以请求我们的支持工程师之一在任何时间删除给定案例的所有日志，然后再将数据永久擦除。  

### <a name="does-azure-notify-customers-when-the-data-is-deleted"></a>删除数据时，Azure 是否通知客户？
对于自动化数据删除操作（案例关闭后 90 天），我们不会主动联系客户并通知他们有关删除的信息。

对于按需删除数据操作，Azure 支持工程师有权访问相应的工具进行按需数据删除。 他们可以在完成删除后通过电话向客户提供确认。

## <a name="diagnostic-data"></a>诊断数据
在支持过程中，Azure Stack Hub 操作员可与 Azure Stack Hub 支持和工程团队[共享诊断日志](azure-stack-configure-on-demand-diagnostic-log-collection.md#use-the-privileged-endpoint-pep-to-collect-diagnostic-logs)，以方便进行故障排除。

Azure 为客户提供所需的工具和脚本用于收集及上传请求的诊断日志文件。 收集日志文件后，这些文件将通过 HTTPS 保护的加密连接发送到 Azure。 由于 HTTPS 提供在线加密，因此传输中加密无需密码。 Azure 收到日志后，会加密并存储日志，在关闭支持案例 90 天后自动将其删除。

## <a name="telemetry-data"></a>遥测数据
[Azure Stack Hub 遥测](azure-stack-telemetry.md)通过互连用户体验将系统数据自动上传到 Azure。 Azure Stack Hub 操作员可以随时控制自定义遥测功能和隐私设置。

Azure 无意收集敏感数据，例如信用卡号、用户名和密码、电子邮件地址等。 如果我们确定敏感信息是无意中收集到的，我们会予以删除。

## <a name="next-steps"></a>后续步骤 
[详细了解 Azure Stack Hub 安全性](azure-stack-security-foundations.md) 
