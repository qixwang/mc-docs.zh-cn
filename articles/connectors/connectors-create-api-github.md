---
title: 连接到 GitHub - Azure 逻辑应用
description: 使用 GitHub REST API 和 Azure 逻辑应用监视 GitHub 事件
services: logic-apps
documentationcenter: ''
author: ecfan
manager: anneta
editor: ''
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
origin.date: 03/02/2018
ms.author: v-yiso
ms.date: 10/08/2018
ms.openlocfilehash: 5f10636862d1da804e182e55b0c598a981831487
ms.sourcegitcommit: 1ac138a9e7dc7834b5c0b62a133ca5ce2ea80054
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2020
ms.locfileid: "78304694"
---
# <a name="connect-to-github-from-azure-logic-apps"></a>从 Azure 逻辑应用连接到 GitHub

GitHub 是基于 Web 的 Git 存储库托管服务，它提供 Git 的所有分布式修订控制和源代码管理 (SCM) 功能以及其他功能。

若要开始使用 GitHub 连接器，[首先创建逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。

## <a name="create-a-connection-to-github"></a>创建到 GitHub 的连接

若要在逻辑应用中使用 GitHub 连接器，必须先创建*连接*，然后提供以下属性的详细信息： 

| 属性 | 必须 | 说明 | 
| -------- | -------- | ----------- | 
| 令牌 | 是 | 提供 GitHub 凭据。 |

创建连接后，可执行操作并侦听触发器，如本文所述。

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-reference"></a>连接器参考

有关触发器、操作和限制（请参阅连接器的 OpenAPI（以前称为 Swagger）说明）的技术详细信息，请查看[连接器的参考页](https://docs.microsoft.com/connectors/github/)。

## <a name="next-steps"></a>后续步骤

* 了解其他[逻辑应用连接器](../connectors/apis-list.md)