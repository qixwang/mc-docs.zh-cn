---
title: Microsoft Threat Modeling Tool 版本 2018/9/12
titleSuffix: Azure
description: 阐述 Threat Modeling Tool 的发行说明
author: jegeib
ms.author: v-tawe
ms.service: security
ms.subservice: security-develop
ms.topic: article
ms.date: 06/05/2020
origin.date: 01/15/2019
ms.openlocfilehash: a3b27a88322239cefdf76e9271a656dc48d4d6b9
ms.sourcegitcommit: 79c99a9ea013b3c74706a1038a505f4eea2aaac4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2020
ms.locfileid: "84439545"
---
# <a name="threat-modeling-tool-ga-release-71509112---9122018"></a>Threat Modeling Tool 正式版 7.1.50911.2 - 2018 年 9 月 12 日

我们很高兴地宣布，Microsoft Threat Modeling Tool 的受支持正式版 (GA) 现已推出，可供用户下载。 此版本包含重要的隐私和安全更新，以及 bug 修复、功能更新和稳定性方面的改进。 2017 预览版的现有用户在打开客户端时，系统会提示他们通过 ClickOnce 技术更新到最新版本。 对于该工具的新用户，可以[下载客户端](https://aka.ms/threatmodelingtool)。

随着此版本的推出，我们将终止对 2017 预览版的支持，建议预览版的所有用户更新到正式版。 在 2018 年 10 月 15 日或之后，我们将会规定 Threat Modeling Tool 的最低要求 ClickOnce 版本，所有预览版客户端都必须升级。

Microsoft Threat Modeling Tool 2016（可从 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=49168)下载）的支持期将持续到 2019 年 10 月 1 日，但仅限关键安全修复方面的支持。

## <a name="feature-changes"></a>功能更改

### <a name="azure-stencil-updates"></a>Azure 模具更新

此版本随附的模具集添加了更多的 Azure 模具及其关联的威胁和缓解措施。 “Azure 应用服务”、“Azure 数据库产品/服务”和“Azure 存储”的重点功能已有重大变化。

![Azure 模具更新](./media/threat-modeling-tool-releases-71509112/tmt_azure_stencil_update-300x70.png)

<!-- OneDrive not avialable-->

## <a name="notable-fixed-bugs-reported-by-customers"></a>修复了客户报告的明显 bug

### <a name="in-tmt-preview-the-tool-crashes-when-using-the-standard-template"></a>在 TMT 预览版中，使用标准模板时该工具会崩溃

- 将通用模具（例如“通用数据流”）添加到图面并生成威胁时，该工具可能会崩溃。 现在已修复此问题。

### <a name="in-tmt-preview-when-i-save-a-report-or-copy-the-threats-the-risk-levels-are-incorrect"></a>在 TMT 预览版中，当我保存报告或复制威胁时，风险级别不正确

- 如果用户修改特定威胁的风险级别，然后保存报告或复制风险，则风险级别可能会还原为“高”。 现在已修复此问题。

## <a name="known-issues-and-faq"></a>已知问题和常见问题解答

### <a name="users-of-high-resolution-screens-may-experience-small-text-in-the-threat-properties"></a>高分辨率屏幕的用户可能会在威胁属性中看到很小的字体

#### <a name="issue"></a>问题

在该工具的分析视图中，如果用户使用的高分辨率屏幕默认设置为放大画面以方便在 Windows 中阅读，则威胁的“可能的缓解措施”部分可能会显示很小的文本。

![使用高分辨率屏幕时的已知问题](./media/threat-modeling-tool-releases-71509112/tmt_screen_resolution-300x153.png)

#### <a name="workaround"></a>解决方法

用户可以单击缓解措施文本，然后使用标准的 Windows 缩放控件（按住 Crtl 并向上滚动鼠标滚轮）来增大该部分的放大倍数。

<!-- one Dirive is not available-->

### <a name="my-organization-uses-the-2016-version-of-the-tool-can-i-use-the-azure-stencil-set"></a>我的组织使用的是 2016 版工具，我是否可以使用 Azure 模具集？

可以！ [Azure 模具集已在 GitHub 上提供](https://github.com/Microsoft/threat-modeling-templates/)，可将其载入 2016 版工具。 若要使用 Azure 模具集创建新模型，请使用主菜单屏幕上的“新模型的模板”对话框。 TMT 2016 无法呈现 Azure 模具集的“可能的缓解措施”字段中的链接，因此，你可能发现这些链接显示为 HTML 标记。

![2016 客户端中的 Azure 模具更新](./media/threat-modeling-tool-releases-71509112/tmt_azure_stencils-300x212.png)

## <a name="system-requirements"></a>系统要求

- 支持的操作系统
  - Microsoft Windows 10
- 所需的 .NET 版本
  - .NET 3.5.2
- 其他要求
  - 需要建立 Internet 连接才能接收工具和模板的更新。

## <a name="documentation-and-feedback"></a>文档和反馈

- Threat Modeling Tool 的文档位于 [docs.azure.cn](threat-modeling-tool.md)，其中包含[有关如何使用该工具](threat-modeling-tool-getting-started.md)的信息。

## <a name="next-steps"></a>后续步骤

下载最新版本的 [Microsoft Threat Modeling Tool](https://aka.ms/threatmodelingtool)。
