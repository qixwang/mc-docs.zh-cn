---
title: include 文件
description: include 文件
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.date: 06/16/2020
ms.subservice: language-understanding
ms.topic: include
origin.date: 05/06/2020
ms.author: v-tawe
ms.openlocfilehash: 418a25455607a97b354904045139e4f6effb87bf
ms.sourcegitcommit: 48b5ae0164f278f2fff626ee60db86802837b0b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85098354"
---
在“管理”部分（右上方菜单），“Azure 资源”页（左侧菜单）上复制“示例查询”URL，然后粘贴到新的浏览器选项卡中************。 

终结点 URL 的格式如下所示，其中 APP-ID 和 KEY-ID 将替换为你自己的自定义子域、应用 ID 和终结点密钥：

```console
https://YOUR-CUSTOM-SUBDMAIN.api.cognitive.azure.cn/luis/prediction/v3.0/apps/APP-ID/slots/production/predict?subscription-key=KEY-ID&verbose=true&show-all-intents=true&log=true&query=YOUR_QUERY_HERE
```