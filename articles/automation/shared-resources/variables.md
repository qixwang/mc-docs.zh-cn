---
title: Azure 自动化中的变量资产
description: 变量资产是可供 Azure 自动化中的所有 Runbook 和 DSC 配置使用的值。  本文介绍了变量的详细信息，以及如何在文本和图形创作中使用变量。
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: WenJason
ms.author: v-jay
origin.date: 05/14/2019
ms.date: 03/30/2020
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: 94f546441535305141ee953e855ce30a4450b32a
ms.sourcegitcommit: 90d01d08faf8adb20083363a8e4e5aab139cd9b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80290418"
---
# <a name="variable-assets-in-azure-automation"></a>Azure 自动化中的变量资产

变量资产是可供自动化帐户中的所有 Runbook 和 DSC 配置使用的值。 可以在 Azure 门户、PowerShell、Runbook 内部或 DSC 配置中管理这些资产。

自动化变量可用于以下方案：

- 在多个 Runbook 或 DSC 配置之间共享某个值。

- 在同一 Runbook 或 DSC 配置中的多个作业之间共享某个值。

- 通过门户或 PowerShell 命令行管理 Runbook 或 DSC 配置使用的值。 例如一组常用配置项，包括特定的 VM 名称列表、特定资源组、AD 域名，等等。  

由于自动化变量将会持久保存，即使 Runbook 或 DSC 配置失败，它们也仍然可用。 此行为允许一个 Runbook 或 DSC 配置设置的值随后由另一个 Runbook 使用，或由同一 Runbook 或 DSC 配置在下次运行时使用。

创建变量时，可以指定将其加密，并由 Azure 自动化将其作为安全资产进行存储。 其他安全资产包括凭据、证书和连接。 Azure 自动化将加密这些资产，并使用针对每个自动化帐户生成的唯一密钥来存储这些资产。 该密钥存储在系统托管的 Key Vault 中。 在存储安全资产之前，Azure 自动化会从 Key Vault 加载该密钥，然后用它加密资产。 

Azure 自动化会安全存储每个加密的变量。 无法使用 Azure PowerShell 模块中随附的 [Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) cmdlet 检索该变量的值。 只能使用 Runbook 或 DSC 配置中的 `Get-AutomationVariable` 活动检索加密的值。

## <a name="variable-types"></a>变量类型

使用 Azure 门户创建变量时，必须从下拉列表指定一个数据类型，使门户能够显示用于输入变量值的相应控件。 下面是可在 Azure 自动化中使用的变量类型：

* String
* Integer
* DateTime
* Boolean
* Null

该变量并不局限于指定的数据类型。 如果想要指定不同类型的值，则必须使用 Windows PowerShell 设置该变量。 如果指定“未定义”，则该变量的值将设置为 Null，并且必须使用 [Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0) cmdlet 或 `Set-AutomationVariable` 活动来设置该值。 

无法使用门户创建或更改复杂变量类型的值。 但是，可以使用 Windows PowerShell 提供任何类型的值。 复杂类型将作为 [PSCustomObject](https://docs.microsoft.com/dotnet/api/system.management.automation.pscustomobject) 检索。

可以通过创建一个数组或哈希表并将其保存到变量，来将多个值存储到单个变量。

## <a name="powershell-cmdlets-that-create-and-manage-variable-assets"></a>用于创建和管理变量资产的 PowerShell cmdlet

对于 Az 模块，下表中的 cmdlet 用于通过 Windows PowerShell 创建和管理自动化变量资产。 可在自动化 Runbook 和 DSC 配置中使用的 [Az.Automation 模块](https://docs.microsoft.com/powershell/azure/overview)已随附了这些 cmdlet。

| Cmdlet | 说明 |
|:---|:---|
|[Get-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationvariable?view=azps-3.5.0) | 检索现有变量的值。|
|[New-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationvariable?view=azps-3.5.0) | 创建新变量并设置变量值。|
|[Remove-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationvariable?view=azps-3.5.0)| 删除现有变量。|
|[Set-AzAutomationVariable](https://docs.microsoft.com/powershell/module/az.automation/set-azautomationvariable?view=azps-3.5.0)| 设置现有变量的值。|

## <a name="activities-to-access-variables"></a>用于访问变量的活动

下表中的活动用于访问 Runbook 和 DSC 配置中的变量。 本文开头已解释加密变量的 `Get-AzAutomationVariable` 与 `Get-AutomationVariable` 之间的差异。

| 活动 | 说明 |
|:---|:---|
|`Get-AutomationVariable`|检索现有变量的值。|
|`Set-AutomationVariable`|设置现有变量的值。|

> [!NOTE]
> 避免在 Runbook 或 DSC 配置中的 `Get-AutomationVariable` 的 `Name` 参数中使用变量。 使用此参数可能会使设计时发现 Runbook 或 DSC 配置与自动化变量之间的依赖关系变得复杂。

下表中的函数用于在 Python2 Runbook 中访问和检索变量。

|Python2 函数|说明|
|:---|:---|
|`automationassets.get_automation_variable`|检索现有变量的值。 |
|`automationassets.set_automation_variable`|设置现有变量的值。 |

> [!NOTE]
> 必须在 Python Runbook 顶部导入 `automationassets` 模块才能访问资产函数。

## <a name="creating-a-new-automation-variable"></a>创建新的自动化变量

### <a name="create-a-new-variable-using-the-azure-portal"></a>使用 Azure 门户创建新变量

1. 在自动化帐户中单击“资产”磁贴，然后单击“资产”边栏选项卡并选择“变量”。   
2. 在“变量”  磁贴中，选择“添加变量”  。
3. 填写“新建变量”边栏选项卡上的选项，然后单击“创建”保存新变量。  

>[!NOTE]
>若要删除某个变量的加密，必须删除该变量，然后重新创建未加密的变量。

### <a name="create-a-new-variable-with-windows-powershell"></a>使用 Windows PowerShell 创建新变量

此脚本使用 `New-AzAutomationVariable` cmdlet 创建新变量并设置其初始值。 然后，它可以使用 `Get-AzAutomationVariable` 检索值。 如果该值为简单类型，则检索相同的类型。 如果是复杂类型，则检索 `PSCustomObject` 类型。

以下示例演示如何创建字符串类型的变量，并返回其值。

```powershell
New-AzAutomationVariable -ResourceGroupName "ResourceGroup01" 
-AutomationAccountName "MyAutomationAccount" -Name 'MyStringVariable' `
-Encrypted $false -Value 'My String'
$string = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
-AutomationAccountName "MyAutomationAccount" -Name 'MyStringVariable').Value
```

以下示例演示如何创建复杂类型的变量，并检索其属性。 本例使用了 [Get-AzVM](https://docs.microsoft.com/powershell/module/Az.Compute/Get-AzVM?view=azps-3.5.0) 返回的虚拟机对象。

```powershell
$vm = Get-AzVM -ResourceGroupName "ResourceGroup01" -Name "VM01"
New-AzAutomationVariable -AutomationAccountName "MyAutomationAccount" -Name "MyComplexVariable" -Encrypted $false -Value $vm

$vmValue = (Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
-AutomationAccountName "MyAutomationAccount" -Name "MyComplexVariable").Value
$vmName = $vmValue.Name
$vmIpAddress = $vmValue.IpAddress
```

## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>使用 Runbook 或 DSC 配置中的变量

使用 `Set-AutomationVariable` 活动设置 PowerShell Runbook 或 DSC 配置中自动化变量的值，并使用 `Get-AutomationVariable` 检索该值。 不应在 Runbook 或 DSC 配置中使用 `Set-AzureRMAutomationVariable` 和 `Get-AzureRMAutomationVariable` cmdlet，因为它们的效率低于工作流活动。 

请注意，不能使用 `Get-AzureRmAutomationVariable` cmdlet 检索安全变量的值。 

只能使用 `New-AzureRmAutomationVariable` cmdlet 从 Runbook 或 DSC 配置内部创建新变量。

### <a name="textual-runbook-samples"></a>文本 Runbook 示例

#### <a name="set-and-retrieve-a-simple-value-from-a-variable"></a>设置和检索变量中的简单值

下面的示例命令演示如何设置和检索文本 Runbook 中的变量。 此示例假设要创建名为 `NumberOfIterations` 和 `NumberOfRunnings` 的整数变量，以及名为 `SampleMessage` 的字符串变量。

```powershell
$NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
$NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
$SampleMessage = Get-AutomationVariable -Name 'SampleMessage'

Write-Output "Runbook has been run $NumberOfRunnings times."

for ($i = 1; $i -le $NumberOfIterations; $i++) {
    Write-Output "$i`: $SampleMessage"
}
Set-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -AutomationAccountName "MyAutomationAccount" -Name NumberOfRunnings -Value ($NumberOfRunnings += 1)
```

#### <a name="set-and-retrieve-a-variable-in-a-python2-runbook"></a>设置和检索 Python2 Runbook 中的变量

以下示例演示如何在 Python2 Runbook 中使用变量、设置变量以及处理有关不存在的变量的异常。

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a variable
value = automationassets.get_automation_variable("test-variable")
print value

# set a variable (value can be int/bool/string)
automationassets.set_automation_variable("test-variable", True)
automationassets.set_automation_variable("test-variable", 4)
automationassets.set_automation_variable("test-variable", "test-string")

# handle a non-existent variable exception
try:
    value = automationassets.get_automation_variable("nonexisting variable")
except AutomationAssetNotFound:
    print "variable not found"
```

### <a name="graphical-runbook-samples"></a>图形 Runbook 示例

在图形 Runbook 中，可以添加 `Get-AutomationVariable` 或 `Set-AutomationVariable` 活动。 只需在图形编辑器的“库”窗格中右键单击变量，然后选择所需的活动即可。

![将变量添加到画布](../media/variables/runbook-variable-add-canvas.png)

#### <a name="set-values-in-a-variable"></a>设置变量中的值

下图显示了在图形 Runbook 中用于更新具有简单值的一个变量的示例活动。 在此示例中，`Get-AzureRmVM` 检索单个 Azure 虚拟机，并将计算机名保存到现有的自动化字符串变量中。 [链接是管道还是序列](../automation-graphical-authoring-intro.md#links-and-workflow)并不重要，因为代码只需要输出中的单个对象。

![设置简单变量](../media/variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>后续步骤

- 若要详细了解如何在图形创作中连接活动，请参阅[图形创作中的链接](../automation-graphical-authoring-intro.md#links-and-workflow)。
- 若要开始使用图形 Runbook，请参阅[我的第一个图形 Runbook](../automation-first-runbook-graphical.md)。
