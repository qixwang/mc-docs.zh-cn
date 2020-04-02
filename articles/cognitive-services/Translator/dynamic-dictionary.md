---
title: 动态字典 - 文本翻译 API
titleSuffix: Azure Cognitive Services
description: 本文介绍如何使用 Azure 认知服务文本翻译 API 的动态字典功能。
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
origin.date: 06/04/2019
ms.date: 07/23/2019
ms.author: v-junlch
ms.openlocfilehash: a6623e4bd558ee72dab96bc06447820466727c37
ms.sourcegitcommit: 303a16c7117b6f3495ef0493b4ae8ccb67d7dbba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2020
ms.locfileid: "80342413"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>如何使用动态字典

若已知道要应用于某个单词或短语的翻译，可以在请求中将其作为标记提供。 动态词典仅适用于复合名词，例如专有名称和产品名称。

**语法：**

<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>

**要求：**

* `From` 和 `To` 语言必须不同。 
* 必须在 API 转换请求中包含 `From` 参数，而不是使用自动检测功能。 

**示例：en-de：**

源输入：The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.

目标输出：Das Wort "wordomatic" ist ein Wörterbucheintrag.

无论使用还是不使用 HTML 模式，此功能都以相同的方式工作。

应尽量少使用此功能。

<!-- Update_Description: wording update -->