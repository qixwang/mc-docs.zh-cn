---
title: 限制 Azure Kubernetes 服务 (AKS) 中的出口流量
description: 了解控制 Azure Kubernetes Service (AKS) 中的出口流量所需的端口和地址
services: container-service
ms.topic: article
origin.date: 03/10/2020
ms.date: 05/25/2020
ms.author: v-yeche
ms.openlocfilehash: 8c514de6b9638f187c994869362c23b40f2df1c8
ms.sourcegitcommit: 7e6b94bbaeaddb854beed616aaeba6584b9316d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2020
ms.locfileid: "83735529"
---
<!--Verified successfully with 21AzureChina Description-->
# <a name="control-egress-traffic-for-cluster-nodes-in-azure-kubernetes-service-aks"></a>控制 Azure Kubernetes 服务 (AKS) 中群集节点的出口流量

默认情况下，AKS 群集具有不受限制的出站（出口）Internet 访问权限。 此级别的网络访问权限允许运行的节点和服务根据需要访问外部资源。 如果希望限制出口流量，则必须限制可访问的端口和地址数量，才能维护正常的群集维护任务。 默认情况下，群集被配置为仅使用 Microsoft 容器注册表 (MCR) 或 Azure 容器注册表 (ACR) 中的基本系统容器映像。 配置首选的防火墙和安全规则，以允许所需的端口和地址。

<!--CORRECT ON Microsoft Container Registry (MCR)-->

本文详细介绍了在 AKS 群集中限制出口流量时，所必需的和可选的网络端口和完全限定的域名 (FQDN)。

> [!IMPORTANT]
> 本文档仅介绍如何锁定离开 AKS 子网的流量。 AKS 没有入口需求。  不支持使用网络安全组 (NSG) 和防火墙阻止内部子网流量。 若要控制和阻止群集内的流量，请使用[网络策略][network-policy]。

## <a name="before-you-begin"></a>准备阶段

需要安装和配置 Azure CLI 版本 2.0.66 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI][install-azure-cli]。

## <a name="egress-traffic-overview"></a>出口流量概述

为了便于管理和操作，AKS 群集中的节点需要访问特定的端口和完全限定的域名 (FQDN)。 这些操作可能是与 API 服务器进行通信，或者是下载并安装核心 Kubernetes 群集组件和节点安全更新。 默认情况下，AKS 群集中的节点不会限制出口（出站）Internet 流量。 群集可以从外部存储库中拉取基本系统容器映像。

若要提高 AKS 群集的安全性，建议限制出口流量。 群集配置为从 MCR 或 ACR 拉取基本系统容器映像。 如果以这种方式锁定出口流量，请定义特定端口和 FQDN，以允许 AKS 节点与所需的外部服务正确通信。 如果没有这些授权端口和 FQDN，则 AKS 节点无法与 API 服务器通信或无法安装核心组件。

可以使用 [Azure 防火墙][azure-firewall]或第三方防火墙设备来保护出口流量，并定义这些必需的端口和地址。 AKS 不会自动为你创建这些规则。 在网络防火墙中创建适当的规则时，可参考以下端口和地址。

> [!IMPORTANT]
> 使用 Azure 防火墙限制出口流量并创建用户定义的路由 (UDR) 来强制所有出口流量时，请确保在防火墙中创建适当的 DNAT 规则，以正确允许入口流量。 结合使用 Azure 防火墙和 UDR 时，会因为路由不对称而中断入口设置。 （如果 AKS 子网具有指向防火墙专用 IP 地址的默认路由，但你使用的是公共负载均衡器 - 类型为 LoadBalancer 的入口或 Kubernetes 服务，则会出现此问题）。 在这种情况下，将通过负载均衡器的公共 IP 地址接收传入的负载均衡器流量，但返回路径将通过防火墙的专用 IP 地址。 由于防火墙是有状态的，并且无法识别已建立的会话，因此会丢弃返回的数据包。 若要了解如何将 Azure 防火墙与入口或服务负载均衡器集成，请参阅[将 Azure 防火墙与 Azure 标准负载均衡器集成](/firewall/integrate-lb)。
> 可以使用出口工作器节点 IP 和 API 服务器 IP 之间的网络规则锁定 TCP 端口 9000、TCP 端口 22 和 UDP 端口 1194 的流量。

在 AKS 中，有两组端口和地址：

* [AKS 群集的所需端口和地址](#required-ports-and-addresses-for-aks-clusters)详细说明了授权出口流量的最低要求。
* [AKS 群集的可选推荐地址和端口](#optional-recommended-addresses-and-ports-for-aks-clusters)并非针对所有方案都是必需的，但是与其他服务（如 Azure Monitor）的集成将不会正常运行。 查看可选端口和 FQDN 列表，并授权 AKS 群集中使用的任何服务和组件。

> [!NOTE]
> 限制出口流量仅适用于新的 AKS 群集。 对于现有群集，在限制出口流量之前，请使用 `az aks upgrade` 命令[执行群集升级操作][aks-upgrade]。

## <a name="required-ports-and-addresses-for-aks-clusters"></a>AKS 群集的必需端口和地址

以下出站端口/网络规则对于 AKS 群集是必需的：

* TCP 端口 443
* 如果有应用需要与 API 服务器通信，则需要 TCP [IPAddrOfYourAPIServer]:443。  创建群集后，可以设置此更改。
* TCP 端口 9000、TCP 端口 22 和 UDP 端口 1194，使隧道前端 pod 与 API 服务器上的隧道后端进行通信  。
    * 若要获取更多具体信息，请参阅下表中的 *.hcp.\<location\>.cx.prod.service.azk8s.cn 和 *.tun.\<location\>.cx.prod.service.azk8s.cn 地址 。
* UDP 端口 123，用于网络时间协议 (NTP) 时间同步（Linux 节点）。
* 如果你有可直接访问 API 服务器的 pod，则还必须具有用于 DNS 的 UDP 端口 53。

必须具有以下 FQDN/应用程序规则：

> [!IMPORTANT]
> *.blob.core.chinacloudapi.cn 和 aksrepos.azurecr.cn 不再需要 FQDN 规则来进行出口锁定。  对于现有群集，请使用 `az aks upgrade` 命令[执行群集升级操作][aks-upgrade]，删除这些规则。

> [!IMPORTANT]
> Azure 公有云区域已从 *.cdn.mscr.io 替换为 *.data.mcr.microsoft.com。 请升级现有防火墙规则，以使更改生效。

<!--Not Available on - Azure Global-->

- Azure 中国世纪互联

| FQDN                       | 端口      | 用途      |
|----------------------------|-----------|----------|
| *.hcp.\<location\>.cx.prod.service.azk8s.cn | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Node <-> API 服务器通信需要此地址。 将 \<location\> 替换为部署 AKS 群集的区域。 |
| *.tun.\<location\>.cx.prod.service.azk8s.cn | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Node <-> API 服务器通信需要此地址。 将 \<location\> 替换为部署 AKS 群集的区域。 |
| *.azk8s.cn        | HTTPS:443 | 下载必需的二进制文件和映像需要此地址|
| mcr.microsoft.com          | HTTPS:443 | 访问 Microsoft 容器注册表 (MCR) 中的映像需要此地址。 此注册表包含在升级和缩放群集期间群集正常运行所需的第一方映像/图表（例如 Moby 等） |
| *.cdn.mscr.io       | HTTPS:443 | Azure 内容分发网络 (CDN) 支持的 MCR 存储需要此地址。 |
| *.data.mcr.microsoft.com             | HTTPS:443 | Azure 内容分发网络 (CDN) 支持的 MCR 存储需要此地址。 |
| management.chinacloudapi.cn       | HTTPS:443 | Kubernetes GET/PUT 操作需要此地址。 |
| login.chinacloudapi.cn  | HTTPS:443 | Azure Active Directory 身份验证需要此地址。 |
| ntp.ubuntu.com             | UDP:123   | Linux 节点上的 NTP 时间同步需要此地址。 |
| packages.microsoft.com     | HTTPS:443 | Microsoft 包存储库使用此地址缓存 apt-get 操作。  示例包包括 Moby、PowerShell 和 Azure CLI。 |

<!--CORRECT ON MICROSOFT-->

<!--Not Available on - Azure Government -->

## <a name="optional-recommended-addresses-and-ports-for-aks-clusters"></a>AKS 群集的可选推荐地址和端口

以下出站端口/网络规则对于 AKS 群集是可选的：

为了使 AKS 群集正常运行，建议使用以下 FQDN/应用程序规则：

| FQDN                                    | 端口      | 用途      |
|-----------------------------------------|-----------|----------|
| security.ubuntu.com、azure.archive.ubuntu.com、changelogs.ubuntu.com | HTTP:80   | 此地址允许 Linux 群集节点下载必需的安全修补程序和更新。 |

## <a name="required-addresses-and-ports-for-gpu-enabled-aks-clusters"></a>启用 GPU 的 AKS 群集必需的地址和端口

启用 GPU 的 AKS 群集需要以下 FQDN/应用程序规则：

| FQDN                                    | 端口      | 用途      |
|-----------------------------------------|-----------|----------|
| nvidia.github.io | HTTPS:443 | 此地址用于在基于 GPU 的节点上进行正确的驱动程序安装和操作。 |
| us.download.nvidia.com | HTTPS:443 | 此地址用于在基于 GPU 的节点上进行正确的驱动程序安装和操作。 |
| apt.dockerproject.org | HTTPS:443 | 此地址用于在基于 GPU 的节点上进行正确的驱动程序安装和操作。 |

## <a name="required-addresses-and-ports-with-azure-monitor-for-containers-enabled"></a>启用用于容器的 Azure Monitor 的必需地址和端口

启用了用于容器的 Azure Monitor 的 AKS 群集需要以下 FQDN/应用程序规则：

| FQDN                                    | 端口      | 用途      |
|-----------------------------------------|-----------|----------|
| dc.services.visualstudio.com | HTTPS:443    | 这适用于使用 Azure Monitor 的正确指标和监视遥测。 |
| *.ods.opinsights.azure.com    | HTTPS:443    | Azure Monitor 使用此地址来引入日志分析数据。 |
| *.oms.opinsights.azure.com | HTTPS:443 | 此地址由 omsagent 使用，用于对日志分析服务进行身份验证。 |
|*.microsoftonline.com | HTTPS:443 | 此地址用于验证指标并将其发送到 Azure Monitor。 |
|*.monitoring.azure.com | HTTPS:443 | 此地址用于将指标数据发送到 Azure Monitor。 |

## <a name="required-addresses-and-ports-with-azure-dev-spaces-enabled"></a>启用 Azure Dev Spaces 所需的地址和端口

启用了 Azure Dev Spaces 的 AKS 群集需要以下 FQDN/应用程序规则：

| FQDN                                    | 端口      | 用途      |
|-----------------------------------------|-----------|----------|
| cloudflare.docker.com | HTTPS:443 | 此地址用于请求 linux alpine 和其他 Azure Dev Spaces 映像 |
| gcr.azk8s.cn | HTTP:443 | 此地址用于请求 helm/tiller 映像 |
| storage.googleapis.com | HTTP:443 | 此地址用于请求 helm/tiller 映像 |
| azds-\<guid\>.\<location\>.azds.io | HTTPS:443 | 与控制器的 Azure Dev Spaces 后端服务进行通信。 可以在 %USERPROFILE%\.azds\settings.json 的“dataplaneFqdn”中找到准确的 FQDN |

## <a name="required-addresses-and-ports-for-aks-clusters-with-azure-policy-in-public-preview-enabled"></a>启用了 Azure Policy（公共预览版）的 AKS 群集所需的地址和端口

> [!CAUTION]
> 下面的某些功能处于预览阶段。  随着此功能向公共预览版和未来发布阶段发展，本文中的建议可能会有所不同。

启用了 Azure Policy 的 AKS 群集需要以下 FQDN/应用程序规则。

<!--CORRECT ON gov-prod-policy-data.trafficmanager.net ON FOLLOWING ITEM-->

| FQDN                                    | 端口      | 用途      |
|-----------------------------------------|-----------|----------|
| gov-prod-policy-data.trafficmanager.net | HTTPS:443 | 此地址用于正确操作 Azure Policy。 （AKS 中目前为预览版） |
| raw.githubusercontent.com | HTTPS:443 | 此地址用于从 GitHub 请求内置策略，以确保正确操作 Azure Policy。 （AKS 中目前为预览版） |
| *.gk.\<location\>.cx.prod.service.azk8s.cn | HTTPS:443    | Azure Policy 加载项，与在主服务器上运行的 Gatekeeper 审核终结点进行通信以获取审核结果。 |
| dc.services.visualstudio.com | HTTPS:443 | Azure Policy 加载项，用于向应用程序见解终结点发送遥测数据。 |

## <a name="required-by-windows-server-based-nodes-enabled"></a>启用基于 Windows Server 的节点需要此地址

使用基于 Windows Server 的节点池需要以下 FQDN/应用程序规则：

<!--CORRECT ON winlayers.blob.core.windows.net ON FOLLOWING ITEM-->

| FQDN                                    | 端口      | 用途      |
|-----------------------------------------|-----------|----------|
| onegetcdn.azureedge.net, winlayers.blob.core.windows.net, winlayers.cdn.mscr.io, go.microsoft.com | HTTPS:443 | 安装与 windows 相关的二进制文件 |
| mp.microsoft.com, www<span></span>.msftconnecttest.com, ctldl.windowsupdate.com | HTTP:80 | 安装与 windows 相关的二进制文件 |
| kms.core.chinacloudapi.cn | TCP:1688 | 安装与 windows 相关的二进制文件 |

## <a name="next-steps"></a>后续步骤

通过学习这篇文章，你了解了在限制群集的出口流量时可以使用的端口和地址。 你还可以定义 pod 本身可以进行通信的方式，以及它们在群集中的限制。 有关详细信息，请参阅[在 AKS 中使用网络策略保护 Pod 之间的流量][network-policy]。

<!-- LINKS - internal -->

[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest
[network-policy]: use-network-policies.md
[azure-firewall]: ../firewall/overview.md
[az-feature-register]: https://docs.azure.cn/cli/feature?view=azure-cli-latest#az-feature-register
[az-feature-list]: https://docs.azure.cn/cli/feature?view=azure-cli-latest#az-feature-list
[az-provider-register]: https://docs.azure.cn/cli/provider?view=azure-cli-latest#az-provider-register
[aks-upgrade]: upgrade-cluster.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md

<!-- Update_Description: update meta properties, wording update, update link -->