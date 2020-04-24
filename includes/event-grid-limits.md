---
title: include 文件
description: include 文件
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 04/30/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 2c8db84a2012708e61e63f6a919c1b7ca67e1c63
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71151854"
---
以下限制适用于 Azure 事件网格系统主题和自定义主题，不适用于  事件域。

| 资源 | 限制 |
| --- | --- |
| 每个 Azure 订阅的自定义主题数 | 100 |
| 每个主题的事件订阅数 | 500 |
| 自定义主题的发布速率（入口） | 每个主题每秒 5,000 个事件 |
| 发布请求数 | 每秒 250 个 |
| 事件大小 | 正式发布版 (GA) 支持 64 KB。 目前预览版支持 1 MB。 |

以下限制仅适用于事件域。

| 资源 | 限制 |
| --- | --- |
| 每个事件域的主题数 | 公开预览期间为 1,000 个 |
| 域中每个主题的事件订阅数 | 公开预览期间为 50 个 |
| 域范围事件订阅数 | 公开预览期间为 50 个 |
| 事件域（入口）的发布速率 | 公开预览期间为每秒 5,000 个事件 |
| 发布请求数 | 每秒 250 个 |