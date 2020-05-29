---
title: 管理 Azure 自动化中的连接
description: Azure 自动化中的连接资产包含从 Runbook 或 DSC 配置连接到外部服务或应用程序所需的信息。 本文介绍了有关连接的详细信息，以及如何在文本和图形创作中使用连接。
services: automation
ms.subservice: shared-capabilities
origin.date: 01/13/2020
ms.date: 05/25/2020
ms.topic: conceptual
ms.custom: has-adal-ref
ms.openlocfilehash: 97b21e8b8f83c37dce6c809b5cb370f788ca45fd
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801302"
---
# <a name="manage-connections-in-azure-automation"></a>管理 Azure 自动化中的连接

Azure 自动化连接资产包含下列信息。 从 Runbook 或 DSC 配置连接到外部服务或应用程序需要此信息。 

* 身份验证所需的信息，例如用户名和密码
* 连接信息，例如 URL 或端口

连接资产会将用于连接特定应用程序的所有属性集中在一起，因此无需创建多个变量。 你可以从一个位置编辑连接的值，并且可以在单个参数中将连接名称传递给 Runbook 或 DSC 配置。 Runbook 或配置使用内部 `Get-AutomationConnection` cmdlet 访问连接的属性。

创建连接时，必须指定连接类型。 连接类型是定义了一组属性的模板。 你可以将集成模块与元数据文件配合使用，以将连接类型添加到 Azure 自动化。 此外还可以使用 [Azure 自动化 API](https://docs.microsoft.com/previous-versions/azure/reference/mt163818(v=azure.100)) 创建连接类型，前提是集成模块包含连接类型，并且已导入到自动化帐户中。 

>[!NOTE]
>Azure 自动化中的安全资产包括凭据、证书、连接和加密的变量。 这些资产已使用针对每个自动化帐户生成的唯一密钥加密并存储在 Azure 自动化中。 Azure 自动化将密钥存储在系统托管的 Key Vault 中。 在存储安全资产之前，自动化会从 Key Vault 加载该密钥，然后用它加密资产。 

## <a name="connection-types"></a>连接类型

Azure 自动化提供以下内置连接类型：

* `Azure` - 表示用于管理经典资源的连接。
* `AzureServicePrincipal` - 表示 Azure 运行方式帐户使用的连接。
* `AzureClassicCertificate` - 表示经典 Azure 运行方式帐户使用的连接。

在大多数情况下不需要创建连接资源，因为在创建[运行方式帐户](manage-runas-account.md)时已创建了该连接资源。

## <a name="powershell-cmdlets-to-access-connections"></a>用于访问连接的 PowerShell cmdlet

下表中的 cmdlet 通过 PowerShell 创建和管理自动化连接。 它们作为 [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/overview)的一部分提供。

|Cmdlet|说明|
|---|---|
|[Get-AzureRmAutomationConnection](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationconnection)|检索有关连接的信息。|
|[New-AzureRmAutomationConnection](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationconnection)|创建新连接。|
|[Remove-AzureRmAutomationConnection](https://docs.microsoft.com/powershell/module/azurerm.automation/remove-azurermautomationconnection)|删除现有连接。|
|[Set-AzureRmAutomationConnectionFieldValue](https://docs.microsoft.com/powershell/module/azurerm.automation/set-azurermautomationconnectionfieldvalue)|设置现有连接的一个特定字段的值。|

## <a name="internal-cmdlets-to-access-connections"></a>用于访问连接的内部 cmdlet

下表中的内部 cmdlet 用于访问 Runbook 和 DSC 配置中的连接。

|内部 Cmdlet|说明|
|---|---|
|`Get-AutomationConnection` | 检索连接中不同字段的值，并将它们以[哈希表](https://go.microsoft.com/fwlink/?LinkID=324844)的形式返回。 此哈希表随后可用于 Runbook 或 DSC 配置中的相应命令。|

>[!NOTE]
>避免在 `Get-AutomationConnection` 的 `Name` 参数中使用变量。 在这种情况下，使用变量可能会使在设计时发现 Runbook 或 DSC 配置与连接资产之间的依赖关系变得复杂。

## <a name="python-2-functions-to-access-connections"></a>用于访问连接的 Python 2 函数

下表中的函数用于在 Python 2 Runbook 中访问连接。

| 函数 | 说明 |
|:---|:---|
| `automationassets.get_automation_connection` | 检索连接。 返回包括该连接属性的字典。 |

> [!NOTE]
> 必须在 Python Runbook 顶部导入 `automationassets` 模块才能访问资产函数。

## <a name="create-a-new-connection"></a>创建新连接

### <a name="create-a-new-connection-with-the-azure-portal"></a>使用 Azure 门户创建新连接

若要在 Azure 门户中创建新连接，请执行以下操作：

1. 在自动化帐户中，单击“共享资源”下的“连接” 。
2. 在“连接”页面上，单击“+ 添加连接”。
4. 在“新建连接”窗格的“类型”字段中，选择要创建的连接类型。 可供选择的类型有 `Azure`、`AzureServicePrincipal` 和 `AzureClassicCertificate`。 
5. 表单会显示所选连接类型的属性。 完成该表单，并单击“创建”以保存新连接。

### <a name="create-a-new-connection-with-windows-powershell"></a>使用 Windows PowerShell 创建新连接

通过在 Windows PowerShell 中使用 `New-AzureRmAutomationConnection` cmdlet 创建新连接。 此 cmdlet 有一个名为 `ConnectionFieldValues` 的参数，该参数需要一个哈希表，用于为连接类型定义的每个属性定义值。

可以使用以下示例命令，作为从门户创建运行方式帐户以新建连接资产的替代方法。

```powershell
$ConnectionAssetName = "AzureRunAsConnection"
$ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
```

创建自动化帐户时，默认情况下该帐户会包含多个全局模块以及用于创建 `AzureRunAsConnection` 连接资产的连接类型 `AzureServicePrincipal`。 如果尝试使用其他身份验证方法创建新的连接资产来连接到服务或应用程序，则操作会失败，原因在于连接类型尚未在自动化帐户中定义。

## <a name="get-a-connection-in-a-runbook-or-dsc-configuration"></a>在 Runbook 或 DSC 配置中获取连接

使用内部 `Get-AutomationConnection` cmdlet 检索 Runbook 或 DSC 配置中的连接。 此 cmdlet 优于 `Get-AzureRmAutomationConnection` cmdlet，原因在于它检索连接值而不是有关连接的信息。 

### <a name="textual-runbook-example"></a>文本 Runbook 示例

以下示例演示如何使用运行方式帐户，向 Runbook 中的 Azure 资源管理器资源进行身份验证。 它使用表示该运行方式帐户的连接资产，该帐户引用基于证书的服务主体。

```powershell
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint -EnvironmentName "AzureChinaCloud" 
```

### <a name="graphical-runbook-examples"></a>图形 Runbook 示例

可以将内部 `Get-AutomationConnection` cmdlet 的活动添加到图形 Runbook。 在图形编辑器的“库”窗格中，右键单击连接，然后选择“添加到画布”。

![添加到画布](media/automation-connections/connection-add-canvas.png)

下图显示了在图形 Runbook 中使用连接对象的示例。 此示例将 `Constant value` 数据集用于 `Get RunAs Connection` 活动，该活动使用连接对象进行身份验证。 此处使用了一个[管道链接](automation-graphical-authoring-intro.md#links-and-workflow)，因为 `ServicePrincipalCertificate` 参数集需要单个对象。

![获取连接](media/automation-connections/automation-get-connection-object.png)

### <a name="python-2-runbook-example"></a>Python 2 Runbook 示例

下面的示例演示如何在 Python 2 Runbook 中使用运行方式连接进行身份验证。

```python
""" Tutorial to show how to authenticate against Azure resource manager resources """
import azure.mgmt.resource
import automationassets

def get_automation_runas_credential(runas_connection):
    """ Returns credentials to authenticate against Azure resoruce manager """
    from OpenSSL import crypto
    from msrestazure import azure_active_directory
    import adal

    # Get the Azure Automation Run As service principal certificate
    cert = automationassets.get_automation_certificate("AzureRunAsCertificate")
    pks12_cert = crypto.load_pkcs12(cert)
    pem_pkey = crypto.dump_privatekey(
        crypto.FILETYPE_PEM, pks12_cert.get_privatekey())

    # Get Run As connection information for the Azure Automation service principal
    application_id = runas_connection["ApplicationId"]
    thumbprint = runas_connection["CertificateThumbprint"]
    tenant_id = runas_connection["TenantId"]

    # Authenticate with service principal certificate
    resource = "https://management.core.chinacloudapi.cn/"
    authority_url = ("https://login.partner.microsoftonline.cn/" + tenant_id)
    context = adal.AuthenticationContext(authority_url)
    return azure_active_directory.AdalAuthentication(
        lambda: context.acquire_token_with_client_certificate(
            resource,
            application_id,
            pem_pkey,
            thumbprint)
    )


# Authenticate to Azure using the Azure Automation Run As service principal
runas_connection = automationassets.get_automation_connection(
    "AzureRunAsConnection")
azure_credential = get_automation_runas_credential(runas_connection)
```

## <a name="next-steps"></a>后续步骤

* 若要了解关于 Runbook 的常规信息，请参阅 [Azure 自动化中的 Runbook 执行](automation-runbook-execution.md)。
* 有关 DSC 配置的详细信息，请参阅[状态配置概述](automation-dsc-overview.md)。