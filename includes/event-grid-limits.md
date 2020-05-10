---
title: include 文件
description: include 文件
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 04/29/2020
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 380dac8d665b37cc4b3f3424571197c80162a27a
ms.sourcegitcommit: 95efd248f5ee3701f671dbd5cfe0aec9c9959a24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516003"
---
以下限制适用于 Azure 事件网格系统主题和自定义主题，不适用于  事件域。

| 资源 | 限制 |
| --- | --- |
| 每个 Azure 订阅的自定义主题数 | 100 |
| 每个主题的事件订阅数 | 500 |
| 自定义主题的发布速率（入口） | 每个主题每秒 5,000 个事件 |
| 发布请求数 | 每秒 250 个 |
| 事件大小 | 1 MB（以多个 64 KB 事件的形式收费） |

以下限制仅适用于事件域。

| 资源 | 限制 |
| --- | --- |
| 每个事件域的主题数 | 100,000 |
| 域中每个主题的事件订阅数 | 500 |
| 域范围事件订阅数 | 50 |
| 事件域（入口）的发布速率 | 每秒 5,000 个事件 |
| 发布请求数 | 每秒 250 个 |
| 每个 Azure 订阅的事件域数 | 100 |

