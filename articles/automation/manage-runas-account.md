---
title: 管理 Azure 自动化运行方式帐户
description: 本文介绍如何使用 PowerShell 或门户管理运行方式帐户。
services: automation
ms.subservice: shared-capabilities
origin.date: 05/24/2019
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: b4c59cd32284ae84241a8331a0ba906bd5d8b77f
ms.sourcegitcommit: 90d01d08faf8adb20083363a8e4e5aab139cd9b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80290381"
---
# <a name="manage-azure-automation-run-as-accounts"></a>管理 Azure 自动化运行方式帐户

Azure 自动化中的运行方式帐户提供身份验证，以使用 Azure cmdlet 管理 Azure 中的资源。 创建运行方式帐户时，将在 Azure Active Directory (AD) 中创建新的服务主体用户，并在订阅级别向此用户分配“参与者”角色。 对于在 Azure 虚拟机上使用混合 Runbook 辅助角色的 Runbook，可以使用 [Azure 资源托管标识](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources)而不是运行方式帐户来对 Azure 资源进行身份验证。

默认情况下，运行方式帐户的服务主体没有读取 Azure AD 的权限。 如果希望添加读取或管理 Azure AD 的权限，需要在“API 权限”  下对服务主体授予该权限。 若要了解详细信息，请参阅[添加用于访问 Web API 的权限](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)。

## <a name="types-of-run-as-accounts"></a>运行方式帐户的类型

Azure 自动化使用两种类型的运行方式帐户：

* Azure 运行方式帐户
* Azure 经典运行方式帐户
### <a name="run-as-account"></a>运行方式帐户

运行方式帐户管理[资源管理器部署模型](../azure-resource-manager/management/deployment-models.md)资源。 它执行以下任务。

* 将创建使用自签名证书的 Azure AD 应用程序，在 Azure AD 中为此应用程序创建服务主体帐户，并在当前订阅中为此帐户分配“参与者”角色。 可将证书设置更改为“所有者”或其他任何角色。 有关详细信息，请参阅 [Azure 自动化中基于角色的访问控制](automation-role-based-access-control.md)。
  
* 在指定的自动化帐户中创建名为 `AzureRunAsCertificate` 的自动化证书资产。 该证书资产保存 Azure AD 应用程序使用的证书私钥。
  
* 在指定的自动化帐户中创建名为 `AzureRunAsConnection` 的自动化连接资产。 该连接资产保存应用程序 ID、租户 ID、订阅 ID 和证书指纹。

### <a name="azure-classic-run-as-account"></a>Azure 经典运行方式帐户

Azure 经典运行方式帐户管理[经典部署模型](../azure-resource-manager/management/deployment-models.md)资源。 只有订阅的共同管理员才能创建或续订这种类型的帐户。

Azure 经典运行方式帐户执行以下任务。

  * 在订阅中创建管理证书

  * 在指定的自动化帐户中创建名为 `AzureClassicRunAsCertificate` 的自动化证书资产。 该证书资产保存管理证书使用的证书私钥。

  * 在指定的自动化帐户中创建名为 `AzureClassicRunAsConnection` 的自动化连接资产。 该连接资产保存订阅名称、订阅 ID 和证书资产名称。

## <a name="run-as-account-permissions"></a><a name="permissions"></a>运行方式帐户的权限

本部分定义普通运行方式帐户和经典运行方式帐户的权限。

### <a name="permissions-to-configure-run-as-accounts"></a>配置运行方式帐户时所需的权限

若要创建或更新运行方式帐户，必须拥有特定的特权和权限。 Azure Active Directory 中的应用程序管理员和订阅中的所有者可以完成所有任务。 下表显示了在实施职责分离的情况下，所需的任务、等效 cmdlet 和权限的列表：

|任务|Cmdlet  |最低权限  |设置权限的位置|
|---|---------|---------|---|
|创建 Azure AD 应用程序|[New-AzADApplication](/https://docs.microsoft.com/powershell/module/az.resources/new-azadapplication?view=azps-3.5.0)     | 应用程序开发人员角色<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>“主页”>“Azure AD”>“应用注册” |
|将凭据添加到应用程序。|[New-AzADAppCredential](https://docs.microsoft.com/powershell/module/az.resources/new-azadappcredential?view=azps-3.5.0)     | 应用程序管理员或全局管理员<sup>1</sup>         |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>“主页”>“Azure AD”>“应用注册”|
|创建和获取 Azure AD 服务主体|[New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal?view=azps-3.5.0)</br>[Get-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/get-azadserviceprincipal?view=azps-3.5.0)     | 应用程序管理员或全局管理员<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>“主页”>“Azure AD”>“应用注册”|
|分配或获取指定主体的 RBAC 角色|[New-AzRoleAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azroleassignment?view=azps-3.5.0)</br>[Get-AzRoleAssignment](https://docs.microsoft.com/powershell/module/Az.Resources/Get-AzRoleAssignment?view=azps-3.5.0)      | 用户访问管理员或所有者，或拥有以下权限：</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br> | [订阅](../role-based-access-control/role-assignments-portal.md)</br>“主页”>“订阅”> \<订阅名称\> -“访问控制(IAM)”|
|创建或删除自动化证书|[New-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationCertificate?view=azps-3.5.0)</br>[Remove-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationcertificate?view=azps-3.5.0)     | 资源组中的参与者         |自动化帐户资源组|
|创建或删除自动化连接|[New-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationconnection?view=azps-3.5.0)</br>[Remove-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationconnection?view=azps-3.5.0)|资源组中的参与者 |自动化帐户资源组|

<sup>1</sup> Azure AD 租户中的非管理员用户可以[注册 AD 应用程序](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)，前提是 Azure AD 租户的“用户设置”页中的“用户可以注册应用程序”选项已设置为“是”。   如果应用程序注册设置为“否”，则执行此操作的用户必须是上表中定义的用户。 

如果你在被添加到订阅的“全局管理员”角色之前不是订阅的 Active Directory 实例的成员，则会将你添加为来宾。 在这种情况下，“添加自动化帐户”页上会显示 `You do not have permissions to create…` 警告。 

如果在分配“全局管理员”角色时你是订阅的 Active Directory 实例的成员，则“添加自动化帐户”页上还会显示 `You do not have permissions to create…` 警告。 在这种情况下，可以请求从订阅的 Active Directory 实例中将你删除，然后请求重新添加，以便成为 Active Directory 中的完整用户。

若要验证是否修正了生成该错误消息的原因：

1. 在 Azure 门户上的“Azure Active Directory”窗格中，选择“用户”。  
2. 选择“所有用户”  。
3. 选择你的姓名，然后选择“配置文件”。  
4. 确保用户配置文件下的“用户类型”属性值未设置为“来宾”。  

### <a name="permissions-to-configure-classic-run-as-accounts"></a><a name="permissions-classic"></a>配置经典运行方式帐户时所需的权限

若要配置或续订经典运行方式帐户，必须在订阅级别具有“共同管理员”角色。 若要了解有关经典订阅权限的详细信息，请参阅 [Azure 经典订阅管理员](../role-based-access-control/classic-administrators.md#add-a-co-administrator)。

## <a name="creating-a-run-as-account-in-azure-portal"></a>在 Azure 门户中创建运行方式帐户

在 Azure 门户中执行以下步骤以更新 Azure 自动化帐户。 单独创建运行方式帐户和经典运行方式帐户。 如果不需管理经典资源，可以只创建 Azure 运行方式帐户。

1. 以订阅管理员角色成员和订阅共同管理员的帐户登录到 Azure 门户。
2. 搜索并选择“自动化帐户”。 
3. 在“自动化帐户”页上，从列表中选择你的自动化帐户。
4. 在左侧窗格中的帐户设置部分选择“运行方式帐户”。 
5. 根据所需帐户，选择“Azure 运行方式帐户”或“Azure 经典运行方式帐户”   。 
6. 根据相关的帐户使用“添加 Azure 运行方式帐户”或“添加 Azure 经典运行方式帐户”窗格。   查看概述信息后，单击“创建”。 
6. 在 Azure 创建运行方式帐户时，可以在菜单的“通知”下面跟踪进度  。 此外还会显示一个横幅，指出正在创建帐户。 该过程可能需要几分钟才能完成。

## <a name="creating-a-run-as-account-using-powershell"></a>使用 PowerShell 创建运行方式帐户

以下列表提供了在 PowerShell 中创建运行方式帐户所要满足的要求。 这些要求适用于这两种类型的运行方式帐户。

* 装有 Azure 资源管理器模块 3.4.1 和更高版本的 Windows 10 或 Windows Server 2016。 PowerShell 脚本不支持早期版本的 Windows。
* Azure PowerShell 1.0 和更高版本。 有关 PowerShell 1.0 版本的信息，请参阅[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs)。
* 作为 `AutomationAccountName` 和 `ApplicationDisplayName` 参数值引用的自动化帐户。
* 与[配置运行方式帐户时所需的权限](#permissions)中所列权限相当的权限。

若要获取 PowerShell 脚本的必需参数 `SubscriptionId`、`ResourceGroupName` 和  的值，请完成以下步骤。

1. 在 Azure 门户中选择“自动化帐户”。 
1. 在“自动化帐户”页上选择你的自动化帐户。
1. 在帐户设置部分选择“属性”。 
1. 记下“属性”页上的“名称”、“订阅 ID”和“资源组”值。    这些值分别对应于 `AutomationAccountName`、`SubscriptionId` 和 `ResourceGroupName` PowerShell 脚本参数值。

   ![自动化帐户属性页](media/manage-runas-account/automation-account-properties.png)

### <a name="powershell-script-to-create-a-run-as-account"></a>用于创建运行方式帐户的 PowerShell 脚本

本部分提供一个用于创建运行方式帐户的 PowerShell 脚本。 该脚本包含对多个配置的支持。

* 使用自签名证书创建运行方式帐户。
* 使用自签名证书创建运行方式帐户和经典运行方式帐户。
* 使用企业证书颁发机构 (CA) 颁发的证书创建运行方式帐户和经典运行方式帐户。
* 在 Azure 中国云中使用自签名证书创建运行方式帐户和经典运行方式帐户。

该脚本使用多个 Azure 资源管理器 cmdlet 来创建资源。 有关 cmdlet 及其所需的权限，请参阅[配置运行方式帐户时所需的权限](#permissions-to-configure-run-as-accounts)。

使用文件名 **New-RunAsAccount.ps1** 将该脚本保存在计算机上。

```powershell
    #Requires -RunAsAdministrator
    Param (
        [Parameter(Mandatory = $true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory = $true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory = $true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory = $true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory = $true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory = $true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory = $false)]
        [string] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [ValidateSet("AzureChinaCloud")]
        [string]$EnvironmentName = "AzureChinaCloud",

        [Parameter(Mandatory = $false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
    )

    function CreateSelfSignedCertificate([string] $certificateName, [string] $selfSignedCertPlainPassword,
        [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
            -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
            -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired) -HashAlgorithm SHA256

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
    }

    function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $keyId = (New-Guid).Guid

        # Create an Azure AD application, AD App Credential, AD ServicePrincipal

        # Requires Application Developer Role, but works with Application administrator or GLOBAL ADMIN
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $keyId)
        # Requires Application administrator or GLOBAL ADMIN
        $ApplicationCredential = New-AzureRmADAppCredential -ApplicationId $Application.ApplicationId -CertValue $keyValue -StartDate $PfxCert.NotBefore -EndDate $PfxCert.NotAfter
        # Requires Application administrator or GLOBAL ADMIN
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        # Requires User Access Administrator or Owner.
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6) {
            Sleep -s 10
            New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
            $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
            $Retries++;
        }
        return $Application.ApplicationId.ToString();
    }

    function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName, [string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
    }

    function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
    }

    Import-Module AzureRm.Profile
    Import-Module AzureRm.Resources

    $AzureRmProfileVersion = (Get-Module AzureRm.Profile).Version
    if (!(($AzureRmProfileVersion.Major -ge 3 -and $AzureRmProfileVersion.Minor -ge 4) -or ($AzureRmProfileVersion.Major -gt 3))) {
        Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
        return
    }

    Connect-AzureRmAccount -Environment $EnvironmentName
    $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

    # Create a Run As account by using a service principal
    $CertifcateAssetName = "AzureRunAsCertificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionTypeName = "AzureServicePrincipal"

    if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
    }
    else {
        $CertificateName = $AutomationAccountName + $CertifcateAssetName
        $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
        $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
        $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
        CreateSelfSignedCertificate $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
    }

    # Create a service principal
    $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
    $ApplicationId = CreateServicePrincipal $PfxCert $ApplicationDisplayName

    # Create the Automation certificate asset
    CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

    # Populate the ConnectionFieldValues
    $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
    $TenantID = $SubscriptionInfo | Select TenantId -First 1
    $Thumbprint = $PfxCert.Thumbprint
    $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

    # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
    CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

    if ($CreateClassicRunAsAccount) {
        # Create a Run As account by using a service principal
        $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
        $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
        $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
        $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
        "Log in to the Azure portal (https://portal.azure.cn) and select Subscriptions -> Management Certificates." + [Environment]::NewLine +
        "Then click Upload and upload the .cer format of #CERT#"

        if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
            $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
            $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
            $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        }
        else {
            $ClassicRunAsAccountCertificateName = $AutomationAccountName + $ClassicRunAsAccountCertifcateAssetName
            $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
            $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
            $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
            $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
            CreateSelfSignedCertificate $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName   $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red       $UploadMessage
    }
```

>[!NOTE]
> Add-AzureRmAccount 现在是 Connect-AzureRMAccount 的别名   。 搜索库项时，如果未看到 **Connect-AzureRMAccount**，可以使用 **Add-AzureRmAccount**。

### <a name="execute-the-powershell-script"></a>执行 PowerShell 脚本

1. 在计算机上，从“开始”屏幕以提升的用户权限启动 **Windows PowerShell**  。
1. 在权限提升的命令行 shell 中，转到包含该脚本的文件夹。
1. 使用所需的配置参数值执行该脚本。
1. 如果创建经典运行方式帐户，则在执行该脚本后，请将公共证书（.cer 文件扩展名）上传到创建自动化帐户的订阅的管理存储中。 

执行脚本后，系统会提示在 Azure 上进行身份验证。 请以订阅管理员角色成员和订阅共同管理员的帐户登录。

#### <a name="create-a-run-as-account-by-using-a-self-signed-certificate"></a>使用自签名证书创建运行方式帐户

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
```

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-a-self-signed-certificate"></a>使用自签名证书创建运行方式帐户和经典运行方式帐户

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
```

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-an-enterprise-certificate"></a>使用企业证书创建运行方式帐户和经典运行方式帐户

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
```

如果使用企业公共证书（.cer 文件）创建了经典运行方式帐户，则使用此证书。  请参阅[将管理 API 证书上传到 Azure 门户](../azure-api-management-certs.md)。

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-a-self-signed-certificate-in-the-azure-china-cloud"></a>在 Azure 中国云中使用自签名证书创建运行方式帐户和经典运行方式帐户

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureChinaCloud
```

如果使用自签名公共证书（.cer 文件）创建了经典运行方式帐户，该脚本将创建该帐户，并将其保存到计算机上的临时文件文件夹中。  可以在用于执行 PowerShell 会话的用户配置文件 `%USERPROFILE%\AppData\Local\Temp` 中找到它。

## <a name="deleting-a-run-as-or-classic-run-as-account"></a>删除运行方式帐户或经典运行方式帐户

本部分介绍如何删除运行方式帐户或经典运行方式帐户。 执行此操作时，将保留自动化帐户。 删除帐户后，可以在 Azure 门户中重新创建它。

1. 在 Azure 门户中，打开自动化帐户。

2. 在左侧窗格中的帐户设置部分选择“运行方式帐户”。 

3. 在运行方式帐户属性页上，选择要删除的运行方式帐户或经典运行方式帐户。 

4. 在所选帐户的“属性”窗格中单击“删除”。 

   ![删除运行方式帐户](media/manage-runas-account/automation-account-delete-runas.png)

5. 帐户删除过程中，可以在菜单的“通知”  下面跟踪进度。

6. 删除该帐户后，可以通过在运行方式帐户属性页中选择创建选项“Azure 运行方式帐户”来重新创建该帐户。 

   ![重新创建自动化运行方式帐户](media/manage-runas-account/automation-account-create-runas.png)

## <a name="renewing-a-self-signed-certificate"></a><a name="cert-renewal"></a>续订自签名证书

为运行方式帐户创建的自签名证书自创建日期算起的一年后过期。 在运行方式帐户过期之前的某个时间点，必须续订该证书。 可以在该证书过期之前的任何时间续订。 

续订自签名证书时，将保留当前的有效证书，以确保已排队等候或正在主动运行且使用运行方式帐户进行身份验证的任何 Runbook 不会受到负面影响。 该证书在过期之前将保持有效。

>[!NOTE]
>如果你认为运行方式帐户已遭到入侵，可以删除再重新创建自签名证书。

>[!NOTE]
>如果已将运行方式帐户配置为使用企业证书颁发机构颁发的证书，并使用续订自签名证书的选项，则该企业证书会被自签名证书取代。

使用以下步骤续订自签名证书。

1. 在 Azure 门户中，打开自动化帐户。

1. 在帐户设置部分选择“运行方式帐户”。 

    ![自动化帐户属性窗格](media/manage-runas-account/automation-account-properties-pane.png)

1. 在运行方式帐户属性页上，选择要为其续订证书的运行方式帐户或经典运行方式帐户。

1. 在所选帐户的“属性”窗格中，单击“续订证书”。 

    ![续订运行方式帐户的证书](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. 证书续订过程中，可以在菜单的“通知”  下面跟踪进度。

## <a name="setting-up-automatic-certificate-renewal-with-an-automation-runbook"></a><a name="auto-cert-renewal"></a>使用自动化 Runbook 设置自动证书续订

若要自动续订证书，可以使用自动化 Runbook。 [GitHub](https://github.com/ikanni/PowerShellScripts/blob/master/AzureAutomation/RunAsAccount/GrantPermissionToRunAsAccountAADApplication-ToRenewCertificateItself-CreateSchedule.ps1) 上的此脚本可在自动化帐户中启用此功能。

>[!NOTE]
>只有 Azure AD 中的“全局管理员”或“公司管理员”才能执行该脚本。

此脚本创建每周续订运行方式帐户证书的计划。 它将 Update-AutomationRunAsCredential Runbook 添加到自动化帐户。  可以在脚本 [Update-AutomationRunAsCredential.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AutomationRunAsCredential.ps1) 中查看 GitHub 上的 Runbook 代码。 可以根据需要，使用该文件中的 PowerShell 代码手动续订证书。

使用以下步骤可立即测试续订过程。

1. 编辑 Update-AutomationRunAsCredential Runbook，并在第 122 行的 Exit(1) 命令前面添加一个注释字符 (#)。  

   ```powershell
   #Exit(1)
   ```

2. 发布 Runbook。
3. 启动 Runbook。
4. 使用以下代码验证续订是否成功：

   ```powershell
   (Get-AzAutomationCertificate -AutomationAccountName TestAA
                                -Name AzureRunAsCertificate
                                -ResourceGroupName TestAutomation).ExpiryTime.DateTime
   ```
    输出：

   ```Output
   Thursday, November 7, 2019 7:00:00 PM
   ```

5. 测试后编辑 Runbook，删除在步骤 1 中添加的注释字符。
6. 发布 Runbook。

## <a name="limiting-run-as-account-permissions"></a><a name="limiting-run-as-account-permissions"></a>限制运行方式帐户权限

若要针对 Azure 中的资源控制自动化目标，可以运行 [Update-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug8) 脚本。 此脚本将现有的运行方式帐户服务主体更改为创建并使用自定义角色定义。 该角色对除 [Key Vault](/key-vault/) 以外的所有资源拥有权限。

>[!IMPORTANT]
>运行 Update-AutomationRunAsAccountRoleAssignments.ps1 脚本后，使用运行方式帐户访问 Key Vault 的 Runbook 将不再可以正常运行。  在运行该脚本之前，应在帐户中检查调用 Azure Key Vault 的 Runbook。 若要从 Azure 自动化 Runbook 访问 Key Vault，必须[添加运行方式帐户对 Key Vault 的权限](#add-permissions-to-key-vault)。

如果需要进一步限制运行方式服务主体可以执行的操作，可将其他资源类型添加到自定义角色定义的 `NotActions` 元素。 以下示例限制对 `Microsoft.Compute/*` 的访问。 如果将此资源类型添加到角色定义的 `NotActions`，该角色将无法访问任何计算资源。 若要详细了解角色定义，请参阅[了解 Azure 资源的角色定义](../role-based-access-control/role-definitions.md)。

```powershell
$roleDefinition = Get-AzureRmRoleDefinition -Name 'Automation RunAs Contributor'
$roleDefinition.NotActions.Add("Microsoft.Compute/*")
$roleDefinition | Set-AzureRMRoleDefinition
```

可以确定运行方式帐户使用的服务主体是在“参与者”角色定义还是自定义角色定义中。 

1. 转到你的自动化帐户，然后在帐户设置部分选择“运行方式帐户”。 
2. 选择“Azure 运行方式帐户”。  
3. 选择“角色”，找到正在使用的角色定义。 

[![](media/manage-runas-account/verify-role.png "Verify the Run As Account role")](media/manage-runas-account/verify-role-expanded.png#lightbox)

还可以确定运行方式帐户对多个订阅或自动化帐户使用的角色定义。 为此，请使用 PowerShell 库中的 [Check-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug5) 脚本。

### <a name="add-permissions-to-key-vault"></a>将权限添加到 Key Vault

可以允许 Azure 自动化验证 Key Vault 和运行方式帐户服务主体是否正在使用自定义角色定义。 必须具备以下条件：

* 授予对 Key Vault 的权限。
* 设置访问策略。

可以使用 PowerShell 库中的 [Extend-AutomationRunAsAccountRoleAssignmentToKeyVault.ps1](https://aka.ms/AA5hugb) 脚本向运行方式帐户授予对 Key Vault 的权限。 有关设置对 Key Vault 的权限的详细信息，请参阅[向应用程序授予对 Key Vault 的访问权限](../key-vault/key-vault-group-permissions-for-apps.md)。

## <a name="resolving-misconfiguration-issues-for-run-as-accounts"></a>解决运行方式帐户的错误配置问题

在初始设置期间，可能不当地删除或创建了运行方式帐户或经典运行方式帐户所需的某些配置项。 可能的错误配置情况包括：

* 证书资产
* 连接资产
* 从“参与者”角色中删除了运行方式帐户
* Azure AD 中的服务主体或应用程序

对于此类错误配置情况，自动化帐户将检测更改，并在该帐户的运行方式帐户属性窗格中显示 `Incomplete` 状态。

![不完整的运行方式帐户配置状态](media/manage-runas-account/automation-account-runas-incomplete-config.png)

选择运行方式帐户时，该帐户的“属性”窗格中会显示以下错误消息：

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

可通过删除并重新创建运行方式帐户来快速解决该帐户的问题。

## <a name="next-steps"></a>后续步骤

* 有关服务主体的详细信息，请参阅[应用程序对象和服务主体对象](../active-directory/develop/app-objects-and-service-principals.md)。
* 有关证书和 Azure 服务的详细信息，请参阅 [Azure 云服务证书概述](../cloud-services/cloud-services-certs-create.md)。
