---
title: 使用适用于容器的 Azure Monitor 监视 Kubernetes 群集运行状况 | Microsoft Docs
description: 本文介绍如何使用用于容器的 Azure Monitor 查看和分析 AKS 和非 AKS 群集运行状况。
ms.topic: conceptual
author: Johnnytechn
ms.author: v-johya
ms.date: 05/22/2020
ms.openlocfilehash: d45c5e8ddfaba68891aa090d65f3bb98fde8cde6
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84200012"
---
# <a name="understand-kubernetes-cluster-health-with-azure-monitor-for-containers"></a>使用适用于容器的 Azure Monitor 了解 Kubernetes 群集运行状况

使用适用于容器的 Azure Monitor，可以监视和报告托管基础结构组件的运行状况，以及适用于容器的 Azure Monitor 支持的任何 Kubernetes 群集上运行的所有节点的运行状况。 此体验超出了在[多群集视图](container-insights-analyze.md#multi-cluster-view-from-azure-monitor)上计算和报告的群集运行状况的范围；在该视图中，你现在可以根据选定的指标，了解群集中的一个或多个节点是否受资源约束，或者节点或 Pod 是否不可用，从而影响集群中正在运行的应用程序。

若要了解如何启用适用于容器的 Azure Monitor，请参阅[载入适用于容器的 Azure Monitor](container-insights-onboard.md)。

>[!NOTE]
>若要支持 AKS 引擎群集，请验证它是否满足以下要求：
>- 它使用的是最新版本的 [HELM 客户端](https://helm.sh/docs/using_helm/)。
>- 容器化代理版本为“microsoft/oms:ciprod11012019”。 若要升级代理，请参阅[在 Kubernetes 群集上升级代理](container-insights-manage-agent.md)。
>

## <a name="overview"></a>概述

在适用于容器的 Azure Monitor 中，运行状况（预览）功能提供对 Kubernetes 群集的主动运行状况监视，以帮助你识别和诊断问题。 通过它，你可以查看检测到的重要问题。 评估群集运行状况的监视器在群集中的容器化代理上运行，且运行状况数据写入 Log Analytics 工作区中的 KubeHealth表。 

Kubernetes 群集运行状况基于由以下 Kubernetes 对象和抽象组织的许多监视场景：

- Kubernetes 基础结构 - 通过评估 CPU 和内存利用率以及 Pod 可用性，它提供在群集中部署的节点上运行的 Kubernetes API 服务器、ReplicaSet 和 DaemonSet 的汇总

    ![Kubernetes 基础结构运行状况汇总视图](./media/container-insights-health/health-view-kube-infra-01.png)

- 节点 - 通过评估 CPU 和内存利用率以及 Kubernetes 报告的状态，它提供节点池和每个池中各个节点状态的汇总。

    ![节点运行状况汇总视图](./media/container-insights-health/health-view-nodes-01.png)

目前，只支持虚拟 kubelet 的状态。 虚拟 kublet 节点的 CPU 和内存利用率的运行状况状态报告为“未知”，因为未接收到来自它们的任何信号。

所有监视器都显示在“运行状况层次结构”窗格的层次结构布局中，其中表示 Kubernetes 对象或抽象（即 Kubernetes 基础结构或节点）的聚合监视器是反映所有依赖的子监控器的组合运行状况的最顶层监视器。 用于获得运行状况的主要监视场景包括：

* 从节点和容器评估 CPU 利用率。
* 从节点和容器评估内存使用率。
* 根据 Kubernetes 报告的就绪状态计算 Pod 和节点的状态。

用于指示状态的图标如下：

|图标|含义|  
|--------|-----------|  
|![绿色勾号图标表示正常](./media/container-insights-health/healthyicon.png)|成功、运行状况正常（绿色）|  
|![黄色三角形和感叹号表示警告](./media/container-insights-health/warningicon.png)|警告（黄色）|  
|![带有白色 X 的红色按钮表示严重状态](./media/container-insights-health/criticalicon.png)|严重（红色）|  
|![灰显图标](./media/container-insights-health/grayicon.png)|未知（灰色）|  

## <a name="monitor-configuration"></a>监视器配置

若要了解支持适用于容器的 Azure Monitor 运行状况功能的每个监视器的行为和配置，请参阅[运行状况监视器配置指南](container-insights-health-monitors-config.md)。

## <a name="sign-in-to-the-azure-portal"></a>登录到 Azure 门户

登录到 [Azure 门户](https://portal.azure.cn)。 

## <a name="view-health-of-an-aks-or-non-aks-cluster"></a>查看 AKS 或非 AKS 群集的运行状况

通过从 Azure 门户的左窗格中选择“见解”，可以直接从 AKS 群集访问适用于容器的 Azure Monitor 运行状况（预览）功能。 在“见解”部分，选择“容器”。  

若要查看非 AKS 群集（即托管在本地或 Azure 堆栈上的 AKS 引擎群集）的运行状况，请从 Azure 门户的左侧窗格中选择“Azure Monitor”。 在“见解”部分，选择“容器”。   在多群集页面上，从列表中选择相应的非 AKS 群集。

在适用于容器的 Azure Monitor 中，从“群集”页面中，选择“运行状况” 。

![群集运行状况仪表板示例](./media/container-insights-health/container-insights-health-page.png)

## <a name="review-cluster-health"></a>查看群集运行状况

“运行状况”页打开时，在“运行状况特性”网格中默认选择了“Kubernetes 基础结构” 。  网格汇总了 Kubernetes 基础结构和群集节点的当前运行状况汇总状态。 选择任一运行状况特性都会更新“运行状况层次结构”窗格（即中间窗格）中的结果，并在层次结构布局中显示所有子监视器以及其当前运行状况状态。 若要查看有关任何依赖监视器的详细信息，可以选择一个监视器，页面右侧自动显示属性窗格。 

![群集运行状况属性窗格](./media/container-insights-health/health-view-property-pane.png)

在属性窗格中，可了解以下内容：

- 在“概述”选项卡上，它显示所选监视器的当前状态、上次计算监视器的时间以及上次状态更改的时间。 其他信息根据在层次结构中选择的监视器类型进行显示。

    如果你在“运行状况层次结构”窗格中选择一个聚合监视器，则在属性窗格上的“概述”选项卡下，显示层次结构中子监视器总数的汇总，以及处于严重、警告和正常状态的聚合监视器的数量。 

    ![聚合监视器的“运行状况”属性窗格“概述”选项卡](./media/container-insights-health/health-overview-aggregate-monitor.png)

    如果你在“运行状况层次结构”窗格中选择一个单元监视器，它还会在“上次状态更改”下显示容器化代理在过去四小时内计算和报告的上一个样本。 它的基础是用于比较多个连续值以确定其状态的单元监视器计算。 例如，如果你选择了“Pod 就绪状态”单元监视器，则它将显示由参数“ConsecutiveSamplesForStateTransition”控制的最后两个样本 。 有关详细信息，请参阅[单元监视器](container-insights-health-monitors-config.md#unit-monitors)的详细说明。
    
    ![“运行状况”属性窗格的“概述”选项卡](./media/container-insights-health/health-overview-unit-monitor.png)

    如果“上次状态更改”报告的时间是一天或更早，这是监视器状态没有更改的结果。 但是，如果收到的单元监视器的最后一个样本时间超过 4 小时，这可能表明容器代理没有发送数据。 如果代理知道某个特定资源（例如节点）存在，但它没有从该节点的 CPU 或内存利用率监视器（举例）接收到数据，则监视器的运行状况状态将设置为“未知”。  

- 在“配置”选项卡上，它显示默认配置参数设置（仅适用于单元监视器，而非聚合监视器）及其值。
- 在“知识”选项卡上，它包含说明监视器的行为以及如何评估运行不正常状况的信息。

此页上的监视数据不会自动刷新，你需要选择页顶部的“刷新”以查看从群集收到的最新运行状况状态。

## <a name="next-steps"></a>后续步骤

请参阅[日志查询示例](container-insights-log-search.md#search-logs-to-analyze-data)，以查看预定义的查询，以及有关群集警报、可视化或分析的评估或自定义示例。

