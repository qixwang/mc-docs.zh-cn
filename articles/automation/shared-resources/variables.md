---
title: 管理 Azure 自动化中的变量
description: 变量资产是可供 Azure 自动化中的所有 Runbook 和 DSC 配置使用的值。  本文介绍了变量的详细信息，以及如何在文本和图形创作中使用变量。
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: WenJason
ms.author: v-jay
origin.date: 05/14/2019
ms.date: 05/18/2020
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: c06daf3c2fa2351d7686fdaf08a22de549ff78e6
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801289"
---
# <a name="manage-variables-in-azure-automation"></a>管理 Azure 自动化中的变量

变量资产是可供自动化帐户中的所有 Runbook 和 DSC 配置使用的值。 可以在 Azure 门户、PowerShell、Runbook 内部或 DSC 配置中管理这些资产。

自动化变量可用于以下方案：

- 在多个 Runbook 或 DSC 配置之间共享某个值。

- 在同一 Runbook 或 DSC 配置中的多个作业之间共享某个值。

- 通过门户或 PowerShell 命令行管理 Runbook 或 DSC 配置使用的值。 例如一组常用配置项，包括特定的 VM 名称列表、特定资源组、AD 域名，等等。  

Azure 自动化会持久保存变量，因此即使 runbook 或 DSC 配置失败，变量也仍然可用。 此行为允许一个 Runbook 或 DSC 配置设置的值随后由另一个 Runbook 使用，或由同一 Runbook 或 DSC 配置在下次运行时使用。

Azure 自动化会安全存储每个加密的变量。 创建变量时，可以指定将其加密，并由 Azure 自动化将其作为安全资产进行存储。 

>[!NOTE]
>Azure 自动化中的安全资产包括凭据、证书、连接和加密的变量。 这些资产已使用针对每个自动化帐户生成的唯一密钥加密并存储在 Azure 自动化中。 Azure 自动化将密钥存储在系统托管的 Key Vault 中。 在存储安全资产之前，自动化会从 Key Vault 加载该密钥，然后用它加密资产。 

## <a name="variable-types"></a>变量类型

使用 Azure 门户创建变量时，必须从下拉列表指定一个数据类型，使门户能够显示用于输入变量值的相应控件。 下面是可在 Azure 自动化中使用的变量类型：

* String
* Integer
* DateTime
* Boolean
* Null

该变量并不局限于指定的数据类型。 如果想要指定不同类型的值，则必须使用 Windows PowerShell 设置该变量。 如果指定 `Not defined`，则变量的值将设置为 Null。 必须使用 [Set-AzureRmAutomationVariable](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationvariable?view=azurermps-6.13.0) cmdlet 或内部 `Set-AutomationVariable` cmdlet 来设置值。

无法使用 Azure 门户创建或更改复杂变量类型的值。 但是，可以使用 Windows PowerShell 提供任何类型的值。 复杂类型将作为 [PSCustomObject](https://docs.microsoft.com/dotnet/api/system.management.automation.pscustomobject) 检索。

可以通过创建一个数组或哈希表并将其保存到变量，来将多个值存储到单一变量。

>[!NOTE]
>VM 名称变量最多可以包含 80 个字符。 资源组变量最多可以包含 90 个字符。 请参阅 [Azure 资源的命名规则和限制](/azure-resource-manager/management/resource-name-rules)。

## <a name="powershell-cmdlets-to-access-variables"></a>用于访问变量的 PowerShell cmdlet

下表中的 cmdlet 通过 PowerShell 创建和管理自动化变量。

| Cmdlet | 说明 |
|:---|:---|
|[Get-AzureRmAutomationVariable](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationvariable?view=azurermps-6.13.0) | 检索现有变量的值。 如果该值为简单类型，则检索相同的类型。 如果是复杂类型，则将检索 `PSCustomObject` 类型。 <br>**注意：** 不能使用此 cmdlet 来检索已加密变量的值。 只能在 Runbook 或 DSC 配置中使用内部 `Get-AutomationVariable` cmdlet 来执行此操作。 请参阅[用于访问变量的内部 cmdlet](#internal-cmdlets-to-access-variables)。 |
|[New-AzureRmAutomationVariable](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationvariable?view=azurermps-6.13.0) | 创建新变量并设置变量值。|
|[Remove-AzureRmAutomationVariable](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationvariable?view=azurermps-6.13.0)| 删除现有变量。|
|[Set-AzureRmAutomationVariable](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationvariable?view=azurermps-6.13.0)| 设置现有变量的值。 |

## <a name="internal-cmdlets-to-access-variables"></a>用于访问变量的内部 cmdlet

下表中的内部 cmdlet 用于访问 Runbook 和 DSC 配置中的变量。 这些 cmdlet 附带全局模块 `Orchestrator.AssetManagement.Cmdlets`。

| 内部 Cmdlet | 说明 |
|:---|:---|
|`Get-AutomationVariable`|检索现有变量的值。|
|`Set-AutomationVariable`|设置现有变量的值。|

> [!NOTE]
> 避免在 Runbook 或 DSC 配置中的 `Get-AutomationVariable` 的 `Name` 参数中使用变量。 使用变量可能会使设计时发现 Runbook 与自动化变量之间的依赖关系变得复杂。

`Get-AutomationVariable` 在 PowerShell 中不起作用，只能在 Runbook 或 DSC 配置中使用。 例如，若要查看某个已加密变量的值，可以创建一个 runbook 来获取该变量，然后将其写入到输出流：
 
```powershell
$mytestencryptvar = Get-AutomationVariable -Name TestVariable
Write-output "The encrypted value of the variable is: $mytestencryptvar"
```

## <a name="python-2-functions-to-access-variables"></a>用于访问变量的 Python 2 函数

下表中的函数用于在 Python 2 Runbook 中访问变量。

|Python 2 函数|说明|
|:---|:---|
|`automationassets.get_automation_variable`|检索现有变量的值。 |
|`automationassets.set_automation_variable`|设置现有变量的值。 |

> [!NOTE]
> 必须在 Python Runbook 顶部导入 `automationassets` 模块才能访问资产函数。

## <a name="create-and-get-a-variable"></a>创建和获取变量

>[!NOTE]
>若要删除某个变量的加密，必须删除该变量，然后重新创建未加密的变量。

### <a name="create-and-get-a-variable-using-the-azure-portal"></a>使用 Azure 门户创建和获取变量

1. 在自动化帐户中单击“资产”磁贴，然后单击“资产”边栏选项卡并选择“变量”。  
2. 在“变量”磁贴中，选择“添加变量”。
3. 填写“新建变量”边栏选项卡上的选项，然后单击“创建”保存新变量。 

> [!NOTE]
> 保存加密的变量后，无法在门户中查看它。 只能更新它。

### <a name="create-and-get-a-variable-in-windows-powershell"></a>在 Windows PowerShell 中创建和获取变量

Runbook 或 DSC 配置使用 `New-AzureRmAutomationVariable` cmdlet 创建新的变量并设置其初始值。 如果变量已加密，则调用应使用 `Encrypted` 参数。 脚本可以使用 `Get-AzureRmAutomationVariable` 检索变量的值。 

>[!NOTE]
>PowerShell 脚本无法检索加密的值。 只能使用内部 `Get-AutomationVariable` cmdlet 执行此操作。

以下示例演示如何创建字符串变量，然后返回其值。

```powershell
New-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" 
-AutomationAccountName "MyAutomationAccount" -Name 'MyStringVariable' `
-Encrypted $false -Value 'My String'
$string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
-AutomationAccountName "MyAutomationAccount" -Name 'MyStringVariable').Value
```

以下示例展示了如何创建复杂类型的变量，然后检索其属性。 在本例中，使用了 [Get-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm?view=azurermps-6.13.0) 返回的虚拟机对象。

```powershell
$vm = Get-AzureRmVM -ResourceGroupName "ResourceGroup01" -Name "VM01"
New-AzureRmAutomationVariable -AutomationAccountName "MyAutomationAccount" -Name "MyComplexVariable" -Encrypted $false -Value $vm

$vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
-AutomationAccountName "MyAutomationAccount" -Name "MyComplexVariable").Value
$vmName = $vmValue.Name
$vmIpAddress = $vmValue.IpAddress
```

## <a name="textual-runbook-examples"></a>文本 Runbook 示例

### <a name="retrieve-and-set-a-simple-value-from-a-variable"></a>检索和设置变量中的简单值

以下示例演示如何设置和检索文本 Runbook 中的变量。 此示例假设要创建名为 `NumberOfIterations` 和 `NumberOfRunnings` 的整数变量，以及名为 `SampleMessage` 的字符串变量。

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

### <a name="retrieve-and-set-a-variable-in-a-python-2-runbook"></a>检索和设置 Python 2 Runbook 中的变量

以下示例演示如何在 Python 2 Runbook 中获取变量、设置变量以及处理不存在的变量的异常。

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

## <a name="graphical-runbook-examples"></a>图形 Runbook 示例

在图形 Runbook 中，可以为内部 cmdlet `Get-AutomationVariable` 或 `Set-AutomationVariable` 添加活动。 只需在图形编辑器的“库”窗格中右键单击每个变量，然后选择所需的活动即可。

![将变量添加到画布](../media/variables/runbook-variable-add-canvas.png)

下图显示了在图形 Runbook 中用于更新具有简单值的变量的示例活动。 在此示例中，`Get-AzureRmVM` 的活动检索单个 Azure 虚拟机，并将计算机名保存到现有的自动化字符串变量中。 [链接是管道还是序列](../automation-graphical-authoring-intro.md#links-and-workflow)并不重要，因为代码只需要输出中的单个对象。

![设置简单变量](../media/variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>后续步骤

* 若要了解关于 Runbook 的常规信息，请参阅 [Azure 自动化中的 Runbook 执行](../automation-runbook-execution.md)。
* 有关 DSC 配置的详细信息，请参阅 [State Configuration 概述](../automation-dsc-overview.md)。
