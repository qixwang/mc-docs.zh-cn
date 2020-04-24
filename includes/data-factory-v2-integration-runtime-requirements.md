---
author: WenJason
ms.service: data-factory
ms.topic: include
origin.date: 08/12/2019
ms.date: 10/14/2019
ms.author: jingwang
ms.openlocfilehash: 36d0ae62f0474036ae3714ff1a9e2a87502efc21
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72275444"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
如果数据存储是以下方式之一配置的，则需要设置[自托管集成运行时](../articles/data-factory/create-self-hosted-integration-runtime.md)才能连接到此数据存储：

- 数据存储位于本地网络内部、Azure 虚拟网络内部或 Amazon 虚拟私有云内。
- 数据存储是一种托管云数据服务，在其中访问权限限制为列入防火墙规则允许列表的 IP。
