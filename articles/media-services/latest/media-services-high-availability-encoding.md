---
title: Azure 媒体服务高可用性编码
description: 了解在发生区域性数据中心中断或故障时，如何故障转移到次要媒体服务帐户。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.subservice: ''
ms.workload: ''
ms.topic: article
ms.custom: ''
origin.date: 02/24/2020
ms.date: 07/27/2020
ms.author: v-jay
ms.openlocfilehash: 24962f14ec2898f08ea470b62262d253c01bb370
ms.sourcegitcommit: 091c672fa448b556f4c2c3979e006102d423e9d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87162751"
---
# <a name="media-services-high-availability-encoding"></a>媒体服务高可用性编码 

Azure 媒体服务编码服务是一个区域性批处理平台，当前不是针对单个区域中的高可用性设计的。 如果出现区域性数据中心中断，或者基础组件或从属服务（例如存储、SQL）发生故障，编码服务目前不提供服务的即时故障转移。 本文介绍如何部署媒体服务来维护具有故障转移功能的高可用性体系结构，并确保应用程序的最佳可用性。

遵循本文中所述的指导原则和最佳做法后，如果单个区域中发生中断，你将能够降低编码失败的风险、延迟，并最大程度地缩短恢复时间。

## <a name="how-to-build-a-cross-regional-encoding-system"></a>如何构建跨区域编码系统

* [创建](create-account-cli-how-to.md)两个（或更多）Azure 媒体服务帐户。

    这两个帐户必须位于不同的区域。 有关详细信息，请参阅[部署 Azure 媒体服务服务的区域](https://azure.microsoft.com/global-infrastructure/services/?regions=china-non-regional,china-east,china-east-2,china-north,china-north-2&products=media-services)。
* 将媒体上传到计划在其中提交作业的同一区域。 有关如何开始编码的详细信息，请参阅[从 HTTPS URL 创建作业输入](job-input-from-http-how-to.md)或[从本地文件创建作业输入](job-input-from-local-file-how-to.md)。

    如果随后需要将[作业](transforms-jobs-concept.md)重新提交到另一个区域，你可以使用 JobInputHttp 或 [Copy-Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) 将数据从源资产容器复制到备用区域中的资产容器。
* 通过 Azure 事件网格订阅每个帐户中的 JobStateChange 消息。 有关详细信息，请参阅：

    * [音频分析示例](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/AudioAnalytics/AudioAnalyzer)，该示例展示如何使用 Azure 事件网格监视作业，包括添加回退以防止 Azure 事件网格消息因某些原因而延迟。
    * [媒体服务事件的 Azure 事件网格架构](media-services-event-schemas.md)
    * [通过 Azure 门户或 CLI 注册事件](reacting-to-media-services-events.md)（也可以使用 EventGrid 管理 SDK 进行注册）
    * [Microsoft.Azure.EventGrid SDK](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/)（原生支持媒体服务事件）。

    你还可以通过 Azure Functions 使用事件网格事件。
* 当你创建[作业](transforms-jobs-concept.md)时：

    * 从当前使用的帐户列表中随机选择一个帐户（此列表通常包含两个帐户，但如果检测到问题，它可能只包含一个帐户）。 如果列表为空，则引发警报，以便操作员能够进行调查。
    * 一般指导原则是，每个 [JobOutput](https://docs.microsoft.com/rest/api/media/jobs/create#joboutputasset) 需要一个[媒体保留单位](media-reserved-units-cli-how-to.md)。
    * 获取所选帐户的媒体保留单位 (MRU) 的计数。 如果当前的媒体保留单位计数尚未达到最大值，请添加作业所需的 MRU 数并更新服务。 如果作业提交率很高，并且你经常查询 MRU 来了解是否达到最大值，请对具有合理超时的值使用分布式缓存。
    * 记录正在进行的作业的数量。

* 当 JobStateChange 处理程序收到作业已达到计划状态的通知时，请记录其进入计划状态的时间以及所使用的区域/帐户。
* 当 JobStateChange 处理程序收到作业已达到处理状态的通知时，将该作业的记录标记为正在处理。
* 当 JobStateChange 处理程序收到作业已达到“已完成/错误/已取消”状态的通知时，将该作业的记录标记为“最终”，并减少正在进行的作业计数。 获取所选帐户的媒体保留单位数量，然后将当前 MRU 数量与正在进行的作业数量进行比较。 如果正在进行的作业计数小于 MRU 计数，则减少正在进行的作业计数并更新服务。
* 设置定期查看作业记录的单独进程
    
    * 如果给定区域中处于计划状态的作业在合理的时间内没有进入正在处理状态，请从当前所用的帐户列表中删除该区域。  取决于你的业务需求，你可以决定立即取消这些作业，然后将它们重新提交给其他区域。 你也可以耐心等待这些作业转到下一个状态。
    * 取决于帐户中配置的媒体保留单位数量和提交速率，可能还有一些作业处于排队状态，系统尚未对这些作业进行处理。  如果处于排队状态的作业列表超出某个区域的可接受限制，则可以取消这些作业并将它们提交给其他区域。  但是，这可能是由于帐户中没有为当前负载配置足够的媒体保留单位而造成的。  如有必要，可以通过 Azure 支持请求更高的媒体保留单位配额。
    * 如果某个区域已从帐户列表中删除，请先监视该区域的恢复状况，然后再将其添加回列表中。  可以通过区域上的现有作业来监视区域运行状况（如果未取消并重新提交这些作业），方法是在一段时间后将帐户重新添加到列表中，并通过操作员监视有关可能影响 Azure 媒体服务的中断的 Azure 通信。
    
如果你发现 MRU 计数上下波动很大，请将递减逻辑移至定期任务。 使用作业前提交逻辑将正在进行的作业计数与当前 MRU 计数进行比较，以查看其是否需要更新 MRU。

## <a name="next-steps"></a>后续步骤

* [构建点播视频跨区域流式处理](media-services-high-availability-streaming.md)
* 查看[代码示例](https://docs.microsoft.com/samples/browse/?products=azure-media-services)
