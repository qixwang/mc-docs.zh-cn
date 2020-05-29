---
title: 使用复合资源在 Azure 自动化状态配置中撰写 DSC 配置
description: 了解如何在 Azure 自动化状态配置中使用复合资源撰写配置。
keywords: powershell dsc, 所需状态配置, powershell dsc azure, 复合资源
services: automation
ms.subservice: dsc
origin.date: 08/21/2018
ms.date: 05/25/2020
ms.topic: conceptual
ms.openlocfilehash: 1cb81527f70822bd1a594e9d869ed675ca595e0b
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801242"
---
# <a name="composing-dsc-configurations-in-azure-automation-state-configuration-using-composite-resources"></a>使用复合资源在 Azure 自动化状态配置中撰写 DSC 配置

需要通过多个所需状态配置 (DSC) 来管理资源时，最佳方法是使用[复合资源](https://docs.microsoft.com/powershell/scripting/dsc/resources/authoringresourcecomposite)。 复合资源是一个嵌套的参数化配置，用作另一配置中的 DSC 资源。 使用复合资源可以创建复杂的配置，同时可以单独管理和构建基础复合资源。

Azure 自动化支持[导入和撰写复合资源](automation-dsc-compile.md)。 将复合资源导入自动化帐户后，可以通过 Azure 门户中的“状态配置(DSC)”功能使用 Azure 自动化状态配置。

## <a name="composing-a-configuration-from-composite-resources"></a>基于复合资源撰写配置

必须先撰写基于复合资源构建的配置，然后才能在 Azure 门户中分配它。 在“状态配置(DSC)”页面上，可以从“配置”或“已编译配置”选项卡使用“撰写配置”来进行撰写  。

1. 登录到 [Azure 门户](https://portal.azure.cn)。
1. 在左侧，单击“所有资源”，并单击自动化帐户的名称。
1. 在“自动化帐户”页的“配置管理”下选择“State configuration (DSC)”。 
1. 在“状态配置(DSC)”页面上，单击“配置”或“已编译配置”选项卡，然后在页面顶部的菜单中单击“撰写配置”  。
1. 在“基本信息”步骤中，提供新配置的名称（必填），在要包括在新配置中的每个复合资源的行中单击任意位置，然后单击“下一步”或单击“源代码”步骤。  对于以下步骤，我们选择了 `PSExecutionPolicy` 和 `RenameAndDomainJoin` 复合资源。
   ![“撰写配置”页面的基本信息步骤的屏幕截图](./media/compose-configurationwithcompositeresources/compose-configuration-basics.png)
1. “源代码”步骤显示所选复合资源的已撰写配置的样子。 你可以看到所有参数的合并以及它们是如何传递给复合资源的。 复查新的源代码后，单击“下一步”或单击“参数”步骤。 
   ![“撰写配置”页面的源代码步骤的屏幕截图](./media/compose-configurationwithcompositeresources/compose-configuration-sourcecode.png)
1. 在“参数”步骤中，将公开每个复合资源具有的参数，以便可以提供那些值。 如果参数具有说明，则它会显示在参数字段旁边。 如果参数是 `PSCredential` 类型，则下拉列表将提供当前自动化帐户中的“凭据”对象列表。 还可以使用“+ 添加凭据”选项。 提供所有必需的参数后，单击“保存并编译”。
   ![“撰写配置”页面的参数步骤的屏幕截图](./media/compose-configurationwithcompositeresources/compose-configuration-parameters.png)

一旦保存新配置，将会立即提交它进行编译。 可以像任何已导入的配置一样查看编译作业的状态。 有关详细信息，请参阅[查看编译作业](automation-dsc-getting-started.md#view-a-compilation-job)。

当编译成功完成后，新配置会显示在“已编译配置”选项卡中。将配置分配到托管节点之后，可以按照[为节点重新分配其他节点配置](automation-dsc-getting-started.md#reassign-a-node-to-a-different-node-configuration)中的步骤进行操作。

## <a name="next-steps"></a>后续步骤

- 有关入门信息，请参阅 [Azure 自动化 State Configuration 入门](automation-dsc-getting-started.md)。
- 若要了解如何加入节点，请参阅[加入 Azure Automation State Configuration 管理的计算机](automation-dsc-onboarding.md)。
- 若要了解如何编译 DSC 配置，以便将它们分配给目标节点，请参阅[在 Azure 自动化 State Configuration 中编译配置](automation-dsc-compile.md)。
- 有关 PowerShell cmdlet 参考，请参阅 [Azure 自动化 State Configuration cmdlet](https://docs.microsoft.com/powershell/module/azurerm.automation/#automation)。
- 有关定价信息，请参阅 [Azure Automation State Configuration 定价](https://azure.cn/pricing/details/automation/)。
- 若要查看在持续部署管道中使用 Azure Automation State Configuration 的示例，请参阅[使用 Azure Automation State Configuration 和 Chocolatey 进行持续部署](automation-dsc-cd-chocolatey.md)。