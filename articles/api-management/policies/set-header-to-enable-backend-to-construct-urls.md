---
title: Azure API 管理策略示例 - 添加 Forwarded 标头
description: Azure API 管理策略示例 - 演示如何在入站请求中添加 Forwarded 标头，以允许后端 API 构造正确的 URL。
services: api-management
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 10/13/2017
ms.date: 02/26/2018
ms.author: v-yiso
ms.openlocfilehash: 8eb0397f3e792fc125faf51478fa6e05293a53b2
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63824718"
---
# <a name="add-a-forwarded-header"></a>添加 Forwarded 标头

本文介绍 Azure API 管理策略示例，该示例演示如何在入站请求中添加 Forwarded 标头，以允许后端 API 构造正确的 URL。 若要设置或编辑策略代码，请执行[设置或编辑策略](../set-edit-policies.md)中所述的步骤。 若要查看其他示例，请参阅[策略示例](../policy-samples.md)。

## <a name="code"></a>代码

将代码粘贴到“入站”块中  。

```xml
<!-- The policy defined in this file demonstrates how to add a Forwarded header in the inbound request to allow the backend API to construct proper URLs.

<!-- Forwarded header is defined in the IETF RFC 7239 https://tools.ietf.org/html/rfc7239  -->

<!-- Copy this snippet into the inbound section. -->

<policies>
  <inbound>
    <base />
    <set-header exists-action="override" name="Forwarded">
      <value>@("proto=" + context.Request.OriginalUrl.Scheme + ";host=" + context.Request.OriginalUrl.Host + ";")</value>
    </set-header>
  </inbound>
  <backend>
    <base />
  </backend>
  <outbound>
    <base />
  </outbound>
  <on-error>
    <base />
  </on-error>
</policies>
```

## <a name="next-steps"></a>后续步骤

了解有关 APIM 策略的详细信息：

+ [转换策略](../api-management-transformation-policies.md)
+ [策略示例](../policy-samples.md)
