---
title: 将 Azure 文件共享与 Azure 存储配合使用 | Microsoft Docs
description: 了解如何在 Windows 和 Windows Server 中使用 Azure 文件共享。
author: WenJason
manager: dcscontentpm
ms.service: storage
ms.topic: conceptual
origin.date: 11/19/2019
ms.date: 01/06/2020
ms.author: v-jay
ms.subservice: files
services: storage
tags: ''
ms.openlocfilehash: 16a0700f2796f3dfaf942815809b8283df4fc70d
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624434"
---
# <a name="how-to-recover-a-deleted-storage-account"></a>如何恢复删除的存储帐户

Azure 存储通过自动化副本提供数据复原能力，但不会阻止用户或应用程序代码破坏数据（不管这些行为是意外发生还是恶意发生）。 在遇到应用程序或用户错误时，维持数据保真度需要更先进的技术，例如使用审核日志将数据复制到辅助存储位置。

下表根据复制策略概述了存储帐户恢复的范围。

| |LRS|GRS|RA - GRS|
|---|---|---|---|
|存储帐户 Azure 资源管理器|是|是|是|
|经典存储帐户|是|是|是|

收集以下信息，并向 Azure 支持部门提出支持请求：

* 存储帐户名称
* 删除时间
* 存储帐户区域
* 如何删除存储帐户？
* 删除存储帐户的方法是什么？ （门户、PowerShell 等）

重要事项

* 通常，从存储服务的删除时间起，最多需要 15 天来执行垃圾回收，因此无法使用 SLA 恢复存储帐户。
* Azure 支持人员会尽力恢复容器/帐户，但无法保证该恢复。

> [!NOTE]
> 如果重新创建了帐户，则恢复可能会失败。 如果已经重新创建了帐户，则必须先将其删除，然后才能尝试进行恢复。
