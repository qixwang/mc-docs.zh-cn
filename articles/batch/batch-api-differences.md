---
title: 管理 API 和服务 API 之间的差异 - Azure Batch | Microsoft Docs
description: API 在 Azure Batch 服务的不同层上工作。
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: v-tawe
ms.custom: seodec18
origin.date: 02/26/2020
ms.openlocfilehash: c501d67be8dddb05f4aa5d04c527704b889b78e9
ms.sourcegitcommit: 1fbdefdace8a1d3412900c6c3f89678d8a9b29bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2020
ms.locfileid: "82886999"
---
# <a name="service-level-and-management-level-apis"></a>服务级别 API 和管理级别 API

Azure Batch 有两组 API，一组用于服务级别，另一组用于管理级别。 这两组 API 命名通常相似，但返回不同的结果。 如果需要活动日志，则需要使用管理 API。 服务级别 API 会绕过 Azure 资源管理层，并且不会进行记录。


例如，Batch 管理和 Batch 服务都有用于池的 API。 
- 此类用于删除池的 API 直接针对批处理帐户： https://docs.microsoft.com/rest/api/batchservice/pool/delete 

- 此类用于删除池的 API (https://docs.microsoft.com/rest/api/batchmanagement/pool/delete ) 以 management.chinacloudapi.cn 层为目标。

