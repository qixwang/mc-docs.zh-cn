---
title: Azure 服务标记概述
titlesuffix: Azure Virtual Network
description: 了解服务标记。 服务标记有助于尽量降低创建安全规则时的复杂性。
services: virtual-network
documentationcenter: na
author: rockboyfor
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 10/22/2019
ms.date: 02/24/2020
ms.author: v-yeche
ms.reviewer: kumud
ms.openlocfilehash: af2a82d2aac0fbe8d1bd320ad19a7ca3db1a8319
ms.sourcegitcommit: f06e1486873cc993c111056283d04e25d05e324f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2020
ms.locfileid: "77653486"
---
# <a name="virtual-network-service-tags"></a>虚拟网络服务标记 
<a name="network-service-tags"></a>

服务标记代表给定 Azure 服务中的一组 IP 地址前缀。 Azure 会管理服务标记包含的地址前缀，并会在地址发生更改时自动更新服务标记，最大限度地降低了频繁更新网络安全规则的复杂性。 

可以在 [网络安全组](/virtual-network/security-overview#security-rules) 或  [Azure 防火墙](/firewall/service-tags)中使用服务标记来定义网络访问控制。 创建安全规则时，请使用服务标记代替特定的 IP 地址。 在规则的相应源或目标字段中指定服务标记名称（例如  **ApiManagement**），可以允许或拒绝相应服务的流量。 **   **   

可以使用服务标记来实现网络隔离，并防止在访问具有公共终结点的 Azure 服务时通过常规 Internet 访问你的 Azure 资源。 创建入站/出站网络安全组规则，以拒绝进出 **Internet** 的流量，并允许进出 **AzureChinaCloud** 或特定 Azure 服务的其他[可用服务标记](#available-service-tags)的流量。 

## <a name="available-service-tags"></a>可用的服务标记
下表列出了可在[网络安全组](/virtual-network/security-overview#security-rules)规则中使用的所有服务标记。

列指示标记是否：

- 适用于涵盖入站或出站流量的规则。
- 支持[区域](https://status.azure.com/status/)范围。
- 可在 [Azure 防火墙](/firewall/service-tags)规则中使用。

默认情况下，服务标记反映整个云的范围。 某些服务标记还可以通过将相应 IP 范围限制为指定的区域，来实现更精细的控制。 例如，服务标记 Storage 代表整个云的 Azure 存储，而 Storage.ChinaNorth 将范围局限于 ChinaNorth 区域中的存储 IP 地址范围   。 下表指示每个服务标记是否支持此区域范围。  

| 标记 | 目的 | 可以使用入站还是出站连接？ | 可以支持区域范围？ | 是否可在 Azure 防火墙中使用？ |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ApiManagement** | Azure API 管理的管理流量 - 专用部署。 <br/><br/>*注意：* 此标记表示每个区域的控制平面的 Azure API 管理服务终结点。 这使得客户可以对在 API 管理服务中配置的 API、操作、策略和 NamedValues 执行管理操作。  | 入站 | 是 | 是 |
| **AppService** | Azure 应用服务。 建议将此标记用于 Web 应用前端的出站安全规则。 | 出站 | 是 | 是 |
| **AppServiceManagement** | 应用服务环境专用部署的管理流量。 | 推送、请求和匿名 | 否 | 是 |
| **AzureActiveDirectory** | Azure Active Directory。 | 出站 | 否 | 是 |
| **AzureActiveDirectoryDomainServices** | Azure Active Directory 域服务专用部署的管理流量。 | 推送、请求和匿名 | 否 | 是 |
| **AzureBackup** |Azure 备份。<br/><br/>*注意：* 此标记依赖于**存储** 和 **AzureActiveDirectory** 标记。 | 出站 | 否 | 是 |
| **AzureBotService** | Azure 机器人服务。 | 出站 | 否 | 否 |
| **AzureCloud** | 所有[数据中心公共 IP 地址](https://www.microsoft.com/download/confirmation.aspx?id=57062)。 | 出站 | 是 | 是 |
| **AzureConnectors** | 用于探测/后端连接的 Azure 逻辑应用连接器。 | 入站 | 是 | 是 |
| **AzureContainerRegistry** | Azure 容器注册表。 | 出站 | 是 | 是 |
| **AzureCosmosDB** | Azure Cosmos DB。 | 出站 | 是 | 是 |
| **AzureDataExplorerManagement** | Azure 数据资源管理器管理。 | 入站 | 否 | 否 |
| **AzureFrontDoor** | Azure Front Door。 | 推送、请求和匿名 | 否 | 否 |
| **AzureIoTHub** | Azure IoT 中心。 | 出站 | 否 | 否 |
| **AzureKeyVault** | Azure Key Vault。<br/><br/>*注意：* 此标记依赖于 **AzureActiveDirectory** 标记。 | 出站 | 是 | 是 |
| **AzureLoadBalancer** | Azure 基础结构负载均衡器。 此标记将转换为[主机的虚拟 IP 地址](security-overview.md#azure-platform-considerations) (168.63.129.16)，Azure 的运行状况探测源于该 IP。 这不包括到 Azure 负载均衡器资源的流量。 如果不使用 Azure 负载均衡器，则可替代此规则。 | 推送、请求和匿名 | 否 | 否 |
| **AzureMonitor** | Log Analytics、Application Insights、AzMon 和自定义指标（GiG 终结点）。<br/><br/>*注意：* 对于 Log Analytics，此标记依赖于 Storage 标记  。 | 出站 | 否 | 是 |
| **AzurePlatformDNS** | 基本基础结构（默认）DNS 服务。<br/><br />可以使用此标记来禁用默认 DNS。 使用此标记时要格外小心。 建议你阅读 [Azure 平台注意事项](/virtual-network/security-overview#azure-platform-considerations)。 我们还建议你在使用此标记之前执行测试。 | 出站 | 否 | 否 |
| **AzurePlatformIMDS** | Azure 实例元数据服务 (IMDS)，它是一个基本基础结构服务。<br/><br/>可以使用此标记来禁用默认 IMDS。 使用此标记时要格外小心。 建议你阅读 [Azure 平台注意事项](/virtual-network/security-overview#azure-platform-considerations)。 我们还建议你在使用此标记之前执行测试。 | 出站 | 否 | 否 |
| **AzurePlatformLKM** | Windows 授权或密钥管理服务。<br/><br/>可以使用此标记来禁用授权默认值。 使用此标记时要格外小心。 建议你阅读 [Azure 平台注意事项](/virtual-network/security-overview#azure-platform-considerations)。  我们还建议你在使用此标记之前执行测试。 | 出站 | 否 | 否 |
| **AzureTrafficManager** | Azure 流量管理器探测 IP 地址。<br/><br/>有关流量管理器探测 IP 地址的详细信息，请参阅 [Azure 流量管理器常见问题解答](/traffic-manager/traffic-manager-faqs)。 | 入站 | 否 | 是 |  
| **BatchNodeManagement** | Azure Batch 专用部署的管理流量。 | 推送、请求和匿名 | 否 | 是 |
| **CognitiveServicesManagement** | Azure 认知服务的流量的地址范围。 | 出站 | 否 | 否 |
| **Dynamics365ForMarketingEmail** | Dynamics 365 的营销电子邮件服务的地址范围。 | 出站 | 是 | 否 |
| **EventHub** | Azure 事件中心。 | 出站 | 是 | 是 |
| **GatewayManager** | Azure VPN 网关和应用程序网关专用部署的管理流量。 | 入站 | 否 | 否 |
| **HDInsight** | Azure HDInsight。 | 入站 | 是 | 否 |
| **Internet** | 虚拟网络外部的 IP 地址空间，可以通过公共 Internet 进行访问。<br/><br/>此地址范围包括 [Azure 拥有的公共 IP 地址空间](https://www.microsoft.com/download/confirmation.aspx?id=57062)。 | 推送、请求和匿名 | 否 | 否 |
| **MicrosoftContainerRegistry** | Azure 容器映像的容器注册表。 <br/><br/>*注意：* 此外，请将以下 IP 列入白名单（不久将删除此依赖项）：204.79.197.219。 | 出站 | 是 | 是 |
| **服务总线** | 使用高级服务层级的 Azure 服务总线流量。 | 出站 | 是 | 是 |
| **ServiceFabric** | Azure Service Fabric。<br/><br/>*注意：* 此标记表示每个区域的控制平面的 Service Fabric 服务终结点。 这使得客户能够从其 VNET（例如终结点： https:// chinanorth.servicefabric.azure.com）对其 Service Fabric 群集执行管理操作。 | 推送、请求和匿名 | 否 | 否 |
| **Sql** | Azure SQL 数据库、Azure Database for MySQL、Azure Database for PostgreSQL和 Azure SQL 数据仓库。<br/><br/>*注意：* 此标记代表服务，而不是服务的特定实例。 例如，标记可表示 Azure SQL 数据库服务，但不能表示特定的 SQL 数据库或服务器。 | 出站 | 是 | 是 |
| **SqlManagement** | SQL 专用部署的管理流量。 | 推送、请求和匿名 | 否 | 是 |
| **存储** | Azure 存储。 <br/><br/>*注意：* 此标记代表服务，而不是服务的特定实例。 例如，标记可表示 Azure 存储服务，但不能表示特定的 Azure 存储帐户。 | 出站 | 是 | 是 |
| **VirtualNetwork** | 虚拟网络地址空间（为虚拟网络定义的所有 IP 地址范围）、所有连接的本地地址空间、[对等互连](virtual-network-peering-overview.md)的虚拟网络、已连接到[虚拟网络网关](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fvirtual-network%3ftoc.json)的虚拟网络、[主机的虚拟 IP 地址](security-overview.md#azure-platform-considerations)以及在[用户定义的路由](virtual-networks-udr-overview.md)上使用的地址前缀。 此标记还可能包含默认路由。 | 推送、请求和匿名 | 否 | 否 |

<!--Not Available on Line 42 for ApplicationInsightsAvailability -->
<!--MOONCAKE: CORRECT ON Line 48 AzureCloud-->
<!--Not Available on Line 45+1 for AzureAdvancedThreatProtection -->
<!--Not Available on Line 45+1 for AzureCognitiveSearch -->
<!--Not Available on Line 46+1 for AzureDataLake -->
<!--Not Available on Line 46+1 for AzureMachineLearning -->
<!--Not Available on Line 46+1 for AzureResourceManager-->
<!--Not Available on Line 46+1 for AzureSiteRecovery-->
<!--Not Available on Line 49+1 for GuestAndHybridManagement-->
<!--Not Available on Line 49+2 for -->
<!--Not Available on Line 49+3 for -->

> [!NOTE]
> 在经典部署模型中（Azure 资源管理器之前），支持上表中列出的标记的子集。 这些标记的拼写不同：
>
> | 经典拼写方式 | 等效的资源管理器标记 |
> |---|---|
> | AZURE_LOADBALANCER | AzureLoadBalancer |
> | INTERNET | Internet |
> | VIRTUAL_NETWORK | VirtualNetwork |

> [!NOTE]
> Azure 服务的服务标记表示来自所使用的特定云的地址前缀。 例如，与 Azure 公有云上的 Sql 标记值对应的基础 IP 范围将不同于 Azure 中国云上的基础范围  。

> [!NOTE]
> 如果为某个服务（例如 Azure 存储或 Azure SQL 数据库）实现了[虚拟网络服务终结点](virtual-network-service-endpoints-overview.md)，Azure 会将[路由](virtual-networks-udr-overview.md#optional-default-routes)添加到该服务的虚拟网络子网。 路由中的地址前缀与相应服务标记的地址前缀或 CIDR 范围相同。

## <a name="service-tags-on-premises"></a>本地服务标记  
可以获取当前服务标记和范围信息，并将其包含为本地防火墙配置的一部分。 此信息是对应于每个服务标记的 IP 范围的最新列表（截止目前）。 可以通过编程方式或通过 JSON 文件下载获取信息，如以下各节所述。

### <a name="use-the-service-tag-discovery-api-public-preview"></a>使用服务标记发现 API（公共预览版）
可以编程方式检索最新的服务标记列表和 IP 地址范围详细信息：

- [REST](https://docs.microsoft.com/rest/api/virtualnetwork/servicetags/list)
- [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/Get-AzNetworkServiceTag?view=azps-2.8.0&viewFallbackFrom=azps-2.3.2)
- [Azure CLI](https://docs.azure.cn/cli/network?view=azure-cli-latest#az-network-list-service-tags)

> [!NOTE]
> 在公共预览版中，发现 API 返回的信息可能不如 JSON 下载所返回的信息新。 （请参阅下一节。）

### <a name="discover-service-tags-by-using-downloadable-json-files"></a>使用可下载的 JSON 文件发现服务标记 
可以下载包含最新服务标记列表和 IP 地址范围详细信息的 JSON 文件。 这些列表每周更新和发布。 每个云的位置如下：

<!--MOONCAKE: CUSTOMIZED ON THE URL-->

- [Azure 公有云](https://www.microsoft.com/download/confirmation.aspx?id=56519)
- [Azure 美国政府版](https://www.microsoft.com/download/confirmation.aspx?id=57063)  
- [Azure 中国](https://www.microsoft.com/download/confirmation.aspx?id=57062) 
- [Azure 德国](https://www.microsoft.com/download/confirmation.aspx?id=57064)   

<!--MOONCAKE: CUSTOMIZED ON THE URL-->

> [!NOTE]
>在这些信息中，有一部分已在 [Azure 公有云](https://www.microsoft.com/download/confirmation.aspx?id=41653)、[Azure 中国云](https://www.microsoft.com/download/confirmation.aspx?id=42064)和 [Azure 德国云](https://www.microsoft.com/download/details.aspx?id=54770)的 XML 文件中发布。 这些 XML 下载内容将在 2020 年 6 月 30 日弃用，在该日期过后将不再提供。 应按照前面几节中所述，使用发现 API 或 JSON 文件下载进行迁移。

<!--MOONCAKE: CUSTOMIZED ON THE URL 41653, 42064, 54770-->

### <a name="tips"></a>提示 
- 可以通过增大 JSON 文件中的 changeNumber  值，检测各个发布的更新。 每个子节（例如 Storage.ChinaNorth  ）都包含自身的 changeNumber，发生更改后，该编号会递增  。 当任意子节发生更改时，文件的顶级 changeNumber 将会递增  。
- 有关如何分析服务标记信息的示例（例如，获取 ChinaNorth 中的存储的所有地址范围），请参阅[服务标记发现 API PowerShell](https://aka.ms/discoveryapi_powershell) 文档。

## <a name="next-steps"></a>后续步骤
- 了解如何[创建网络安全组](tutorial-filter-network-traffic.md)。

<!-- Update_Description: update meta properties, wording update, update link -->