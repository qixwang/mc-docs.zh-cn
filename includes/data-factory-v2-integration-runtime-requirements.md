---
author: WenJason
ms.service: data-factory
ms.topic: include
origin.date: 08/12/2019
ms.date: 06/29/2020
ms.author: jingwang
ms.openlocfilehash: d07081ad5cae61591ae5156134a69d3ae2806500
ms.sourcegitcommit: f5484e21fa7c95305af535d5a9722b5ab416683f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/24/2020
ms.locfileid: "85320307"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
如果数据存储是以下方式之一配置的，则需要设置[自承载集成运行时](../articles/data-factory/create-self-hosted-integration-runtime.md)才能连接到此数据存储：

- 数据存储位于本地网络内部、Azure 虚拟网络内部或 Amazon 虚拟私有云内。
- 数据存储是一种托管的云数据服务，只有在防火墙规则中列入允许列表的 IP 才能访问该服务。
