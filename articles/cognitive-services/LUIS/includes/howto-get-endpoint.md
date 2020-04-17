---
title: include 文件
description: include 文件
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.date: 11/05/2019
ms.author: diberry
ms.openlocfilehash: 6fd3b665862d5949c6a78f678ba44dc21b51de20
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75857404"
---
在“管理”部分（右上方菜单），“Azure 资源”页（左侧菜单）上复制“示例查询”URL，然后粘贴到新的浏览器选项卡中    。 

终结点 URL 的格式如下所示，其中 APP-ID 和 KEY-ID 将替换为你自己的应用 ID 和终结点密钥：

```console
https://chinaeast2.api.cognitive.azure.cn/luis/prediction/v3.0/apps/APP-ID/slots/production/predict?subscription-key=KEY-ID&verbose=true&show-all-intents=true&log=true&query=YOUR_QUERY_HERE
```