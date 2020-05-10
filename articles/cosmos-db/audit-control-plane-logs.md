---
title: 如何审核 Azure Cosmos DB 控制平面操作
description: 了解如何在 Azure Cosmos DB 中审核控制平面操作，例如添加区域、更新吞吐量、区域故障转移、添加 VNet，等等
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/16/2020
ms.date: 05/06/2020
ms.author: v-yeche
ms.openlocfilehash: cec9fb931826664e783d69b627ae180d1221cd52
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83002100"
---
# <a name="how-to-audit-azure-cosmos-db-control-plane-operations"></a>如何审核 Azure Cosmos DB 控制平面操作

控制平面操作包括对 Azure Cosmos 帐户或容器的更改。 例如，创建 Azure Cosmos 帐户、添加区域、更新吞吐量、区域故障转移、添加 VNet 等操作都属于控制平面操作。 本文介绍如何在 Azure Cosmos DB 中审核控制平面操作。

## <a name="disable-key-based-metadata-write-access"></a>禁用基于密钥的元数据写入访问

在 Azure Cosmos DB 中审核控制平面操作之前，请在帐户中禁用基于密钥的元数据写入访问。 禁用基于密钥的元数据写入访问后，会阻止通过帐户密钥连接到 Azure Cosmos 帐户的客户端访问该帐户。 可以通过将 `disableKeyBasedMetadataWriteAccess` 属性设置为 true 来禁用写入访问。 设置此属性后，只有拥有适当的、基于角色的访问控制 (RBAC) 角色和凭据的用户才能对任一资源进行更改。 若要详细了解如何设置此属性，请参阅[阻止从 SDK 进行更改](role-based-access-control.md#preventing-changes-from-cosmos-sdk)一文。

 禁用元数据写入访问时，请注意以下几点：

* 进行评估，并确保应用程序不会使用 SDK 或帐户密钥发出更改上述资源（例如，创建集合、更新吞吐量等）的元数据调用。

* 目前，Azure 门户会对元数据操作使用帐户密钥，因此这些操作将被阻止。 请使用 Azure CLI、SDK 或资源管理器模板部署来执行此类操作。

## <a name="enable-diagnostic-logs-for-control-plane-operations"></a>对控制平面操作启用诊断日志

可以使用 Azure 门户对控制平面操作启用诊断日志。 使用以下步骤对控制平面操作启用日志记录：

1. 登录到 [Azure 门户](https://portal.azure.cn)，导航到你的 Azure Cosmos 帐户。

1. 打开“诊断设置”窗格，为要创建的日志提供名称。  

1. 选择“ControlPlaneRequests”作为日志类型，然后选择“发送到 Log Analytics”选项。  

还可以将日志存储在存储帐户中，或将其流式传输到事件中心。 本文介绍如何将日志发送到 Log Analytics，然后查询这些日志。 启用诊断日志后，要使诊断日志生效，需要几分钟的时间。 可以跟踪在该时间点之后执行的所有控制平面操作。 以下屏幕截图显示如何启用控制平面日志：

![启用控制平面请求日志记录](./media/audit-control-plane-logs/enable-control-plane-requests-logs.png)

## <a name="view-the-control-plane-operations"></a>查看控制平面操作

启用日志记录后，使用以下步骤跟踪针对特定帐户的操作：

1. 登录到 [Azure 门户](https://portal.azure.cn)。
1. 在左侧导航栏中打开“监视”选项卡，然后选择“日志”窗格。   此时会打开一个 UI，可在其中轻松针对该特定帐户按范围运行查询。 运行以下查询以查看控制平面日志：

    ```kusto
    AzureDiagnostics
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="ControlPlaneRequests"
    | where TimeGenerated >= ago(1h)
    ```

以下屏幕截图捕获了在将 VNET 添加到 Azure Cosmos 帐户时的日志：

![添加 VNet 时的控制平面日志](./media/audit-control-plane-logs/add-ip-filter-logs.png)

以下屏幕截图捕获了更新 Cassandra 表的吞吐量时的日志：

![更新吞吐量时的控制平面日志](./media/audit-control-plane-logs/throughput-update-logs.png)

## <a name="identify-the-identity-associated-to-a-specific-operation"></a>识别与特定操作关联的标识

若要进一步进行调试，可以使用活动 ID 或者按照操作的时间戳，来识别“活动日志”中的特定操作。  时间戳用于某些未显式传递活动 ID 的资源管理器客户端。 “活动日志”提供有关用于启动操作的标识的详细信息。 以下屏幕截图显示如何使用活动 ID，以及如何在“活动日志”中查找与该 ID 关联的操作：

![使用活动 ID 和查找操作](./media/audit-control-plane-logs/find-operations-with-activity-id.png)

## <a name="next-steps"></a>后续步骤

<!--Not Available on [Explore Azure Monitor for Azure Cosmos DB](../azure-monitor/insights/cosmosdb-insights-overview.md?toc=/cosmos-db/toc.json&bc=/azure/cosmos-db/breadcrumb/toc.json)-->

* [使用 Azure Cosmos DB 中的指标进行监视和调试](use-metrics.md)

<!-- Update_Description: new article about audit control plane logs -->
<!--NEW.date: 04/27/2020-->

