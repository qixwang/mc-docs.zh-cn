---
title: 查找云 ID
description: 如何在 Azure Stack Hub 的“帮助 + 支持”中查找云 ID。
author: WenJason
ms.topic: article
origin.date: 10/08/2019
ms.date: 05/18/2020
ms.author: v-jay
ms.reviewer: shisab
ms.lastreviewed: 10/08/2019
ms.openlocfilehash: 968697f6b911dfe054eed8a51b6a301d2af9aed8
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83422983"
---
# <a name="find-your-cloud-id"></a>查找云 ID

本主题介绍如何使用管理员门户或特权终结点 (PEP) 获取云 ID。 

## <a name="use-the-administrator-portal"></a>使用管理员门户

1. 打开管理员门户。 
1. 单击“区域管理”。

   ![仪表板的屏幕截图](./media/azure-stack-automatic-log-collection/dashboard.png)

1. 单击“属性”并复制“印花云 ID”。

   ![具有印花云 ID 的区域属性的屏幕截图](media/azure-stack-automatic-log-collection/region-properties-blade-with-stamp-cloud-id.png)


## <a name="use-the-privileged-endpoint"></a>使用特权终结点

1. 打开权限提升的 PowerShell 会话，运行以下脚本。 根据环境需要，替换 PEP VM 的 IP 地址和云管理员凭据。 

   ```powershell
   $ipAddress = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.

   $password = ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
   $cred = New-Object -TypeName System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $password)
   $session = New-PSSession -ComputerName $ipAddress -ConfigurationName PrivilegedEndpoint -Credential $cred

   $stampInfo = Invoke-Command -Session $session { Get-AzureStackStampInformation }
   if ($session) {
       Remove-PSSession -Session $session
   }

   $stampInfo.CloudID
   ```

## <a name="next-steps"></a>后续步骤

* [主动发送日志](azure-stack-configure-automatic-diagnostic-log-collection-tzl.md)
* [立即发送日志](azure-stack-configure-on-demand-diagnostic-log-collection-portal-tzl.md)






