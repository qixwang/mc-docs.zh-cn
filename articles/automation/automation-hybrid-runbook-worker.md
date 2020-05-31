---
title: Azure 自动化中的混合 Runbook 辅助角色概述
description: 本文概述混合 Runbook 辅助角色，该角色是 Azure 自动化的一项功能，可用于在本地数据中心或云提供商的计算机上运行 Runbook。
services: automation
ms.subservice: process-automation
origin.date: 04/05/2019
ms.date: 05/25/2020
ms.topic: conceptual
ms.openlocfilehash: 07f3626f32b04a305f8fce4b223cfa9dcf96b1da
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801263"
---
# <a name="hybrid-runbook-worker-overview"></a>混合 Runbook 辅助角色概述

Azure 自动化中的 Runbook 可能无权访问其他云或本地环境中的资源，因为它们在 Azure 云平台中运行。 利用 Azure 自动化的混合 Runbook 辅助角色功能，既可以直接在托管角色的计算机上运行 Runbook，也可以对环境中的资源运行 Runbook，从而管理这些本地资源。 Runbook 在 Azure 自动化中进行存储和管理，然后发送到一台或多台指定的计算机。

下图说明了此功能：

![混合 Runbook 辅助角色概述](media/automation-hybrid-runbook-worker/automation.png)

混合 Runbook 辅助角色可以运行 Windows 或 Linux 操作系统。 若要进行监视，需要对受支持的操作系统使用 Azure Monitor 和 Log Analytics 代理。 有关详细信息，请参阅 [Azure Monitor](automation-runbook-execution.md#azure-monitor)。

每个混合 Runbook 辅助角色都是你在安装代理时指定的混合 Runbook 辅助角色组的成员。 一个组可以包含一个代理，但是可以在一个组中安装多个代理，以实现高可用性。 每台计算机可以托管一个向一个自动化帐户报告的混合辅助角色。 

在混合 Runbook 辅助角色中启动 Runbook 时，可以指定该辅助角色会在其中运行的组。 组中的每个辅助角色都会轮询 Azure 自动化以查看是否有可用作业。 如果作业可用，获取作业的第一个辅助角色将执行该作业。 作业队列的处理时间取决于混合辅助角色硬件配置文件和负载。 不能指定特定的辅助角色。 

请使用混合 Runbook 辅助角色而不要使用 [Azure 沙盒](automation-runbook-execution.md#runbook-execution-environment)，因为它不会像沙盒那样对磁盘空间、内存或网络套接字施加许多的[限制](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)。 混合辅助角色的限制仅与该辅助角色自身的资源相关。 

> [!NOTE]
> [公平份额](automation-runbook-execution.md#fair-share)时间限制不会对混合 Runbook 辅助角色造成约束，而 Azure 沙盒则受到这种约束。 

## <a name="hybrid-runbook-worker-installation"></a>混合 Runbook 辅助角色的安装

安装混合 Runbook 辅助角色的过程取决于操作系统。 下表定义了部署类型。

|操作系统  |部署类型  |
|---------|---------|
|Windows     | [自动](automation-windows-hrw-install.md#automated-deployment)<br>[手动](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#install-a-linux-hybrid-runbook-worker)        |

建议的安装方法是使用 Azure 自动化 Runbook 来完全自动执行 Windows 计算机的配置过程。 第二种方法是执行分步过程来手动安装和配置角色。 对于 Linux 计算机，运行 Python 脚本，在计算机上安装代理。

## <a name="network-planning"></a><a name="network-planning"></a>网络规划

要使混合 Runbook 辅助角色连接并注册到 Azure 自动化，必须让其有权访问此部分所述的端口号和 URL。 辅助角色还必须有权访问 [Log Analytics 代理所需的端口和 URL](../azure-monitor/platform/agent-windows.md)，以便能够连接到 Azure Monitor Log Analytics 工作区。

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

混合 Runbook 辅助角色需要以下端口和 URL：

* 端口：出站 Internet 访问只需 TCP 443
* 全局 URL：*.azure-automation.cn
* 代理服务： https://\<workspaceId\>.agentsvc.azure-automation.cn

建议使用在定义[例外](automation-runbook-execution.md#exceptions)时列出的地址。 对于 IP 地址，可以下载 [Azure 数据中心 IP 范围](https://www.microsoft.com/en-us/download/details.aspx?id=42064)。 此文件每周更新，包含当前部署的范围以及即将对 IP 范围进行的更新。

### <a name="proxy-server-use"></a>使用代理服务器

如果使用代理服务器在 Azure 自动化与 Log Analytics 代理之间通信，请确保能够访问相应的资源。 混合 Runbook 辅助角色和自动化服务发出的请求的超时为 30 秒。 尝试三次后，请求将会失败。 

### <a name="firewall-use"></a>使用防火墙

如果使用防火墙来限制对 Internet 的访问，则必须将防火墙配置为允许访问。 如果使用 Log Analytics 网关作为代理，请确保为混合 Runbook 辅助角色配置该网关。 请参阅[为自动化混合辅助角色配置 Log Analytics 网关](/azure-monitor/platform/gateway)。

## <a name="update-management-on-hybrid-runbook-worker"></a>混合 Runbook 辅助角色上的更新管理

启用 Azure 自动化[更新管理](automation-update-management.md)后，连接到 Log Analytics 工作区的任何计算机会自动配置为混合 Runbook 辅助角色。 每个辅助角色能够支持以更新管理为目标的 Runbook。 

以这种方式配置的计算机不会注册到任何已在自动化帐户中定义的混合 Runbook 辅助角色组。 可将计算机添加到混合 Runbook 辅助角色组，但必须对更新管理和混合 Runbook 辅助角色组成员身份使用同一帐户。 此功能已添加到 7.2.12024.0 版本的混合 Runbook 辅助角色。

### <a name="update-management-addresses-for-hybrid-runbook-worker"></a>混合 Runbook 辅助角色的更新管理地址

在混合 Runbook 辅助角色所需的标准地址和端口的顶层，更新管理需要下表中的地址。 与这些地址的通信使用端口 443。

|Azure 中国云  |
|---------|---------|
|*.ods.opinsights.azure.cn     |
|*.oms.opinsights.azure.cn     |
|*.blob.core.chinacloudapi.cn |

## <a name="state-configuration-dsc-on-hybrid-runbook-worker"></a>混合 Runbook 辅助角色上的 State Configuration (DSC)

可以在混合 Runbook 辅助角色上运行 [State Configuration (DSC)](automation-dsc-overview.md) 功能。 若要管理支持混合 Runbook 辅助角色的服务器的配置，必须将这些服务器添加为 DSC 节点。 有关加入的详细信息，请参阅[加入由 State Configuration (DSC) 管理的计算机](automation-dsc-onboarding.md)。

## <a name="runbooks-on-a-hybrid-runbook-worker"></a>混合 Runbook 辅助角色上的 Runbook

可以使用 Runbook 来管理本地计算机上的资源，或者针对部署混合 Runbook 辅助角色的本地环境中的资源运行 Runbook。 在这种情况下，可以选择在混合辅助角色而不是自动化帐户中运行 Runbook。 在混合 Runbook 辅助角色上运行的 Runbook 的结构，与在自动化帐户中运行的 Runbook 结构相同。 请参阅[在混合 Runbook 辅助角色中运行 Runbook](automation-hrw-run-runbooks.md)。

### <a name="hybrid-runbook-worker-jobs"></a>混合 Runbook 辅助角色作业

混合 Runbook 辅助角色作业在 Windows 上的本地 **System** 帐户下运行，或者在 Linux 上的 [nxautomation 帐户](automation-runbook-execution.md#log-analytics-agent-for-linux)下运行。 Azure 自动化处理混合 Runbook 辅助角色上的作业的方式，与处理 Azure 沙盒中运行的作业的方式稍有不同。 请参阅 [Runbook 执行环境](automation-runbook-execution.md#runbook-execution-environment)。

如果混合 Runbook 辅助角色主机重新启动，则任何正在运行的 Runbook 作业将从头重启，或者从 PowerShell 工作流 Runbook 的最后一个检查点重启。 Runbook 作业重启 3 次以上后将会暂停。

### <a name="runbook-permissions-for-a-hybrid-runbook-worker"></a>混合 Runbook 辅助角色的 Runbook 权限

由于混合 Runbook 辅助角色上运行的 Runbook 访问非 Azure 资源，因此无法使用通常由 Runbook 对 Azure 资源进行身份验证时所用的身份验证机制。 Runbook 可以针对本地资源提供其自身的身份验证，或者配置使用 [Azure 资源托管标识](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager)的身份验证。 你还可以指定运行方式帐户，为所有 Runbook 提供用户上下文。

## <a name="next-steps"></a>后续步骤

* 若要了解如何配置 Runbook，使本地数据中心或其他云环境中的过程自动化，请参阅[在混合 Runbook 辅助角色上运行 Runbook](automation-hrw-run-runbooks.md)。
* 若要了解如何对混合 Runbook 辅助角色进行故障排除，请参阅[混合 Runbook 辅助角色的故障排除](troubleshoot/hybrid-runbook-worker.md#general)。
