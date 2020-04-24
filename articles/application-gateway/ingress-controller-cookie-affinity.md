---
title: 使用应用程序网关启用基于 Cookie 的相关性
description: 本文介绍如何使用应用程序网关启用基于 Cookie 的相关性。
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
origin.date: 11/04/2019
ms.date: 11/19/2019
ms.author: v-junlch
ms.openlocfilehash: e56c31be76faf4513a22b9731183630b2fe0f155
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74328434"
---
# <a name="enable-cookie-based-affinity-with-an-application-gateway"></a>使用应用程序网关启用基于 Cookie 的相关性
如 [Azure 应用程序网关文档](/application-gateway/application-gateway-components#http-settings)中所述，应用程序网关支持基于 Cookie 的相关性，这意味着它可以将后续流量从用户会话定向到同一服务器进行处理。

## <a name="example"></a>示例
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: guestbook
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: frontend
          servicePort: 80
```

