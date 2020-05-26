---
title: ASDK 发行说明
description: Azure Stack 开发工具包 (ASDK) 的改进、修复和已知问题。
author: WenJason
ms.topic: article
origin.date: 04/06/2020
ms.date: 05/18/2020
ms.author: v-jay
ms.reviewer: misainat
ms.lastreviewed: 03/18/2020
ms.openlocfilehash: ab902e11063afc5698b84f8daa2a2d1df2247af0
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83422566"
---
# <a name="asdk-release-notes"></a>ASDK 发行说明

本文介绍了 Azure Stack 开发工具包 (ASDK) 中的更改、修复和已知问题。 如果不确定所运行的版本，请[使用门户进行查看](../operator/azure-stack-updates.md)。

请订阅 [![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS 源](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#)，随时了解 ASDK 的新增功能。

::: moniker range="azs-2002"
## <a name="build-12002035"></a>内部版本 1.2002.0.35

### <a name="new-features"></a>新增功能

- 有关此版本中已修复问题、更改和新功能的列表，请参阅 [Azure Stack 发行说明](../operator/release-notes.md)中的相关章节。

### <a name="fixed-and-known-issues"></a>修复的和已知的问题

- 解密认证密码是一种新选项，用于指定包含解密备份数据所需的私钥的自签名证书 (.pfx) 的密码。 只有在使用证书加密备份时，此密码才是必需的。
- 有关此发布中 Azure Stack 已知问题的列表，请参阅[已知问题](../operator/known-issues.md)一文。
- 请注意，可用的 Azure Stack 修补程序不适用于 ASDK。

#### <a name="sql-vm-provision-fails-in-asdk"></a>ASDK 中的 SQL VM 预配失败

- 适用于：此问题适用于 ASDK 2002。
- 原因：在 ASDK 2002 中创建新 SQL VM 时，可能会收到一条错误消息“扩展存储库中找不到发布者为‘Microsoft.SqlServer.Management’，类型为‘SqlIaaSAgent’，并且类型处理程序版本为‘2.0’的扩展。” 在 Azure Stack Hub 中没有 SqlIaaSAgent 2.0。
::: moniker-end

::: moniker range="azs-1910"
## <a name="build-11910058"></a>内部版本 1.1910.0.58

### <a name="new-features"></a>新增功能

- 有关此版本中已修复问题、更改和新功能的列表，请参阅 [Azure Stack 发行说明](../operator/release-notes.md)中的相关章节。

### <a name="fixed-and-known-issues"></a>修复的和已知的问题

- 修复了收集日志并将其存储在 Azure 存储 Blob 容器中时遇到的问题。 此操作的语法如下所示：

  ```powershell
  Get-AzureStackLog -OutputSasUri "<Blob service SAS Uri>"
  ``` 

- 修复了一个部署问题：缓慢加载后台处理程序服务会阻止删除某些 Windows 功能，并需要重新启动。
- 有关此发布中 Azure Stack 已知问题的列表，请参阅[已知问题](../operator/known-issues.md)一文。
- 请注意，可用的 Azure Stack 修补程序不适用于 ASDK。
::: moniker-end

::: moniker range="azs-1908"
  
## <a name="build-11908020"></a>内部版本 1.1908.0.20

### <a name="new-features"></a>新增功能

- 如需此版本中新功能的列表，请参阅 Azure Stack 发行说明的[此部分](/azure-stack/operator/release-notes?view=azs-1908#whats-new-2)。

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>修复的和已知的问题

<!-- - For a list of Azure Stack issues fixed in this release, see [this section](/azure-stack/operator/release-notes?view=azs-1908#fixes-1) of the Azure Stack release notes. -->
- 如需已知问题的列表，请参阅[此文](/azure-stack/operator/known-issues?view=azs-1908)。
- 请注意，可用的 Azure Stack 修补程序不适用于 ASDK。
::: moniker-end

::: moniker range="azs-1907"
## <a name="build-11907020"></a>内部版本 1.1907.0.20

### <a name="new-features"></a>新增功能

- 如需此版本中新功能的列表，请参阅 Azure Stack 发行说明的[此部分](/azure-stack/operator/release-notes?view=azs-1907#whats-in-this-update)。

<!-- ### Changes -->

### <a name="fixed-and-known-issues"></a>修复的和已知的问题

- 使用某些市场映像创建 VM 资源时，可能无法完成部署。 作为一种解决方法，可以单击“摘要”页中的“下载模板和参数”链接，然后单击“模板”边栏选项卡中的“部署”按钮。
- 如需此版本中已修复的 Azure Stack 问题的列表，请参阅 Azure Stack 发行说明的[此部分](/azure-stack/operator/release-notes?view=azs-1907#fixes-3)。
- 如需已知问题的列表，请参阅[此文](/azure-stack/operator/known-issues?view=azs-1907)。
- 请注意，[发布的 Azure Stack 修补程序](/azure-stack/operator/release-notes?view=azs-1907#hotfixes-3)不适用于 Azure Stack ASDK。
::: moniker-end
