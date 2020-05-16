---
title: 管理帐户 - Azure Batch | Microsoft Docs
description: 了解 Azure Batch 帐户的组成
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: v-tawe
ms.custom: H1Hack27Feb2017
origin.date: 01/28/2020
ms.openlocfilehash: 8992134d2915db1e4b8d99671ace6a8a31a84cf8
ms.sourcegitcommit: 1fbdefdace8a1d3412900c6c3f89678d8a9b29bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/07/2020
ms.locfileid: "82886986"
---
# <a name="manage-your-batch-account"></a>管理 Batch 帐户

批处理帐户是批处理服务中唯一标识的实体。 所有处理都与一个 Batch 帐户相关联。

可以通过 [Azure 门户](batch-account-create-portal.md)或编程方式（例如使用[批处理管理 .NET 库](batch-management-dotnet.md)）创建 Azure Batch 帐户。 创建该帐户时，可以关联一个 Azure 存储帐户，用于存储与作业相关的输入和输出数据或应用程序。

可以在单个批处理帐户中运行多个批处理工作负荷，或者在相同订阅的不同 Azure 区域的批处理帐户之间分散工作负荷。
## <a name="components-of-the-batch-account"></a>Batch 帐户的组件

使用 Batch 帐户可在 Azure 中高效运行大规模并行的高性能计算 (HPC) 批处理作业。 在帐户中，你管理：

- 正在运行的应用程序

- 池和池中节点的分配

- 任务的数目和类型 

- 数据的输入和输出。 无需安装其他软件即可管理任务。

- 创建 Batch 帐户时，系统会要求你为该帐户指定一个名称。 此名称是其 ID，指定后无法更改。

- 若要更改帐户的名称，需要将其删除，然后新建一个 Batch 帐户。

- 该帐户将在要使用的订阅中创建。

- 使用该帐户从你订阅中的任何 Batch 帐户中识别和检索主帐户密钥和辅助帐户密钥。

- 该帐户保留池分配和核心配额方面的信息。  

- 该帐户包含位置信息。

- 该帐户标识你的存储帐户。

## <a name="next-steps"></a>后续步骤

- 使用 [Azure 门户](batch-account-create-portal.md)创建 Batch 帐户。
- 以编程方式（例如，使用 [Batch Management .NET 库](batch-management-dotnet.md)）创建 Batch 帐户。
- [配置或禁用对 Azure Batch 池中计算节点的远程访问](pool-endpoint-configuration.md)。
- [在 Batch 计算节点上运行作业准备和作业发布任务](batch-job-prep-release.md)
