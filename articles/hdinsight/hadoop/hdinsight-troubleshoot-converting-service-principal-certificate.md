---
title: 在 Azure HDInsight 中将服务主体证书内容转换为 base-64 编码的字符串格式
description: 在 Azure HDInsight 中将服务主体证书内容转换为 base-64 编码的字符串格式
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: v-yiso
origin.date: 07/31/2019
ms.date: 09/23/2019
ms.openlocfilehash: 902b1ea741b5692d0e69ff1907984ade7ea98206
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74388941"
---
# <a name="scenario-converting-service-principal-certificate-contents-to-base-64-encoded-string-format-in-azure-hdinsight"></a>方案：在 Azure HDInsight 中将服务主体证书内容转换为 base-64 编码的字符串格式

本文介绍在与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方案。

## <a name="issue"></a>问题

你会收到一条错误消息，指出输入不是有效的 Base-64 字符串，因为它包含非 Base 64 字符、两个以上的填充字符，或者在填充字符中包含非空格字符。

## <a name="cause"></a>原因

使用 PowerShell 或 Azure 模板部署创建 Data Lake 作为主要或附加存储的群集时，所提供的用于访问 Data Lake 存储帐户的服务主体证书内容采用 base-64 格式。 将 pfx 证书内容转换为 base-64 编码的字符串不正确可能会导致此错误。

## <a name="resolution"></a>解决方法

获得 pfx 格式的服务主体证书后（有关服务主体创建步骤的示例，请参阅[此处](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage)），请使用以下 PowerShell 命令或 C# 代码片段将证书内容转换为 base-64 格式。

```powershell
$servicePrincipalCertificateBase64 = [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes(path-to-servicePrincipalCertificatePfxFile))
```

```csharp
using System;
using System.IO;

namespace ConsoleApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            var certContents = File.ReadAllBytes(@"<path to pfx file>");
            string certificateData = Convert.ToBase64String(certContents);
            System.Diagnostics.Debug.WriteLine(certificateData);
        }
    }
}
```

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：


* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。 从菜单栏中选择“支持”  ，或打开“帮助 + 支持”  中心。 有关更多详细信息，请参阅[如何创建 Azure 支持请求](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)。 在 Microsoft Azure 订阅中可以访问订阅管理和计费支持；通过 [Azure 支持计划](https://azure.microsoft.com/support/plans/)之一提供技术支持。
