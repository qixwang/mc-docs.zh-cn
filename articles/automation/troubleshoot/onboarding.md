---
title: 对 Azure 自动化管理解决方案的加入进行故障排除
description: 了解如何对 Azure 自动化解决方案加入错误进行故障排除。
services: automation
author: WenJason
ms.author: v-jay
origin.date: 05/22/2019
ms.date: 05/25/2020
ms.topic: conceptual
ms.service: automation
manager: digimobile
ms.openlocfilehash: 4a1534ba8f1b74558edc9d8ad4fdb3ce1216ca30
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801843"
---
# <a name="troubleshoot-solution-onboarding"></a>对解决方案加入进行故障排除

当加入 Azure 自动化更新管理解决方案时，可能会收到错误消息。 本文描述可能会发生的各种错误及其解决方法。

## <a name="known-issues"></a>已知问题

### <a name="scenario-renaming-a-registered-node-requires-unregister-or-register-again"></a><a name="node-rename"></a>场景：重命名已注册的节点需要取消注册或重新注册

#### <a name="issue"></a>问题

将一个节点注册到 Azure 自动化，然后更改操作系统计算机名。 节点的报表继续以原始名称显示。

#### <a name="cause"></a>原因

重命名已注册的节点不会更新 Azure 自动化中的节点名称。

#### <a name="resolution"></a>解决方法

从 Azure Automation State Configuration 中注销该节点，然后重新注册它。 在此之前，发布到服务的报表将不再可用。

### <a name="scenario-re-signing-certificates-via-https-proxy-isnt-supported"></a><a name="resigning-cert"></a>场景：不支持通过 HTTPS 代理重新签名证书

#### <a name="issue"></a>问题

通过终止 HTTPS 流量的代理解决方案进行连接，然后使用新证书重新加密流量时，服务不允许该连接。

#### <a name="cause"></a>原因

Azure 自动化不支持对用于加密流量的证书重新签名。

#### <a name="resolution"></a>解决方法

目前没有针对此问题的解决方法。

## <a name="general-errors"></a>常规错误

### <a name="scenario-onboarding-fails-with-the-message-the-solution-cannot-be-enabled"></a><a name="missing-write-permissions"></a>场景：加入失败并显示消息“无法启用解决方案”

#### <a name="issue"></a>问题

尝试将 VM 加入解决方案时，你会收到以下消息之一：

```error
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

```error
The solution cannot be enabled on this VM because the permission to read the workspace is missing
```

#### <a name="cause"></a>原因

此错误是由于 VM、工作区或用户的权限不正确或丢失造成的。

#### <a name="resolution"></a>解决方法

请确保你拥有[加入计算机所需的权限](../automation-role-based-access-control.md#onboarding-permissions)，然后尝试重新加入解决方案。 如果收到错误消息 `The solution cannot be enabled on this VM because the permission to read the workspace is missing`，请确保你具有 `Microsoft.OperationalInsights/workspaces/read` 权限，能够获知 VM 是否已加入工作区。

### <a name="scenario-onboarding-fails-with-the-message-failed-to-configure-automation-account-for-diagnostic-logging"></a><a name="diagnostic-logging"></a>场景：加入失败，并显示消息“未能为诊断日志记录配置自动化帐户”

#### <a name="issue"></a>问题

此错误代码表示，用于定向解决方案的已保存搜索计算机组查询的格式不正确。 

#### <a name="cause"></a>原因

你可能已经更改了查询，或者系统可能已经更改了查询。

#### <a name="resolution"></a>解决方法

可以删除对此解决方案的查询，然后再次加入解决方案，这会重新创建查询。 可以在你的工作区内找到此查询，它位于“保存的搜索”下。 查询名称是“MicrosoftDefaultComputerGroup”，查询类别是关联解决方案的名称。 如果启用了多个解决方案，则“MicrosoftDefaultComputerGroup”查询会在“保存的搜索”下显示多次 。

### <a name="scenario-policyviolation"></a><a name="policy-violation"></a>场景：PolicyViolation

#### <a name="issue"></a>问题

此错误代码表示部署由于违反一个或多个策略而失败。

#### <a name="cause"></a>原因 

策略正在阻止操作完成。

#### <a name="resolution"></a>解决方法

若要成功部署解决方案，必须考虑更改指示的策略。 由于可以定义许多不同类型的策略，需要进行的更改取决于所违反的策略。 例如，如果在资源组上定义了一个策略，该策略拒绝更改所包含资源的内容，则可以选择以下修复方法之一：

* 将该策略完全删除。
* 尝试将解决方案加入不同的资源组。
* 将策略重定向到特定资源，例如，自动化帐户。
* 修改该策略已配置为拒绝的资源集。

检查 Azure 门户右上角的通知，或转到包含自动化帐户的资源组，然后选择“设置”下的“部署”以查看失败的部署 。 若要详细了解 Azure Policy，请参阅 [Azure Policy 概述](../../governance/policy/overview.md?toc=%2fautomation%2ftoc.json)。

### <a name="scenario-errors-trying-to-unlink-a-workspace"></a><a name="unlink"></a>场景：尝试取消链接工作区时遇到错误

#### <a name="issue"></a>问题

尝试取消链接工作区时收到以下错误消息：

```error
The link cannot be updated or deleted because it is linked to Update Management Solutions.
```

#### <a name="cause"></a>原因

当你的 Log Analytics 工作区中仍有活动的解决方案（取决于链接的自动化帐户和 Log Analytics 工作区）时，会发生此错误。

### <a name="resolution"></a>解决方法

如果正在使用以下解决方案，请将其从工作区中删除：

* 更新管理

删除解决方案后，可以取消链接工作区。 从工作区和自动化帐户中清理这些解决方案中的任何现有项目非常重要：

* 对于更新管理，请从自动化帐户中删除“更新部署(计划)”。

## <a name="log-analytics-for-windows-extension-failures"></a><a name="mma-extension-failures"></a>Windows 扩展的 Log Analytics 故障

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

安装适用于 Windows 扩展的 Log Analytics 代理可能会因各种原因而失败。 以下部分描述在部署 Windows 扩展的 Log Analytics 代理期间可能导致失败的加入问题。

>[!NOTE]
>适用于 Windows 的 Log Analytics 代理是 Microsoft Monitoring Agent (MMA) 当前在 Azure 自动化中使用的名称。

### <a name="scenario-an-exception-occurred-during-a-webclient-request"></a><a name="webclient-exception"></a>场景：在 WebClient 请求期间发生异常

VM 上适用于 Windows 扩展的 Log Analytics 无法与外部资源通信，并且部署失败。

#### <a name="issue"></a>问题

下面是返回的错误消息示例：

```error
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```error
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>原因

此错误的部分潜在原因包括：

* VM 中配置的代理只允许使用特定端口。
* 某个防火墙设置已阻止访问所需的端口和地址。

#### <a name="resolution"></a>解决方法

确保已打开正确的端口和地址用于通信。 有关端口和地址的列表，请参阅[规划网络](../automation-hybrid-runbook-worker.md#network-planning)。

### <a name="scenario-install-failed-because-of-transient-environment-issues"></a><a name="transient-environment-issue"></a>场景：暂时性的环境问题导致安装失败

在部署期间安装适用于 Windows 扩展的 Log Analytics 失败，因为另一项安装或操作阻止了此项安装。

#### <a name="issue"></a>问题

下面是可能返回的错误消息示例：

```error
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>原因

此错误的部分潜在原因包括：

* 另一项安装正在进行。
* 在部署模板期间，已触发系统重新启动。

#### <a name="resolution"></a>解决方法

此错误本质上是暂时性的。 请重试部署，以安装该扩展。

### <a name="scenario-installation-timeout"></a><a name="installation-timeout"></a>场景：安装超时

由于超时，适用于 Windows 扩展的 Log Analytics 安装未完成。

#### <a name="issue"></a>问题

下面是可能返回的错误消息示例：

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>原因

发生这种类型的错误是因为在安装期间 VM 负载过重。

### <a name="resolution"></a>解决方法

在 VM 负载较低时，尝试安装安装适用于 Windows 扩展的 Log Analytics 代理。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请尝试通过以下渠道之一获取更多支持：

* 通过 [Azure 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home?forum=windowsazurezhchs)获取 Azure 专家的解答。
* 提出 Azure 支持事件。 请转到 [Azure 支持站点](https://support.azure.cn/zh-cn/support/contact/)以获取支持。
