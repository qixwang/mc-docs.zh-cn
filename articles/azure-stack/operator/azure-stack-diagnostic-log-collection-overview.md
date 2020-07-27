---
title: Azure Stack Hub 中的诊断日志收集
description: 了解 Azure Stack Hub 的“帮助 + 支持”中的诊断日志收集。
author: WenJason
ms.topic: article
origin.date: 02/26/2020
ms.date: 07/20/2020
ms.author: v-jay
ms.reviewer: shisab
ms.lastreviewed: 02/26/2020
ms.openlocfilehash: 6de3307d086ef47acf480dbfc910abe319ca4613
ms.sourcegitcommit: e9ffd50aa5eaab402a94bfabfc70de6967fe6278
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86307752"
---
# <a name="diagnostic-log-collection-in-azure-stack-hub"></a>Azure Stack Hub 中的诊断日志收集

::: moniker range=">= azs-2002"

Azure Stack Hub 是一个大型集合，包含可以彼此交互的 Windows 组件和本地 Azure 服务。 所有这些组件和服务都会生成自己的日志集。 为了让 Azure 支持能够有效地诊断问题，我们提供了用于收集诊断日志的无缝体验。

使用“帮助 + 支持”中的诊断日志收集功能，操作员可以在简单的用户界面中快速收集诊断日志并与 Azure 支持共享该日志，而无需使用 PowerShell。 即使其他基础结构服务关闭，也可以收集日志。  

建议使用此日志收集方法，仅当管理员门户或“帮助 + 支持”边栏选项卡不可用时才[使用特权终结点 (PEP)](azure-stack-get-azurestacklog.md)。

>[!NOTE]
>若要使用诊断日志收集功能，Azure Stack Hub 必须已注册，并且具有 Internet 连接。 如果 Azure Stack Hub 未注册，请[使用特权终结点 (PEP)](azure-stack-get-azurestacklog.md) 来共享日志。

![Azure Stack Hub 中的诊断日志收集选项](media/azure-stack-help-and-support/banner-enable-automatic-log-collection.png)

## <a name="collection-options-and-data-handling"></a>收集选项和数据处理

诊断日志收集功能提供两个用于发送日志的选项。 下表说明了每个选项以及如何在各种情况下处理数据。

### <a name="send-logs-proactively"></a>主动发送日志

[主动收集日志](azure-stack-configure-automatic-diagnostic-log-collection-tzl.md)可优化和简化诊断日志收集过程，让客户可以在建立支持案例之前将日志发送给 Azure。 可以从 Azure Stack Hub 主动上传要分析的诊断日志。 这些日志仅在发出了[系统运行状况警报](azure-stack-configure-automatic-diagnostic-log-collection-tzl.md#proactive-diagnostic-log-collection-alerts)的情况下收集，并且仅在建立了支持案例的情况下供 Azure 支持访问。

#### <a name="how-the-data-is-handled"></a>数据处理方式

你同意 Azure 可以仅根据 Azure Stack Hub 系统运行状况警报定期自动收集日志。 你还确认并同意，这些日志可以上传并保留在由 Azure 管理和控制的 Azure 存储帐户中。

这些数据只用于排查系统运行状况警报问题，在未经你同意的情况下不会用于市场营销、广告或任何其他商业目的。 这些数据最多可保留 90 天，由 Azure 收集的任何数据都会按照我们的[标准隐私做法](https://www.trustcenter.cn/privacy/default.html)进行处理。

如果你不再同意，则以前在你同意的情况下收集的任何数据都不会受到影响。

通过“主动收集日志”方式收集的日志会上传到由 Azure 管理和控制的 Azure 存储帐户中。 在收到支持案例的情况下，或者是为了改善 Azure Stack Hub 的运行状况，Azure 可能会访问这些日志。

### <a name="send-logs-now"></a>立即发送日志

[立即发送日志](azure-stack-configure-on-demand-diagnostic-log-collection-portal-tzl.md)是一个手动选项。如果选择该选项，那么仅当你以客户身份启动收集操作时（通常在提交支持案例之前），才会将诊断日志从 Azure Stack Hub 上传。

Azure Stack 操作员可以根据需要使用管理员门户或 PowerShell 将诊断日志发送到 Microsoft 客户支持服务 (CSS)。 如果 Azure Stack Hub 已连接到 Azure，建议使用[管理员门户中的“立即发送日志”选项](azure-stack-configure-on-demand-diagnostic-log-collection-portal-tzl.md)，因为这是直接将日志发送到 Azure 的最简单方法。 如果门户不可用，则操作员应改为[使用 PowerShell 立即发送日志](azure-stack-configure-on-demand-diagnostic-log-collection-powershell-tzl.md)。 

如果已断开与 Internet 的连接，或者只想在本地保存日志，请使用 [Get-AzureStackLog](azure-stack-get-azurestacklog.md) 方法发送日志。 以下流程图显示了各种情况下用于发送诊断日志的选项。

![流程图，显示如何将日志立即发送到 Microsoft](media/azure-stack-help-and-support/send-logs-now-flowchart.png)

#### <a name="how-the-data-is-handled"></a>数据处理方式

启动从 Azure Stack Hub 收集诊断日志的操作即表明你确认并同意，这些日志可以上传并保留到 Azure 管理和控制的 Azure 存储帐户中。 Azure 支持人员可以通过支持案例立即访问这些日志，而不必与客户联系以收集日志。

这些数据只用于排查系统运行状况警报问题，在未经你同意的情况下不会用于市场营销、广告或任何其他商业目的。 这些数据最多可保留 90 天，由 Azure 收集的任何数据都会按照我们的[标准隐私做法](https://www.trustcenter.cn/privacy/default.html)进行处理。 

使用“立即发送日志”选项收集的日志会上传到由 Azure 管理和控制的存储中。 在收到支持案例的情况下，或者是为了改善 Azure Stack Hub 的运行状况，Azure 会访问这些日志。

## <a name="bandwidth-considerations"></a>带宽注意事项

进行诊断日志收集时日志的平均大小各不相同，具体取决于它是主动运行还是手动运行。 “主动收集日志”选项对应的日志平均大小约为 2 GB。 “立即发送日志”选项对应的收集日志的大小取决于需要收集多少小时。

下表列出了在以受限或计量方式连接到 Azure 时的环境注意事项。

| 网络连接 | 影响 |
|----|---|
| 低带宽/高延迟连接 | 完成日志上传的时间会延长。 |
| 共享连接 | 上传也可能影响共享网络连接的其他应用/用户。 |
| 计量连接 | ISP 可能会针对你额外使用网络的情况收取额外费用。 |

::: moniker-end
::: moniker range="<= azs-1910"

## <a name="collecting-logs-from-multiple-azure-stack-hub-systems"></a>从多个 Azure Stack Hub 系统收集日志

为每个需要从其收集日志的 Azure Stack Hub 缩放单元设置一个 Blob 容器。 若要详细了解如何配置 Blob 容器，请参阅[配置 Azure Stack Hub 诊断日志自动收集](azure-stack-configure-automatic-diagnostic-log-collection-tzl.md)。 最佳做法是仅将同一 Azure Stack Hub 缩放单元中的诊断日志保存到单个 Blob 容器中。

## <a name="retention-policy"></a>保留策略

创建 Azure Blob 存储[生命周期管理规则](/storage/blobs/storage-lifecycle-management-concepts)，管理日志保留策略。 建议将诊断日志保留 30 天。 若要在 Azure 存储中创建生命周期管理规则，请登录到 Azure 门户，选择“存储帐户”再选择 Blob 容器，然后在“Blob 服务”下选择“生命周期管理”  。

![Azure 门户中的生命周期管理](media/azure-stack-automatic-log-collection/blob-storage-lifecycle-management.png)

## <a name="sas-token-expiration"></a>SAS 令牌过期

将 SAS URL 到期时间设置为两年。 如果续订存储帐户密钥，请确保重新生成 SAS URL。 应按最佳做法管理 SAS 令牌。 有关详细信息，请参阅[使用 SAS 时的最佳做法](/storage/common/storage-dotnet-shared-access-signature-part-1#best-practices-when-using-sas)。

## <a name="bandwidth-consumption"></a>带宽消耗

进行诊断日志收集时日志的平均大小各不相同，具体取决于日志收集是按需的还是自动的。

对于按需日志收集，进行日志收集时日志的大小取决于需要收集多少小时。 可以从过去七天选择 1-4 小时滑动窗口。

启用诊断日志自动收集时，服务会对是否存在严重警报进行监视。 在严重警报出现并持续大约 30 分钟后，服务会收集并上传相应的日志。 该日志收集的日志大小平均约为 2 GB。 如果修补升级失败，则仅当严重警报出现并持续约 30 分钟时，才会启动日志自动收集。 建议按照[监视修补升级指南](azure-stack-updates.md)操作。 警报监视、日志收集和上传对用户透明。

在正常运行的系统中，根本不会收集到日志。 在运行不正常的系统中，可能会每天运行两到三次日志收集，但通常只运行一次。 在最糟糕的情况下，每天可能运行多达 10 次。  

用户可以根据下表考虑在以受限或计量方式连接到 Azure 时启用日志自动收集对环境的影响。

| 网络连接 | 影响 |
|---|---|
| 低带宽/高延迟连接 | 完成日志上传的时间会延长。 | 
| 共享连接 | 上传也可能影响共享网络连接的其他应用/用户。 |
| 计量连接 | ISP 可能会针对你额外使用网络的情况收取额外费用。 |

## <a name="managing-costs"></a>管理成本

Azure [Blob 存储费用](https://azure.cn/pricing/details/storage/blobs/)取决于每月保存的数据量以及其他因素，例如数据冗余。 如果没有现有的存储帐户，可以登录到 Azure 门户，选择“存储帐户”，然后按步骤[创建 Azure Blob 容器 SAS URL](azure-stack-configure-automatic-diagnostic-log-collection-tzl.md)。

最佳做法是创建 Azure Blob 存储[生命周期管理策略](/storage/blobs/storage-lifecycle-management-concepts)，尽量降低持续产生的存储成本。 若要详细了解如何设置存储帐户，请参阅[配置 Azure Stack Hub 诊断日志自动收集](azure-stack-configure-automatic-diagnostic-log-collection-tzl.md)

::: moniker-end

## <a name="see-also"></a>另请参阅

[Azure Stack Hub 日志和客户数据处理](/azure-stack/operator/azure-stack-data-collection)
