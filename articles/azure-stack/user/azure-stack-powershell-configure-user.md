---
title: 以用户身份使用 PowerShell 连接到 Azure Stack Hub
description: 了解如何使用 PowerShell 连接到 Azure Stack Hub。
author: WenJason
ms.topic: article
origin.date: 1/22/2020
ms.date: 02/24/2020
ms.author: v-jay
ms.reviewer: thoroet
ms.lastreviewed: 10/02/2019
ms.openlocfilehash: 7381ab24774ed6aab60866e78f40a90a30866118
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77541045"
---
# <a name="connect-to-azure-stack-hub-with-powershell-as-a-user"></a>以用户身份使用 PowerShell 连接到 Azure Stack Hub

可以使用 PowerShell 连接到 Azure Stack Hub 以管理 Azure Stack Hub 资源。 例如，可以使用 PowerShell 来订阅产品/服务、创建虚拟机 (VM) 和部署 Azure 资源管理器模板。

若要进行设置，请执行以下操作：
  - 确保满足要求。
  - 与 Azure Active Directory (Azure AD) 或 Active Directory 联合身份验证服务 (AD FS) 连接。 
  - 注册资源提供程序。
  - 测试连接。

## <a name="prerequisites-to-connecting-with-powershell"></a>使用 PowerShell 进行连接的先决条件

如果已[通过 VPN 建立连接](../asdk/asdk-connect.md#connect-to-azure-stack-using-vpn)，请通过[开发工具包](../asdk/asdk-connect.md#connect-to-azure-stack-using-rdp)或基于 Windows 的外部客户端配置这些先决条件：

* 安装 [Azure Stack Hub 兼容的 Azure PowerShell 模块](../operator/azure-stack-powershell-install.md)。
* 下载[使用 Azure Stack Hub 所需的工具](../operator/azure-stack-powershell-download.md)。

确保将以下脚本变量替换为 Azure Stack Hub 配置中的值：

- **Azure AD 租户名称**  
  用于管理 Azure Stack Hub 的 Azure AD 租户名称。 例如，yourdirectory.partner.onmschina.cn。
- **Azure 资源管理器终结点**  
  对于 Azure Stack 开发工具包，此值设置为 https://management.local.azurestack.external 。 若要为 Azure Stack Hub 集成系统获取此值，请与服务提供商联系。

## <a name="connect-to-azure-stack-hub-with-azure-ad"></a>使用 Azure AD 连接到 Azure Stack Hub

```powershell  
    Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint "https://management.local.azurestack.external"
    # Set your tenant name
    $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackUser").ActiveDirectoryAuthority.TrimEnd('/')
    $AADTenantName = "<myDirectoryTenantName>.partner.onmschina.cn"
    $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

    # After signing in to your environment, Azure Stack Hub cmdlets
    # can be easily targeted at your Azure Stack Hub instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackUser" -TenantId $TenantId
```

## <a name="connect-to-azure-stack-hub-with-ad-fs"></a>使用 AD FS 连接到 Azure Stack Hub

  ```powershell  
  # Register an Azure Resource Manager environment that targets your Azure Stack Hub instance
  Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint "https://management.local.azurestack.external"

  # Sign in to your environment
  Login-AzureRmAccount -EnvironmentName "AzureStackUser"
  ```

## <a name="register-resource-providers"></a>注册资源提供程序

不会自动为没有通过门户部署任何资源的新用户订阅自动注册资源提供程序。 可以通过运行以下脚本显式注册资源提供程序：

```powershell  
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider
    }
```

## <a name="test-the-connectivity"></a>测试连接

完成所有设置后，请通过使用 PowerShell 在 Azure Stack Hub 中创建资源来测试连接。 作为测试，为应用程序创建资源组并添加 VM。 运行以下命令创建名为“MyResourceGroup”的资源组：

```powershell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>后续步骤

- [为 Azure Stack Hub 开发模板](azure-stack-develop-templates.md)
- [通过 PowerShell 部署模板](azure-stack-deploy-template-powershell.md)
- [Azure Stack Hub PowerShell 模块参考](https://docs.microsoft.com/powershell/azure/azure-stack/overview)
- 若要为云操作员环境设置 PowerShell，请参阅[配置 Azure Stack Hub 操作员的 PowerShell 环境](../operator/azure-stack-powershell-configure-admin.md)一文。