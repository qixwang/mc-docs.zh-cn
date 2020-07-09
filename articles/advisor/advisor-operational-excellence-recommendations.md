---
title: 使用 Azure 顾问为 Azure 订阅改进卓越运营
description: 使用顾问为 Azure 订阅优化和完善卓越运营
ms.topic: article
origin.date: 10/24/2019
ms.date: 06/28/2020
ms.author: v-tawe
ms.openlocfilehash: 7c795dc03f5d446903f50b8d5eace36f240ba645
ms.sourcegitcommit: 3a8a7d65d0791cdb6695fe6c2222a1971a19f745
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2020
ms.locfileid: "85516804"
---
# <a name="achieve-operational-excellence-with-azure-advisor"></a>使用 Azure 顾问实现卓越运营

Azure 顾问卓越运营建议可在过程和工作流效率、资源可管理性以用最佳部署做法等方面为客户提供帮助。 可以在顾问仪表板的“卓越运营”选项卡上获取这些来自顾问的建议。

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>创建 Azure 服务运行状况警报，以便在 Azure 问题影响你时收到通知

我们建议设置 Azure 服务运行状况警报，以便在 Azure 服务问题影响你时收到通知。 [Azure 服务运行状况](https://www.azure.cn/features/service-health/)是一项免费服务，可在你受到 Azure 服务问题影响时提供个性化指导和支持。 顾问会识别未配置警报的订阅，并建议创建一个警报。


## <a name="design-your-storage-accounts-to-prevent-hitting-the-maximum-subscription-limit"></a>设计存储帐户以防止达到最大订阅限制

一个 Azure 区域可以支持每个订阅最多 250 个存储帐户。 一旦达到该限制，将无法再在该区域/订阅组合中创建更多存储帐户。 顾问将会检查你的订阅并显示建议，以便你为任何即将达到最大限制的订阅设计较少的存储帐户。

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>确保在需要时有权访问 Azure 云专家

在运行业务关键型工作负载时，在需要时有权访问技术支持至关重要。 顾问标识在其支持计划中不包含技术支持的潜在业务关键型订阅，并建议升级到包含技术支持的选项。

## <a name="delete-and-recreate-your-pool-to-remove-a-deprecated-internal-component"></a>删除并重新创建你的池，以移除弃用的内部组件

你的池使用了某个弃用的内部组件。 请删除并重新创建池，以提高稳定性和性能。

## <a name="repair-invalid-log-alert-rules"></a>修复无效的日志警报规则

Azure 顾问将会检测出那些在其条件部分指定了无效查询的警报规则。 日志警报规则在 Azure Monitor 中创建，用于按指定时间间隔运行分析查询。 查询结果决定了是否需要触发警报。 随着时间的推移，分析查询可能会因所引用资源、表或命令的变化而变得无效。 顾问将会建议你更正警报规则中的查询以防止自动禁用，并确保监视 Azure 中你的所有资源。

## <a name="follow-best-practices-using-azure-policy"></a>遵循使用 Azure Policy 的最佳做法

Azure Policy 是 Azure 中的一项服务，用于创建、分配和管理策略。 这些策略对资源强制实施不同的规则和效果。 下面是有助于实现卓越运营的 Azure Policy 建议： 
1. 使用 Azure Policy 管理标记：创建或更新任何资源时，此策略将添加或替换指定的标记和值。 可以通过触发修正任务来修正现有资源。 此外，这还不会修改资源组上的标记。
2. 使用 Azure Policy 强制实施地域合规性要求：利用该策略能够限制组织在部署资源时可以指定的位置。 
3. 指定允许进行部署的虚拟机 SKU：此策略可用于指定组织可部署的一组虚拟机 SKU。
4. 使用 Azure Policy 强制执行“审核不使用托管磁盘的 VM”
5. 利用 Azure Policy 使用“从资源组继承标记”：在创建或更新任何资源时，该策略添加或替换来自父资源组中的指定标记和值。 可以通过触发修正任务来修正现有资源。

## <a name="next-steps"></a>后续步骤

若要了解有关顾问建议的详细信息，请参阅以下资源：
* [顾问简介](advisor-overview.md)
* [入门](advisor-get-started.md)
* [顾问成本建议](advisor-cost-recommendations.md)
* [顾问性能建议](advisor-performance-recommendations.md)
* [顾问高可用性建议](advisor-high-availability-recommendations.md)
* [顾问安全性建议](advisor-security-recommendations.md)
