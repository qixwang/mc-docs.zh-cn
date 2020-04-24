---
title: Azure API 管理策略示例 - 设置响应缓存持续时间
description: Azure API 管理策略示例 - 演示如何使用后端发送的 Cache-Control 标头中的 maxAge 值设置响应缓存持续时间。
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
ms.openlocfilehash: dc83398160b27f8c10dfc52b4569a2060ac373d4
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63847001"
---
# <a name="set-response-cache-duration"></a>设置响应缓存持续时间

本文介绍 Azure API 管理策略示例，该示例演示如何使用后端发送的 Cache-Control 标头中的 maxAge 值设置响应缓存持续时间。 若要设置或编辑策略代码，请执行[设置或编辑策略](../set-edit-policies.md)中所述的步骤。 若要查看其他示例，请参阅[策略示例](../policy-samples.md)。

## <a name="policy"></a>策略

将代码粘贴到“入站”块中  。

```xml
<!-- This snippet demonstrates how to set response cache duration using maxAge value in Cache-Control header sent by the backend. -->

<!-- Copy the following snippet into the outbound section. -->
      
<policies>
  <inbound>
    <base />
  </inbound>
  <backend>
    <base />
  </backend>
  <outbound>
    <base />
    <!-- The following policy can be tested by setting up an API and operation mapped to GET http://httpbin.org/cache/{duration} -->
    <cache-store duration="@{
        var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");
            var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;
            return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;
        }"
     />
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

