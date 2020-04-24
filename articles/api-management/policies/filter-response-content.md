---
title: Azure API 管理策略示例 - 筛选响应内容
description: Azure API 管理策略示例 - 演示如何基于与请求关联的产品从响应有效负载中筛选数据元素。
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
ms.openlocfilehash: a456c1bb312e544e1d8fe42b1c7cd635a34445a9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63852372"
---
# <a name="filter-response-content"></a>筛选响应内容

本文介绍 Azure API 管理策略示例，该示例演示如何基于与请求关联的产品从响应有效负载中筛选数据元素。 若要设置或编辑策略代码，请执行[设置或编辑策略](../set-edit-policies.md)中所述的步骤。 若要查看其他示例，请参阅[策略示例](../policy-samples.md)。

## <a name="policy"></a>策略

将代码粘贴到“出站”块中  。

```xml
<!-- The policy defined in this file demonstrates how to filter data elements from the response payload based on the product associated with the request.
<!-- The snippet assumes that response content is formatted as JSON and contains root-level properties named "minutely", "hourly", "daily", "flags". -->

<!-- Use https://darksky.net/dev/ API to test this policy. -->

<!-- Copy this snippet into the outbound section. -->

<policies>
  <inbound>
    <base />
  </inbound>
  <backend>
    <base />
  </backend>
  <outbound>
    <base />
    <choose>
      <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals("Starter="""))">
        <set-body>
          @{
            <!-- NOTE that we are not using preserveContent=true when deserializing response body stream into a JSON object since we don't intend to access it again. See details on https://docs.azure.cn/zh-cn/api-management/api-management-transformation-policies#SetBody -->
            var response = context.Response.Body.As<JObject>();
            foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {
            response.Property (key).Remove ();
           }
          return response.ToString();
          }
        </set-body>
      </when>
    </choose>    
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

