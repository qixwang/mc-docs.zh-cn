---
title: Azure Service Fabric 监视合作伙伴 | Azure
description: 了解如何使用合作伙伴监视解决方案监视 Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 10/16/2018
ms.date: 12/09/2019
ms.author: v-yeche
ms.openlocfilehash: d3fdd6d43bbdec2cdd9686b56e7d193050eefb03
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75336446"
---
# <a name="azure-service-fabric-monitoring-partners"></a>Azure Service Fabric 监视合作伙伴

本文演示如何使用几个合作伙伴解决方案来监视 Service Fabric 应用程序、群集和基础结构。 我们与以下每家合作伙伴协作，以创建适用于 Service Fabric 的集成产品/服务。

## <a name="dynatrace"></a>Dynatrace

与 Dynatrace 的集成可提供许多现成的功能用于监视 Service Fabric 群集。 在 VMSS 实例上安装 Dynatrace OneAgent 可以提供性能计数器，并实现应用级别的 Service Fabric 部署拓扑。 此外，Dynatrace 非常适合用于本地监视。 请查看[通告](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/)和[说明](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/)中所列的，用于在群集上启用 Dynatrace 的其他功能。 

## <a name="datadog"></a>Datadog

Datadog 包含适用于 Windows 和 Linux 实例的 VMSS 扩展。 使用 Datadog 可以收集 Windows 事件日志，因此可以收集 Windows 上的 Service Fabric 平台事件。 请在[此处](https://www.datadoghq.com/blog/azure-monitoring-enhancements/#integrate-with-azure-service-fabric)查看有关如何将诊断数据发送到 Datadog 的说明。

## <a name="appdynamics"></a>AppDynamics

Service Fabric 与 AppDynamics 的集成是在应用程序级别实现的。 通过更新环境变量并使用 App Dynamics NuGet，可将应用程序遥测数据发送到 AppDynamics。 请参阅这些[说明](https://docs.appdynamics.com/display/AZURE/Install+AppDynamics+for+Azure+Service+Fabric)，了解如何将 .NET Service Fabric 应用程序与 AppDynamics 集成。

## <a name="new-relic"></a>New Relic

New Relic 是与 Service Fabric 应用程序完美集成的另一个应用程序性能管理工具。 可以安装 New Relic NuGet 包，并在清单文件中添加特定的环境变量，以便将应用程序遥测数据发送到 New Relic。 请查看这些[说明](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/install-net-agent-azure-service-fabric)，了解如何为 .NET Service Fabric 应用程序启用 New Relic 遥测。

<!-- Pending on ## ELK -->
<!-- Not Available on [here](service-fabric-tutorial-java-elk.md)-->

## <a name="humio"></a>Humio

Humio 是一项日志收集服务，可以通过实时方式在云中或本地收集应用程序中的日志以及 Service Fabric 中的事件。 除了实时可观测性，Humio 还提供现代的分析和可视化功能，用于查看和收集诊断中的见解。 Humio 具有经济有效的定价计划，构建后可以在保持超高速度的同时进行缩放。 它直接集成 Service Fabric 平台事件和应用程序遥测。 可以在[此处](https://github.com/humio/service-fabric-humio)详细了解 Humio 和 Service Fabric 集成。

## <a name="next-steps"></a>后续步骤

* 参阅 Service Fabric 中[监视和诊断的概述](service-fabric-diagnostics-overview.md)

<!--Not Available on [diagnose common scenarios](service-fabric-diagnostics-common-scenarios.md)-->

<!-- Update_Description: update meta properties, wording update, update link -->