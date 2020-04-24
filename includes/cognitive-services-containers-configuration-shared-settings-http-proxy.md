---
author: IEvangelist
ms.author: v-tawe
origin.date: 06/25/2019
ms.date: 09/24/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: cdd4fcfc021b359a6e05b4241d91f442ea2f94d4
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71267041"
---
如果需要配置 HTTP 代理以发出出站请求，请使用以下两个参数：

| 名称 | 数据类型 | 说明 |
|--|--|--|
|HTTP_PROXY|字符串|要使用的代理，例如 `http://proxy:8888`<br>`<proxy-url>`|
|HTTP_PROXY_CREDS|字符串|对代理进行身份验证所需的凭据，例如 username:password。|
|`<proxy-user>`|字符串|代理的用户。|
|`<proxy-password>`|字符串|与代理的 `<proxy-user>` 关联的密码。|
||||


```bash
docker run --rm -it -p 5000:5000 \
--memory 2g --cpus 1 \
--mount type=bind,src=/home/azureuser/output,target=/output \
<registry-location>/<image-name> \
Eula=accept \
Billing=<endpoint> \
ApiKey=<api-key> \
HTTP_PROXY=<proxy-url> \
HTTP_PROXY_CREDS=<proxy-user>:<proxy-password> \
```
