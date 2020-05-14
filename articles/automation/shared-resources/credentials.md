---
title: Azure 自动化中的凭据资产
description: Azure 自动化中的凭据资产包含可用于向 Runbook 或 DSC 配置访问的资源进行身份验证的安全凭据。 本文介绍如何创建凭据资产并在 Runbook 或 DSC 配置中使用它们。
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: WenJason
ms.author: v-jay
origin.date: 01/31/2020
ms.date: 05/11/2020
ms.topic: conceptual
manager: digimobile
ms.openlocfilehash: fe1d0a9497d46a804021f6b8fa91080e31b6de32
ms.sourcegitcommit: 7443ff038ea8afe511f7419d9c550d27fb642246
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83001582"
---
# <a name="credential-assets-in-azure-automation"></a>Azure 自动化中的凭据资产

自动化凭据资产包含一个对象，该对象包含用户名和密码等安全凭据。 Runbook 和 DSC 配置使用接受 [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?view=pscore-6.2.0) 对象进行身份验证的 cmdlet。 另外，它们可以提取 `PSCredential` 对象的用户名和密码，以便将其提供给要求进行身份验证的某个应用程序或服务。 

Azure 自动化可以安全地存储凭据的属性。 通过 Runbook 或 DSC 配置访问属性需要使用 [Get-AutomationPSCredential](#activities-used-to-access-credentials) 活动。

> [!NOTE]
> Azure 自动化中的安全资产包括凭据、证书、连接和加密的变量。 这些资产已使用针对每个自动化帐户生成的唯一密钥加密并存储在 Azure 自动化中。 此密钥存储在密钥保管库中。 在存储安全资产之前，从密钥保管库加载密钥，然后使用该密钥加密资产。

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="azure-powershell-az-cmdlets-used-for-credential-assets"></a>用于凭据资产的 Azure PowerShell Az cmdlet

对于 AzureRM，下表中的 cmdlet 用于通过 Windows PowerShell 创建和管理自动化凭据资产。  可在自动化 Runbook 和 DSC 配置中使用的 [AzureRM.Automation 模块](https://docs.microsoft.com/powershell/azure/overview)已随附了这些 cmdlet。

| Cmdlet | 说明 |
|:--- |:--- |
| [Get-AzureRmAutomationCredential](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationcredential) |检索有关凭据资产的信息。 此 cmdlet 不会返回 PSCredential 对象。  |
| [New-AzureRmAutomationCredential](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationcredential) |创建新的自动化凭据。 |
| [Remove-AzureRmAutomationCredential](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationcredential) |删除自动化凭据。 |
| [Set-AzureRmAutomationCredential](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationcredential) |设置现有自动化凭据的属性。 |

## <a name="activities-used-to-access-credentials"></a>用来访问凭据的活动

下表中的活动用来访问 Runbook 和 DSC 配置中的凭据。

| 活动 | 说明 |
|:--- |:--- |
| `Get-AutomationPSCredential` |在 Runbook 或 DSC 配置中获取要使用的凭据。 凭据采用 `PSCredential` 对象的形式。 |
| [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) |通过提示输入用户名和密码来获取凭据。 |
| [New-AzureAutomationCredential](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azureautomationcredential?view=azuresmps-4.0.0) | 创建凭据资产。 |

> [!NOTE]
> 应避免在 `Get-AutomationPSCredential` 的 `Name` 参数中使用变量。 使用变量在设计时可能会导致难以厘清 Runbook 或 DSC 配置与凭据资产之间的依赖关系。

## <a name="python2-functions-that-access-credentials"></a>Python2 函数，用于访问凭据

下表中的函数用于在 Python2 Runbook 中访问凭据。

| 函数 | 说明 |
|:---|:---|
| `automationassets.get_automation_credential` | 检索有关凭据资产的信息。 |

> [!NOTE]
> 在 Python Runbook 的顶部导入 `automationassets` 模块以访问资产函数。

## <a name="creating-a-new-credential-asset"></a>创建新凭据资产

可以使用 Azure 门户或 Windows PowerShell 来创建新的凭据资产。

### <a name="create-a-new-credential-asset-with-the-azure-portal"></a>使用 Azure 门户创建新的凭据资产

1. 在自动化帐户的“共享资源”  下选择“凭据”  。
1. 选择“添加凭据”  。
2. 在“新建凭据”窗格中，根据你的命名标准输入合适的凭据名称。 
3. 在“用户名”字段中键入你的访问 ID。  
4. 对于两个密码字段，请输入你的机密访问密钥。

    ![创建新凭据](../media/credentials/credential-create.png)

5. 如果选中了“多重身份验证”框，请将其取消选中。 
6. 单击“创建”以保存新的凭据资产  。

> [!NOTE]
> Azure 自动化不支持使用多重身份验证的用户帐户。

### <a name="create-a-new-credential-asset-with-windows-powershell"></a>使用 Windows PowerShell 创建新的凭据资产

以下示例展示了如何创建新的自动化凭据资产。 首先创建了一个具有名称和密码的 `PSCredential` 对象，然后使用该对象创建凭据资产。 你可以改为使用 `Get-Credential` cmdlet 来提示用户键入名称和密码。

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="using-a-powershell-credential"></a>使用 PowerShell 凭据

Runbook 或 DSC 配置使用 `Get-AutomationPSCredential` 活动检索凭据资产。 此活动检索 `PSCredential` 对象，可将其用于需要凭据的活动或 cmdlet。 还可以检索要单独使用的凭据对象的属性。 该对象包含用户名和安全密码的属性。 此外，还可以使用 [GetNetworkCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential.getnetworkcredential?view=pscore-6.2.0) 方法来检索表示不安全的密码版本的 [NetworkCredential](https://docs.microsoft.com/dotnet/api/system.net.networkcredential) 对象。

> [!NOTE]
> `Get-AzureRmAutomationCredential` 不会检索可用于身份验证的 `PSCredential` 对象。 它仅提供有关凭据的信息。 如果需要在 Runbook 中使用凭据，则必须使用 `Get-AutomationPSCredential` 将其作为 `PSCredential` 对象进行检索。

### <a name="textual-runbook-example"></a>文本 Runbook 示例

下面的示例展示了如何在 Runbook 中使用 PowerShell 凭据。 它检索凭据并将其用户名和密码分配给变量。


```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

也可使用凭据通过 [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) 向 Azure 进行身份验证。 大多数情况下，应使用[运行方式帐户](../manage-runas-account.md)并使用 [Get-AutomationConnection](../automation-connections.md) 来检索连接。


```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCred.UserName
$securePassword = $myCred.Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$securePassword)

Connect-AzureRmAccount -Credential $myPsCred
```

### <a name="graphical-runbook-example"></a>图形 Runbook 示例

在图形编辑器的“库”窗格中，可以通过右键单击凭据并选择“添加到画布”  将 `Get-AutomationPSCredential` 活动添加到图形 Runbook。

![将凭据添加到画布](../media/credentials/credential-add-canvas.png)

下图显示了在图形 Runbook 中使用凭据的示例。 在本例中，凭据为 Runbook 提供针对 Azure 资源的身份验证。 第一个活动检索有权访问 Azure 订阅的凭据。 然后，帐户连接活动使用此凭据为它之后的任何活动提供身份验证。 此处使用了一个[管道链接](../automation-graphical-authoring-intro.md#links-and-workflow)，因为 `Get-AutomationPSCredential` 需要单个对象。  

![将凭据添加到画布](../media/credentials/get-credential.png)

## <a name="using-credentials-in-a-dsc-configuration"></a>使用 DSC 配置中的凭据

虽然 Azure 自动化中的 DSC 配置可以使用 `Get-AutomationPSCredential` 来处理凭据资产，但它们也可以通过参数传递凭据资产。 有关详细信息，请参阅 [Compiling configurations in Azure Automation DSC](../automation-dsc-compile.md#credential-assets)（在 Azure 自动化 DSC 中编译配置）。

## <a name="using-credentials-in-python2"></a>在 Python2 中使用凭据

以下示例展示了如何在 Python2 Runbook 中访问凭据。


```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a credential
cred = automationassets.get_automation_credential("credtest")
print cred["username"]
print cred["password"]
```

## <a name="next-steps"></a>后续步骤

* 若要详细了解图形创作中的链接，请参阅[图形创作中的链接](../automation-graphical-authoring-intro.md#links-and-workflow)。
* 若要了解用于自动化的各种身份验证方法，请参阅 [Azure 自动化安全性](../automation-security-overview.md)。
* 若要开始使用图形 Runbook，请参阅[我的第一个图形 Runbook](../automation-first-runbook-graphical.md)。
* 若要开始使用 PowerShell 工作流 Runbook，请参阅 [My first PowerShell workflow runbook](../automation-first-runbook-textual.md)（我的第一个 PowerShell 工作流 Runbook）。
* 若要开始使用 Python2 runbook，请参阅[第一个 Python2 runbook](../automation-first-runbook-textual-python2.md)。 
