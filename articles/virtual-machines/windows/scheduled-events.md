---
title: Azure 元数据服务 - 适用于 Windows VM 的计划事件
description: Windows 虚拟机上使用 Azure 元数据服务的计划事件。
author: rockboyfor
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure-services
origin.date: 06/01/2020
ms.date: 07/06/2020
ms.author: v-yeche
ms.openlocfilehash: 7ffd6ac9a7f9ae1bb16592e6f0295f9fb9bc180b
ms.sourcegitcommit: 89118b7c897e2d731b87e25641dc0c1bf32acbde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85945597"
---
# <a name="azure-metadata-service-scheduled-events-for-windows-vms"></a>Azure 元数据服务：适用于 Windows VM 的计划事件

预定事件是一个 Azure 元数据服务，可提供应用程序时间用于准备虚拟机维护。 它提供有关即将发生的维护事件的信息（例如重新启动），使应用程序可以为其准备并限制中断。 它可用于 Windows 和 Linux 上的所有 Azure 虚拟机类型（包括 PaaS 和 IaaS）。 

有关 Linux 上的计划事件的信息，请参阅[适用于 Linux VM 的计划事件](../linux/scheduled-events.md)。

> [!Note] 
> 计划事件在所有 Azure 区域中正式发布。 有关最新版本信息，请参阅[版本和区域可用性](#version-and-region-availability)。

## <a name="why-scheduled-events"></a>为何使用计划事件？

许多应用程序都可以受益于时间来准备虚拟机维护。 时间可以用于执行应用程序的特定任务的提高可用性、可靠性和可维护性，包括： 

- 检查点和还原
- 连接清空
- 主要副本故障转移 
- 从负载均衡器池删除
- 事件日志记录
- 正常关闭 

使用计划事件，应用程序可以发现维护的发生，并触发任务以限制其影响。 启用计划事件可在执行维护活动之前为虚拟机提供最少的时间。 有关详细信息，请参阅下面的“事件计划”部分。

预定事件提供以下用例中的事件：
- [平台启动的维护](/virtual-machines/windows/maintenance-and-updates)（例如，VM 重新启动、实时迁移或主机的内存保留更新）
- 虚拟机在预计很快将出现故障的[降级后的主机硬件](https://azure.microsoft.com/blog/find-out-when-your-virtual-machine-hardware-is-degraded-with-scheduled-events)上运行
- 用户启动的维护（例如，用户重启或重新部署 VM）

    <!--Not Available on - [Spot VM](spot-vms.md) and [Spot scale set](../../virtual-machine-scale-sets/use-spot.md) instance evictions-->

## <a name="the-basics"></a>基础知识  

Azure 元数据服务公开在 VM 中使用可访问的 REST 终结点运行虚拟机的相关信息。 该信息通过不可路由的 IP 提供，因此不会在 VM 外部公开。

### <a name="endpoint-discovery"></a>终结点发现
对于启用了 VNET 的 VM，元数据服务可通过不可路由的静态 IP (`169.254.169.254`) 使用。 最新版本的计划事件的完整终结点是： 

 > `http://169.254.169.254/metadata/scheduledevents?api-version=2019-08-01`

如果不是在虚拟网络中创建虚拟机（云服务和经典 VM 的默认情况），则需使用额外的逻辑以发现要使用的 IP 地址。 请参阅此示例，了解如何[发现主机终结点](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm)。

### <a name="version-and-region-availability"></a>版本和区域可用性
计划事件服务受版本控制。 版本是必需的，当前版本为 `2019-01-01`。

| 版本 | 发布类型 | 区域 | 发行说明 | 
| - | - | - | - |
| 2019-08-01 | 正式版 | 全部 | <li> 添加了对 EventSource 的支持 |
| 2019-04-01 | 正式版 | 全部 | <li> 添加了对事件说明的支持 |
| 2019-01-01 | 正式版 | 全部 | <li> 添加了对虚拟机规模集 EventType“Terminate”的支持 |
| 2017-08-01 | 正式版 | 全部 | <li> 已从 IaaS VM 的资源名称中删除前置下划线<br /><li>针对所有请求强制执行元数据标头要求 | 
| 2017-03-01 | 预览 | 全部 |<li>初始版本 |

<!--Not Available on | 2017-11-01 | General Availability | All | <li> Added support for Low-priority VM eviction EventType 'Preempt'<br /> |-->

> [!NOTE] 
> 支持的计划事件的前一预览版 {latest} 发布为 api-version。 此格式不再受支持，并且将在未来弃用。

### <a name="enabling-and-disabling-scheduled-events"></a>启用和禁用计划事件
首次为事件发出请求时，为服务启用了计划事件。 首次调用时应该会延迟响应最多两分钟。 你应定期查询终结点，以便检测到即将发生的维护事件以及正在执行的维护活动的状态。

如果 24 小时未发出请求，将为服务禁用计划事件。

### <a name="user-initiated-maintenance"></a>用户启动的维护
用户通过 Azure 门户、API、CLI 或 PowerShell 启动的虚拟机维护会生成计划事件。 这样便可以在应用程序中测试维护准备逻辑，并可以通过应用程序准备用户启动的维护。

重启虚拟机会计划 `Reboot` 类型的事件。 重新部署虚拟机会计划 `Redeploy` 类型的事件。

## <a name="using-the-api"></a>使用 API

### <a name="headers"></a>头文件
查询元数据服务时，必须提供标头 `Metadata:true` 以确保不会在无意中重定向该请求。 `Metadata:true` 标头对于所有预定事件请求是必需的。 不在请求中包含标头会导致元数据服务发出的“错误的请求”响应。

### <a name="query-for-events"></a>查询事件
只需进行以下调用即可查询计划事件：

#### <a name="powershell"></a>PowerShell
```
curl http://169.254.169.254/metadata/scheduledevents?api-version=2019-08-01 -H @{"Metadata"="true"}
```

响应包含计划事件的数组。 数组为空意味着目前没有计划事件。
如果有计划事件，响应会包含事件的数组： 

<!--MOONCAKE: Not Avaialble on | "Preempt" -->

```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze" | "Terminate",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},
            "Description": {eventDescription},
            "EventSource" : "Platform" | "User",
        }
    ]
}
```

<!--MOONCAKE: Not Avaialble on | "Preempt" -->

DocumentIncarnation 是一个 ETag，它提供了一种简单的方法来检查自上次查询以来事件有效负载是否已更改。

### <a name="event-properties"></a>事件属性

<!--MOONCAKE: Not Available on EventType <li>`Preempt`: The Low-priority Virtual Machine is being deleted (ephemeral disks are lost).-->

|属性  |  说明 |
| - | - |
| EventId | 此事件的全局唯一标识符。 <br /><br /> 示例： <br /><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| EventType | 此事件造成的影响。 <br /><br /> 值： <br /><ul><li> `Freeze`：虚拟机计划暂停数秒。 CPU 和网络连接可能会暂停，但对内存或打开的文件没有影响。 <li>`Reboot`：计划重启虚拟机（非永久性内存丢失）。 <li>`Redeploy`：计划将虚拟机移到另一节点（临时磁盘将丢失）。 <li> `Terminate`：计划删除虚拟机。 |
| ResourceType | 此事件影响的资源的类型。 <br /><br /> 值： <ul><li>`VirtualMachine`|
| 资源| 此事件影响的资源的列表。 它保证最多只能包含一个[更新域](manage-availability.md)的计算机，但可能不包含该更新域中的所有计算机。 <br /><br /> 示例： <br /><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| 事件状态 | 此事件的状态。 <br /><br /> 值： <ul><li>`Scheduled`：此事件计划在 `NotBefore` 属性指定的时间之后启动。<li>`Started`：此事件已启动。</ul> 不提供 `Completed` 或类似状态；事件完成后，将不再返回事件。
| NotBefore| 此事件可能会在之后启动的时间。 <br /><br /> 示例： <br /><ul><li> 2016 年 9 月 19 日星期一 18:29:47 GMT  |
| 说明 | 此事件的说明。 <br /><br /> 示例： <br /><ul><li> 主机服务器正在维护中。 |
| EventSource | 事件的发起者。 <br /><br /> 示例： <br /><ul><li> `Platform`：此事件是由平台发起的。 <li>`User`：此事件是由用户发起的。 |

<!--MOONCAKE, Not Available on Line 125 EventType <li>`Preempt`: The Low-priority Virtual Machine is being deleted (ephemeral disks are lost).-->

### <a name="event-scheduling"></a>事件计划
将根据事件类型为每个事件计划将来的最小量时间。 此时间反映在某个事件的 `NotBefore` 属性上。 

|EventType  | 最小值通知 |
| - | - |
| 冻结| 15 分钟 |
| 重新启动 | 15 分钟 |
| 重新部署 | 10 分钟 |
| 终止 | [用户可配置](../../virtual-machine-scale-sets/virtual-machine-scale-sets-terminate-notification.md#enable-terminate-notifications)：5 - 15 分钟 |

<!--Not Available on Line 132 | Preempt | 30 seconds |-->

> [!NOTE] 
> 在某些情况下，由于硬件降级，Azure 能够预测主机故障，并会尝试通过对迁移进行计划来缓解服务中断。 受影响的虚拟机会收到计划事件，该事件的 `NotBefore` 通常是将来几天的时间。 实际时间因预测的故障风险评估而异。 Azure 会尽可能提前 7 天发出通知，但实际时间可能会有变化，如果预测硬件即将发生故障的可能性很大，则实际时间可能更早。 为了最大程度地降低服务的风险，以防硬件在系统启动迁移之前出现故障，建议尽快自行重新部署虚拟机。

### <a name="event-scope"></a>事件作用域     
计划的事件传送到：
 - 独立虚拟机。
 - 云服务中的所有虚拟机。     
 - 可用性集中的所有虚拟机。 
 - 规模集放置组中的所有虚拟机（包括 Batch）。       

<!--Not Available on  availability zone-->

### <a name="starting-an-event"></a>启动事件 

了解即将发生的事件并完成正常关闭逻辑后，可以通过使用 `EventId` 对元数据服务进行 `POST` 调用来批准未完成的事件。 这指示 Azure 可以缩短最小通知时间（如可能）。 

下面是 `POST` 请求正文中所需的 json。 请求应包含 `StartRequests` 列表。 每个 `StartRequest` 包含想要加速的事件的 `EventId`：
```
{
    "StartRequests" : [
        {
            "EventId": {EventId}
        }
    ]
}
```

#### <a name="powershell"></a>PowerShell
```
curl -H @{"Metadata"="true"} -Method POST -Body '{"StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' -Uri http://169.254.169.254/metadata/scheduledevents?api-version=2019-01-01
```

> [!NOTE] 
> 确认事件后，即可允许事件针对事件中所有的 `Resources` 继续进行，而不仅仅是确认该事件的虚拟机。 因此，可以选择一个指挥计算机来协调该确认，为简单起见，可选择 `Resources` 字段中的第一个计算机。

## <a name="powershell-sample"></a>PowerShell 示例 

下例将查询计划事件的元数据服务器并审核所有未完成的事件。

```powershell
# How to get scheduled events 
function Get-ScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function Approve-ScheduledEvent($eventId, $uri)
{
    # Create the Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests} 

    # Convert to JSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with the following: `n" $approvalString

    # Post approval string to scheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function Handle-ScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up the scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2019-01-01' -f $localHostIP 

# Get events
$scheduledEvents = Get-ScheduledEvents $scheduledEventURI

# Handle events however is best for your service
Handle-ScheduledEvents $scheduledEvents

# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
        Approve-ScheduledEvent $event.EventId $scheduledEventURI 
    }
}
``` 

## <a name="next-steps"></a>后续步骤 

<!-- Not Available on https://channel9.msdn.com/-->

- 在 [Azure 实例元数据计划事件 GitHub 存储库](https://github.com/Azure-Samples/virtual-machines-scheduled-events-discover-endpoint-for-non-vnet-vm)中查看计划事件代码示例
- 有关 API 的更多信息，请参阅[实例元数据服务](instance-metadata-service.md)。
- 了解 [Azure 中 Windows 虚拟机的计划内维护](planned-maintenance.md)。

<!-- Update_Description: update meta properties, wording update, update link -->