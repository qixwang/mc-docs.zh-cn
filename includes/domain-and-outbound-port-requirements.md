---
title: include 文件
description: include 文件
services: data-factory
author: WenJason
ms.service: data-factory
ms.topic: include
origin.date: 10/09/2019
ms.date: 06/29/2019
ms.author: v-jay
ms.openlocfilehash: d4a5f243e5c969ad3b67a7d3095450ff9d78749b
ms.sourcegitcommit: f5484e21fa7c95305af535d5a9722b5ab416683f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85319439"
---
| 域名                  | 出站端口 | 说明                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.chinacloudapi.cn`    | 443            | 自承载集成运行时连接到 Azure 数据工厂中的数据移动服务时需要此端口。 |
| `*.frontend.datamovement.azure.cn` | 443            | 自承载集成运行时连接到数据工厂服务时需要此端口。 |
| `download.microsoft.com`    | 443            | 自承载集成运行时下载更新时需要此端口。 如果已禁用自动更新，则可以跳过对此域的配置。 |
| `*.core.chinacloudapi.cn`          | 443            | 使用[分阶段复制](/data-factory/copy-activity-performance#staged-copy)功能时，由自承载集成运行时用来连接到 Azure 存储帐户。 |
| `*.database.chinacloudapi.cn`      | 1433           | 仅当从/向 Azure SQL 数据库或 Azure SQL 数据仓库复制时才为必需项，否则为可选项。 在不打开端口 1433 的情况下，使用暂存复制功能将数据复制到 SQL 数据库或 SQL 数据仓库。 |
