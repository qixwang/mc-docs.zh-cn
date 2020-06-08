---
title: 管理 API 和服务 API 之间的差异
description: API 在 Azure Batch 服务的不同层上工作。
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: v-tawe
ms.custom: seodec18
origin.date: 02/26/2020
ms.openlocfilehash: eceed4c3e36773e340ee8fff131f4604ef3c351f
ms.sourcegitcommit: cbaa1aef101f67bd094f6ad0b4be274bbc2d2537
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2020
ms.locfileid: "84126766"
---
# <a name="service-level-and-management-level-apis"></a>服务级别 API 和管理级别 API

Azure Batch 有两组 API，一组用于服务级别，另一组用于管理级别。 这两组 API 命名通常相似，但返回不同的结果。 如果需要活动日志，则需要使用管理 API。 服务级别 API 会绕过 Azure 资源管理层，并且不会进行记录。


例如，Batch 管理和 Batch 服务都有用于池的 API。 
- 此类用于删除池的 API 直接针对批处理帐户： https://docs.microsoft.com/rest/api/batchservice/pool/delete 

- 此类用于删除池的 API (https://docs.microsoft.com/rest/api/batchmanagement/pool/delete ) 以 management.chinacloudapi.cn 层为目标。

