---
title: include 文件
description: include 文件
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.date: 08/07/2020
ms.subservice: language-understanding
ms.topic: include
ms.author: v-johya
ms.openlocfilehash: 28ff38e8190109e213e2edab2759bf782f775ce6
ms.sourcegitcommit: caa18677adb51b5321ad32ae62afcf92ac00b40b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2020
ms.locfileid: "88024222"
---
在“Azure 资源”页（左侧菜单）的“管理”部分（右上方菜单）中，复制“示例查询”URL，然后粘贴到新的浏览器选项卡中  。

终结点 URL 的格式如下所示，其中 APP-ID 和 KEY-ID 将替换为你自己的自定义子域、应用 ID 和终结点密钥：

```console
https://YOUR-CUSTOM-SUBDOMAIN.api.cognitive.azure.cn/luis/prediction/v3.0/apps/APP-ID/slots/production/predict?subscription-key=KEY-ID&verbose=true&show-all-intents=true&log=true&query=YOUR_QUERY_HERE
```

