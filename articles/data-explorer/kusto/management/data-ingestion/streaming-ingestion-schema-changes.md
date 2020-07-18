---
title: 流式引入和架构更改 - Azure 数据资源管理器
description: 本文介绍了在 Azure 数据资源管理器中通过流式引入处理架构更改的选项。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 05/20/2020
ms.date: 07/01/2020
ms.openlocfilehash: 7a498582afe7992ae9e8fa202f259ecf0fbf67e5
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226327"
---
# <a name="streaming-ingestion-and-schema-changes"></a>流式引入和架构更改

## <a name="background"></a>背景

群集节点会缓存通过流式引入接收数据的数据库的架构。 此过程会优化群集资源的性能和利用率，但可能会在架构更改时导致传播延迟。

架构更改的示例如下：

* 创建和删除数据库和表
* 添加、删除、重新键入或重命名表中的列
* 添加或删除预先创建的引入映射
* 添加、删除或更改策略

如果架构更改和流式引入流不协调，则某些流式引入请求可能会失败。 失败可能包括与架构相关的错误，或将不完整或失真的数据插入到表中。
在实现自定义引入应用程序时，强烈建议通过在有限的时间内执行重试操作或通过使用排队引入方法从失败的请求中重新路由数据来处理与架构相关的故障。

## <a name="clearing-the-schema-cache"></a>清除架构缓存

通过在群集节点上显式清除架构缓存来减小传播延迟的影响。
使用[清除用于流式引入的架构缓存](clear-schema-cache-command.md)管理命令之一清除架构缓存。
如果流式引入流和架构更改协调，则可以完全消除故障及其关联的数据失真。 

**协调流示例：**

1. 暂停流式引入。
1. 等待所有未处理的流式引入请求完成。
1. 进行架构更改。
1. 发出一个或多个 `.clear cache streaming ingestion` 架构命令。 
    * 重复上述操作直到成功，此时命令输出中的所有行都指示成功
1. 恢复流式引入。

> [!NOTE]
> 经常使用清除缓存流式引入架构命令可能会对流式引入的性能产生不利影响。
