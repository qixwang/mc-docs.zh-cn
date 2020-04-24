---
title: Azure API 管理策略示例 - 将请求上下文信息发送到后端服务
description: Azure API 管理策略示例 - 演示如何将请求上下文信息发送到后端服务。
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
ms.openlocfilehash: 9e7b2cca5b1c220c2b28cb0fa2fce11cec2c7e33
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63858235"
---
# <a name="send-request-context-information-to-the-backend-service"></a>将请求上下文信息转发到后端服务

本文介绍 Azure API 管理策略示例，该示例演示如何将请求上下文信息发送到后端服务。 若要设置或编辑策略代码，请执行[设置或编辑策略](../set-edit-policies.md)中所述的步骤。 若要查看其他示例，请参阅[策略示例](../policy-samples.md)。

## <a name="policy"></a>策略

将代码粘贴到“入站”块中  。

```xml
<!-- The policies described in this file show how to send some context information to the backend service for logging or processing. -->

<!-- Copy these snippets into the inbound element -->

<policies>
  <inbound>
    <base />
      <!-- Forward the name of the product associated with the subscription key in the request to the backend service. -->
      <set-query-parameter name="x-product-name" exists-action="override">
        <value>@(context.Product.Name)</value>
      </set-query-parameter>

      <!-- Forward the user id associated with the subscription key in the request as well as the region where the proxy processing the request is hosted. -->
      <set-header name="x-request-context-data" exists-action="override">
        <value>@(context.User.Id)</value>
        <value>@(context.Deployment.Region)</value>
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

