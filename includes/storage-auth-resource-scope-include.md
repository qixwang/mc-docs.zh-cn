---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 03/28/2019
ms.date: 05/27/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 40789da97b89e44d76ca2d802ff860b6de6f4568
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "66004042"
---
在将 RBAC 角色分配到某个安全主体之前，请确定该安全主体应该获取的访问范围。 最佳做法指出，最好是授予尽可能小的范围。

以下列表描述了可将 Azure Blob 和队列资源访问权限限定到哪些级别，从最小的范围开始：

- **单个容器。** 在此范围内，角色分配适用于容器中的所有 Blob，以及容器属性和元数据。
- **单个队列。** 在此范围内，角色分配适用于队列中的消息，以及队列属性和元数据。
- **存储帐户。** 在此范围内，角色分配适用于所有容器及其 Blob，或者适用于所有队列及其消息。
- **资源组。** 在此范围内，角色分配适用于资源组中所有存储帐户内的所有容器或队列。
- **订阅。** 在此范围内，角色分配适用于订阅中所有资源组内的所有存储帐户中的所有容器或队列。

