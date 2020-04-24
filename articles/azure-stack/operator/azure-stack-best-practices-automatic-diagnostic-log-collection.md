---
title: 有关 Azure Stack Hub 日志自动收集的最佳做法
description: 在 Azure Stack Hub“帮助 + 支持”中进行自动日志收集的最佳做法
author: WenJason
ms.topic: article
origin.date: 07/25/2019
ms.date: 02/24/2020
ms.author: v-jay
ms.reviewer: prchint
ms.lastreviewed: 07/25/2019
ms.openlocfilehash: a951228717496b1d59d88699327fa979bc984003
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77540949"
---
# <a name="best-practices-for-automatic-azure-stack-hub-log-collection"></a>有关 Azure Stack Hub 日志自动收集的最佳做法 

本主题介绍为 Azure Stack Hub 管理诊断日志自动收集的最佳做法。 

## <a name="collecting-logs-from-multiple-azure-stack-hub-systems"></a>从多个 Azure Stack Hub 系统收集日志

为每个需要从其收集日志的 Azure Stack Hub 缩放单元设置一个 Blob 容器。 若要详细了解如何配置 Blob 容器，请参阅[配置 Azure Stack Hub 诊断日志自动收集](azure-stack-configure-automatic-diagnostic-log-collection.md)。 最佳做法是仅将同一 Azure Stack Hub 缩放单元中的诊断日志保存到单个 Blob 容器中。 

## <a name="retention-policy"></a>保留策略

创建 Azure Blob 存储[生命周期管理规则](/storage/blobs/storage-lifecycle-management-concepts)，管理日志保留策略。 建议将诊断日志保留 30 天。 若要在 Azure 存储中创建生命周期管理规则，请登录到 Azure 门户，单击“存储帐户”，接着单击 Blob 容器，然后在“Blob 服务”下单击“生命周期管理”。   

![屏幕截图，显示 Azure 门户中的生命周期管理](media/azure-stack-automatic-log-collection/blob-storage-lifecycle-management.png)


## <a name="sas-token-expiration"></a>SAS 令牌过期

将 SAS URL 到期时间设置为两年。 如果续订存储帐户密钥，请确保重新生成 SAS URL。 应按最佳做法管理 SAS 令牌。 有关详细信息，请参阅[使用 SAS 时的最佳做法](/storage/common/storage-dotnet-shared-access-signature-part-1#best-practices-when-using-sas)。


## <a name="bandwidth-consumption"></a>带宽消耗

进行诊断日志收集时日志的平均大小各不相同，具体取决于日志收集是按需的还是自动的。 

对于按需日志收集，进行日志收集时日志的大小取决于需要收集多少小时。 可以从过去七天选择 1-4 小时滑动窗口。 

启用诊断日志自动收集时，服务会对是否存在严重警报进行监视。 在严重警报出现并持续大约 30 分钟后，服务会收集并上传相应的日志。 该日志收集的日志大小平均约为 2 GB。 如果修补升级失败，就会启动日志自动收集，但前提是严重警报出现并持续约 30 分钟。 建议[按指南监视修补升级](azure-stack-updates.md)。
警报监视、日志收集和上传对用户透明。 



在正常系统中，根本不会收集日志。 在运行不正常的系统中，可能会每天运行两到三次日志收集，但通常只运行一次。 在最糟糕的情况下，每天可能运行多达十次。  

用户可以根据下表考虑在以受限或计量方式连接到 Azure 时启用日志自动收集对环境的影响。

| 网络连接 | 影响 |
|--------------------|--------|
| 低带宽/高延迟连接 | 完成日志上传的时间会延长。 | 
| 共享连接 | 上传也可能影响共享网络连接的其他应用程序/用户 |
| 计量连接 | ISP 可能会针对你额外使用网络的情况收取额外费用 |


## <a name="managing-costs"></a>管理成本

Azure [Blob 存储费用](https://azure.cn/pricing/details/storage/blobs/)取决于每月保存的数据量以及其他因素，例如数据冗余。 如果没有现有的存储帐户，可以登录到 Azure 门户，单击“存储帐户”，  然后按步骤[创建 Azure Blob 容器 SAS URL](azure-stack-configure-automatic-diagnostic-log-collection.md)。

最佳做法是创建 Azure Blob 存储[生命周期管理策略](/storage/blobs/storage-lifecycle-management-concepts)，尽量降低持续产生的存储成本。 若要详细了解如何设置存储帐户，请参阅[配置 Azure Stack Hub 诊断日志自动收集](azure-stack-configure-automatic-diagnostic-log-collection.md)

## <a name="see-also"></a>另请参阅

[配置 Azure Stack Hub 日志自动收集](azure-stack-best-practices-automatic-diagnostic-log-collection.md)

