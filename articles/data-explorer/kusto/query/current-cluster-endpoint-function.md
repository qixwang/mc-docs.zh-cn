---
title: current_cluster_endpoint() - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的 current_cluster_endpoint()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 02/13/2020
ms.date: 07/31/2020
ms.openlocfilehash: 5f447b9a5b48a13c38d18a246b6fa094362e62eb
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509248"
---
# <a name="current_cluster_endpoint"></a>current_cluster_endpoint()

返回所查询当前群集的网络终结点（DNS 名称）。

## <a name="syntax"></a>语法

`current_cluster_endpoint()`

## <a name="returns"></a>返回

所查询当前群集的网络终结点（DNS 名称），`string` 类型的值。

## <a name="example"></a>示例

```kusto
print strcat("This query executed on: ", current_cluster_endpoint())
```