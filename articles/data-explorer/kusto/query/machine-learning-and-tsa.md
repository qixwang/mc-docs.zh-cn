---
title: 时序分析 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍 Azure 数据资源管理器中的时序分析。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 05/06/2019
ms.date: 08/06/2020
ms.openlocfilehash: 02320de8b63f93e7a6b5819e863ef876eedfbe91
ms.sourcegitcommit: 7ceeca89c0f0057610d998b64c000a2bb0a57285
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87841295"
---
# <a name="time-series-analysis"></a>时序分析 

Kusto 查询语言以本机数据类型形式提供系列支持。
make-series 运算符将数据转换为序列数据类型，并提供一系列函数，用于对该数据类型进行高级处理。 这些函数涵盖数据清除、机器学习建模和离群值检测。