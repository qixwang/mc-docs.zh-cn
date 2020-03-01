---
title: 网络安全组的诊断日志记录
titlesuffix: Azure Virtual Network
description: 了解如何为 Azure 网络安全组启用事件和规则计数器诊断日志。
services: virtual-network
documentationcenter: na
author: rockboyfor
manager: digimobile
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 06/04/2018
ms.date: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: 084ba81c5e9d1a5526acb3d7e62044f6355f63f3
ms.sourcegitcommit: f06e1486873cc993c111056283d04e25d05e324f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2020
ms.locfileid: "77653478"
---
<!--NOT SUITABLE TO AZURE CHINA CLOUD-->
<!--UPDATE CAREFULLY-->
# <a name="diagnostic-logging-for-a-network-security-group"></a>网络安全组的诊断日志记录

网络安全组 (NSG) 包含的规则可以用来允许或拒绝发往虚拟网络子网和/或网络接口的流量。 为 NSG 启用诊断日志记录时，可以记录以下类别的信息：

* **事件：** 根据 MAC 地址记录的与应用到 VM 的 NSG 规则相对应的条目。
* **规则计数器：** 包含应用每个 NSG 规则以拒绝或允许流量的次数的条目。 每隔 60 秒收集一次这些规则的状态。

诊断日志仅适用于通过 Azure 资源管理器部署模型部署的 NSG。 对于通过经典部署模型部署的 NSG，不能启用诊断日志记录。 若要更好地了解这两种模型，请参阅[了解 Azure 部署模型](../resource-manager-deployment-model.md?toc=%2fvirtual-network%2ftoc.json)。

分别为要收集其诊断数据的  每个 NSG 启用诊断日志记录。 如果感兴趣的是操作日志或活动日志，请参阅 Azure [活动日志记录](../azure-monitor/platform/activity-logs-overview.md?toc=%2fvirtual-network%2ftoc.json)。

## <a name="enable-logging"></a>启用日志记录

可以使用 [PowerShell](#powershell) 或 [Azure CLI](#azure-cli) 来启用诊断日志记录。

<!--Not Available on [Azure Portal](#azure-portal)-->

<!--Not Available on ### Azure Portal -->
<!--No Dialognose setting in the portal-->

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

可以通过从计算机运行 PowerShell 来运行命令。  如果在计算机上运行 PowerShell，需要 Azure PowerShell 模块 1.0.0 或更高版本。 在计算机上运行 `Get-Module -ListAvailable Az`，找到已安装的版本。 如果需要进行升级，请参阅 [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps)（安装 Azure PowerShell 模块）。 如果在本地运行 PowerShell，则还需要运行 `Connect-AzAccount -Environment AzureChinaCloud`，以使用具有[所需权限](virtual-network-network-interface.md#permissions)的帐户登录到 Azure。

<!--Not Available on [Azure Cloud Shell](https://shell.azure.com/powershell)-->
<!--Not Available on The Azure Cloud Shell is a free interactive shell. It has common Azure tools preinstalled and configured to use with your account.-->

若要启用诊断日志记录，需要现有 NSG 的 ID。 如果没有现成的 NSG，则可使用 [New-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup) 创建一个。

使用 [Get-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/get-aznetworksecuritygroup) 检索要为其启用诊断日志记录的网络安全组。 例如，若要在名为 *myResourceGroup* 的资源组中检索现有的名为 *myNsg* 的 NSG，请输入以下命令：

```powershell
$Nsg=Get-AzNetworkSecurityGroup `
  -Name myNsg `
  -ResourceGroupName myResourceGroup
```

可以将诊断日志写入三种目标类型。 有关详细信息，请参阅[日志目标](#log-destinations)。 例如，在本文中，日志发送到 *Log Analytics* 目标。 使用 [Get-AzOperationalInsightsWorkspace](https://docs.microsoft.com/powershell/module/az.operationalinsights/get-azoperationalinsightsworkspace) 检索现有的 Log Analytics 工作区。 例如，若要在名为 *myWorkspaces* 的资源组中检索名为 *myWorkspace* 的现有工作区，请输入以下命令：

```powershell
$Oms=Get-AzOperationalInsightsWorkspace `
  -ResourceGroupName myWorkspaces `
  -Name myWorkspace
```

如果没有现成的工作区，则可使用 [New-AzOperationalInsightsWorkspace](https://docs.microsoft.com/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace) 创建一个。

可以为日志启用两种类别的日志记录。 有关详细信息，请参阅[日志类别](#log-categories)。 使用 [Set-AzDiagnosticSetting](https://docs.microsoft.com/powershell/module/az.monitor/set-azdiagnosticsetting) 为 NSG 启用诊断日志记录。 以下示例使用 NSG 的 ID 和以前检索的工作区将事件和计数器类别的数据记录到 NSG 的工作区：

```powershell
Set-AzDiagnosticSetting `
  -ResourceId $Nsg.Id `
  -WorkspaceId $Oms.ResourceId `
  -Enabled $true
```

若要记录其中一个类别的数据而不是两个类别都记录，请将 `-Categories` 选项添加到前一命令，后跟 *NetworkSecurityGroupEvent* 或 *NetworkSecurityGroupRuleCounter*。 若要记录到 Log Analytics 工作区之外的[目标](#log-destinations)，请使用适合 Azure [存储帐户](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fvirtual-network%2ftoc.json)或[事件中心](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fvirtual-network%2ftoc.json)的参数。

<!--Not Available on  or [Event Hub](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fvirtual-network%2ftoc.json)-->

查看和分析日志。 有关详细信息，请参阅[查看和分析日志](#view-and-analyze-logs)。

### <a name="azure-cli"></a>Azure CLI

可以通过从计算机运行 Azure CLI 来运行命令。  如果在计算机上运行 CLI，需要版本 2.0.38 或更高版本。 在计算机上运行 `az --version`，找到已安装的版本。 如果需要进行升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。 如果在本地运行 CLI，则还需要运行 `az login`，以使用具有[所需权限](virtual-network-network-interface.md#permissions)的帐户登录到 Azure。

<!--Not Available on [Azure Cloud Shell](https://shell.azure.com/powershell)-->
<!--Not Available on The Azure Cloud Shell is a free interactive shell. It has common Azure tools preinstalled and configured to use with your account.-->

若要启用诊断日志记录，需要现有 NSG 的 ID。 如果没有现成的 NSG，则可使用 [az network nsg create](https://docs.azure.cn/cli/network/nsg?view=azure-cli-latest#az-network-nsg-create) 创建一个。

使用 [az network nsg show](https://docs.azure.cn/cli/network/nsg?view=azure-cli-latest#az-network-nsg-show) 检索要为其启用诊断日志记录的网络安全组。 例如，若要在名为 *myResourceGroup* 的资源组中检索现有的名为 *myNsg* 的 NSG，请输入以下命令：

```azurecli
nsgId=$(az network nsg show \
  --name myNsg \
  --resource-group myResourceGroup \
  --query id \
  --output tsv)
```

可以将诊断日志写入三种目标类型。 有关详细信息，请参阅[日志目标](#log-destinations)。 例如，在本文中，日志发送到 *Log Analytics* 目标。 有关详细信息，请参阅[日志类别](#log-categories)。

使用 [az monitor diagnostic-settings create](https://docs.azure.cn/cli/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create) 为 NSG 启用诊断日志记录。 以下示例使用前面检索到的 NSG 的 ID 将事件和计数器类别数据记录到名为 *myWorkspace* 的现有工作区，该工作区存在于名为 *myWorkspaces* 的资源组中：

<!--MOONCAKE: ALL THE SECTION OF Json format will be enclosed with ' and " -->

```azurecli
az monitor diagnostic-settings create \
  --name myNsgDiagnostics \
  --resource $nsgId \
  --logs "[ { 'category': 'NetworkSecurityGroupEvent', 'enabled': 'true', 'retentionPolicy': { 'days': '30', 'enabled': 'true' } }, { 'category': 'NetworkSecurityGroupRuleCounter', 'enabled': 'true', 'retentionPolicy': { 'days': '30', 'enabled': 'true' } } ]" \
  --workspace myWorkspace \
  --resource-group myWorkspaces
```

<!--MOONCAKE: ALL THE SECTION OF Json format will be enclosed with ' and " -->

如果没有现成的工作区，则可以使用 [Azure 门户](../azure-monitor/learn/quick-create-workspace.md?toc=%2fvirtual-network%2ftoc.json)或 [PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace) 创建一个。 可以为日志启用两种类别的日志记录。

如果只想记录一种类别或另一种类别的数据，请在上一个命令中删除不想记录其数据的类别。 若要记录到 Log Analytics 工作区之外的[目标](#log-destinations)，请使用适合 Azure [存储帐户](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fvirtual-network%2ftoc.json)或[事件中心](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fvirtual-network%2ftoc.json)的参数。

<!--Not Available on or [Event Hub](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fvirtual-network%2ftoc.json)-->

查看和分析日志。 有关详细信息，请参阅[查看和分析日志](#view-and-analyze-logs)。

## <a name="log-destinations"></a>日志目标

诊断数据可以：
- [写入到 Azure 存储帐户](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fvirtual-network%2ftoc.json)，以便进行审核或手动检查。 可以使用“资源诊断设置”指定保留时间（天）。

<!--Not Available on - [Streamed to an Event hub](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fvirtual-network%2ftoc.json)-->
<!--Not Available on - [Written to Azure Monitor logs](../azure-monitor/platform/resource-logs-collect-storage.md?toc=%2fvirtual-network%2ftoc.json)-->

## <a name="log-categories"></a>日志类别

将为以下日志类别写入 JSON 格式的数据：

### <a name="event"></a>事件

此事件日志记录了根据 MAC 地址将哪些 NSG 规则应用于 VM。 对每个事件记录以下数据。 在以下示例中，对 IP 地址为 192.168.1.4 和 MAC 地址为 00-0D-3A-92-6A-7C 的虚拟机记录了数据：

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY-RULE-NAME]",
        "direction":"[DIRECTION-SPECIFIED-IN-RULE]",
        "priority":"[PRIORITY-SPECIFIED-IN-RULE]",
        "type":"[ALLOW-OR-DENY-AS-SPECIFIED-IN-RULE]",
        "conditions":{
            "protocols":"[PROTOCOLS-SPECIFIED-IN-RULE]",
            "destinationPortRange":"[PORT-RANGE-SPECIFIED-IN-RULE]",
            "sourcePortRange":"[PORT-RANGE-SPECIFIED-IN-RULE]",
            "sourceIP":"[SOURCE-IP-OR-RANGE-SPECIFIED-IN-RULE]",
            "destinationIP":"[DESTINATION-IP-OR-RANGE-SPECIFIED-IN-RULE]"
            }
        }
}
```

### <a name="rule-counter"></a>规则计数器

此规则计数器日志记录了应用到资源的每个规则。 每次应用规则时会记录以下示例数据。 在以下示例中，对 IP 地址为 192.168.1.4 和 MAC 地址为 00-0D-3A-92-6A-7C 的虚拟机记录了数据：

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY-RULE-NAME]",
        "direction":"[DIRECTION-SPECIFIED-IN-RULE]",
        "type":"[ALLOW-OR-DENY-AS-SPECIFIED-IN-RULE]",
        "matchedConnections":125
        }
}
```

> [!NOTE]
> 通信的源 IP 地址不记录。 但是，可以为 NSG 启用 [NSG 流日志记录](../network-watcher/network-watcher-nsg-flow-logging-portal.md)，以便记录所有规则计数器信息以及启动通信的源 IP 地址。 NSG 流日志数据写入 Azure 存储帐户。 可以使用 Azure 网络观察程序的[流量分析](../network-watcher/traffic-analytics.md)功能来分析数据。

## <a name="view-and-analyze-logs"></a>查看和分析日志

若要了解如何查看诊断日志数据，请参阅 [Azure 诊断日志概述](../azure-monitor/platform/resource-logs-overview.md?toc=%2fvirtual-network%2ftoc.json)。 如果将诊断数据发送到：

- **Azure Monitor 日志**：则可使用[网络安全组分析](../azure-monitor/insights/azure-networking-analytics.md?toc=%2fvirtual-network%2ftoc.json#azure-network-security-group-analytics-solution-in-azure-monitor
)解决方案来获取增强的见解。 此解决方案提供 NSG 规则的可视化效果，此类规则可以根据 MAC 地址允许或拒绝虚拟机中网络接口的流量。
- **Azure 存储帐户**，则将数据写入 PT1H.json 文件。 可以找到：
    - 事件日志，位于以下路径：`insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`
    - 规则计数器日志，位于以下路径：`insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`

## <a name="next-steps"></a>后续步骤

- 详细了解[活动日志记录](../azure-monitor/platform/resource-logs-overview.md?toc=%2fvirtual-network%2ftoc.json)（以前称为审核或操作日志）。 默认情况下，对通过任一 Azure 部署模型创建的 NSG 启用活动日志记录。 若要在活动日志中确定完成了哪些 NSG 相关操作，请查看含有以下资源类型的条目：
  - Microsoft.ClassicNetwork/networkSecurityGroups
  - Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
  - Microsoft.Network/networkSecurityGroups
  - Microsoft.Network/networkSecurityGroups/securityRules
- 若要了解如何记录诊断信息，使日志包含每个流的源 IP 地址，请参阅 [NSG 流日志记录](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fvirtual-network%2ftoc.json)。

<!--NOT SUITABLE TO AZURE CHINA CLOUD-->
<!--UPDATE CAREFULLY-->

<!-- Update_Description: new article about virtual network nsg manage log -->
<!--ms.date: 10/21/2019-->