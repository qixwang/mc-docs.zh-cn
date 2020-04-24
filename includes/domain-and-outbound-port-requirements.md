---
title: include 文件
description: include 文件
services: data-factory
author: WenJason
ms.service: data-factory
ms.topic: include
origin.date: 10/09/2019
ms.date: 11/11/2019
ms.author: v-jay
ms.openlocfilehash: 2cd3157e9ca1c352746e98561e24e41aee2c260c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73648764"
---
| 域名                  | 出站端口 | 说明                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.chinacloudapi.cn`    | 443            | 自承载集成运行时连接到数据工厂中的数据移动服务时需要此端口。 |
| `*.frontend.datamovement.azure.cn` | 443            | 自承载集成运行时连接到数据工厂服务时需要此端口。 |
| `download.microsoft.com`    | 443            | 自承载集成运行时下载更新时需要此端口。 如果已禁用自动更新，则可以跳过此设置。 |
| `*.core.chinacloudapi.cn`          | 443            | 使用[分阶段复制](/data-factory/copy-activity-performance#staged-copy)功能时，由自承载集成运行时用来连接到 Azure 存储帐户。 |
| `*.database.chinacloudapi.cn`      | 1433           | （可选）从/向 Azure SQL 数据库或 Azure SQL 数据仓库复制时需要。 在不打开端口 1433 的情况下，使用暂存复制功能将数据复制到 Azure SQL 数据库或 Azure SQL 数据仓库。 |
