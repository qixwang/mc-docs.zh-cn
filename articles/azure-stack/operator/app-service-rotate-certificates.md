---
title: 轮换 Azure Stack Hub 上的应用服务的机密和证书
description: 了解如何转换 Azure Stack Hub 上的 Azure 应用服务使用的机密和证书。
author: WenJason
ms.topic: article
origin.date: 04/09/2020
ms.date: 06/22/2020
ms.author: v-jay
ms.reviewer: anwestg
ms.lastreviewed: 04/09/2020
ms.openlocfilehash: aaa3d3d67abb7322b039f91e84c2100b320246df
ms.sourcegitcommit: d86e169edf5affd28a1c1a4476d72b01a7fb421d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85096498"
---
# <a name="rotate-app-service-on-azure-stack-hub-secrets-and-certificates"></a>轮换 Azure Stack Hub 上的应用服务的机密和证书

本文中的说明仅适用于 Azure Stack Hub 上的 Azure 应用服务。 Azure Stack Hub 的集中式机密轮换过程不包括 Azure Stack Hub 上的 Azure 应用服务机密轮换。 操作员可以监视系统中机密的有效性、上次更新日期，以及机密过期之前的剩余时间。

> [!Important]
> 操作员不会在 Azure Stack Hub 仪表板上收到机密过期警报，因为 Azure Stack Hub 上的 Azure 应用服务未与 Azure Stack Hub 警报服务集成。 操作员必须在 Azure Stack Hub 管理员门户中使用 Azure Stack Hub 上的 Azure 应用服务管理体验定期监视其机密。

本文档包含轮换以下机密的过程：

* Azure Stack Hub 上的 Azure 应用服务中使用的加密密钥。
* Azure Stack Hub 上的 Azure 应用服务用来与宿主数据库和计量数据库交互的数据库连接凭据。
* Azure Stack Hub 上的 Azure 应用服务使用的证书，用于确保终结点的安全，以及确保在 Azure Active Directory (Azure AD) 或 Active Directory 联合身份验证服务 (AD FS) 中安全地轮换标识应用程序证书。
* Azure Stack Hub 上的 Azure 应用服务基础结构角色的系统凭据。

## <a name="rotate-encryption-keys"></a>轮换加密密钥

若要轮换 Azure Stack Hub 上的 Azure 应用服务中使用的加密密钥，请执行以下步骤：

1. 在 Azure Stack Hub 管理员门户中转到“应用服务管理”体验。

1. 转到“机密”菜单选项。

1. 选择“加密密钥”部分中的“轮换”按钮。

1. 选择“确定”以启动轮换过程。

1. 加密密钥将会轮换，所有角色实例将会更新。 操作员可以使用“状态”按钮来查看该过程的状态。

## <a name="rotate-connection-strings"></a>轮换连接字符串

若要更新应用服务宿主数据库和计量数据库的数据库连接字符串凭据，请执行以下步骤：

1. 在 Azure Stack Hub 管理员门户中转到“应用服务管理”体验。

1. 转到“机密”菜单选项。

1. 选择“连接字符串”部分中的“轮换”按钮。

1. 提供 **SQL SA 用户名**和**密码**，然后选择“确定”以启动轮换过程。

1. 凭据会在整个 Azure 应用服务角色实例中轮换。 操作员可以使用“状态”按钮来查看该过程的状态。

## <a name="rotate-certificates"></a>轮换证书

若要轮换 Azure Stack Hub 上的 Azure 应用服务中使用的证书，请执行以下步骤：

1. 在 Azure Stack Hub 管理员门户中转到“应用服务管理”体验。

1. 转到“机密”菜单选项。

1. 选择“证书”部分中的“轮换”按钮

1. 为要轮换的证书提供**证书文件**和关联的**密码**，然后选择“确定”。

1. 这些证书会按照要求在 Azure Stack Hub 上的整个 Azure 应用服务角色实例中轮换。 操作员可以使用“状态”按钮来查看该过程的状态。

轮换标识应用程序证书后，还必须使用新证书来更新 Azure AD 或 AD FS 中的相应应用。

> [!IMPORTANT]
> 如果在轮换后没有使用新证书更新标识应用程序，则会影响 Azure Functions 的用户门户体验，妨碍用户使用 KUDU 开发人员工具，并且使管理员无法使用应用服务管理体验来管理辅助角色层规模集。

### <a name="rotate-credential-for-the-azure-ad-identity-application"></a>轮换 Azure AD 标识应用程序的凭据

标识应用程序由操作员在 Azure Stack Hub 上部署 Azure 应用服务之前创建。 如果应用程序 ID 未知，请按照以下步骤来发现它：

1. 转到“Azure Stack Hub 管理员门户”。

1. 转到“订阅”，选择“默认提供商订阅”。 

1. 选择“访问控制(IAM)”，然后选择“应用服务”应用程序。

1. 记下**应用 ID**，此值是必须在 Azure AD 中更新的标识应用程序的应用程序 ID。

若要在 Azure AD 中轮换应用程序的证书，请执行以下步骤：

1. 转到 **Azure 门户**，并使用用于部署 Azure Stack Hub 的全局管理员身份登录。

1. 转到“Azure Active Directory”，然后浏览到“应用注册”。

1. 搜索“应用程序 ID”，然后指定标识应用程序 ID。

1. 选择应用程序，然后转到“证书和机密”。

1. 选择“上传证书”，然后使用以下文件类型之一上传标识应用程序的新证书：.cer、.pem、.crt。

1. 确认**指纹**与 Azure Stack Hub 管理员门户的应用服务管理体验中列出的指纹匹配。

1. 删除旧证书。

### <a name="rotate-certificate-for-ad-fs-identity-application"></a>轮换 AD FS 标识应用程序的证书

标识应用程序由操作员在 Azure Stack Hub 上部署 Azure 应用服务之前创建。 如果应用程序的对象 ID 未知，请按照以下步骤来发现它：

1. 转到“Azure Stack Hub 管理员门户”。

1. 转到“订阅”，选择“默认提供商订阅”。 

1. 选择“访问控制(IAM)”，然后选择 **AzureStack-AppService-<guid>** 应用程序。

1. 记下**对象 ID**，此值是必须在 AD FS 中更新的服务主体的 ID。

若要轮换 AD FS 中的应用程序的证书，需要访问特权终结点 (PEP)。 然后，使用 PowerShell 更新证书凭据，将以下占位符替换为你自己的值：

| 占位符 | 说明 | 示例 |
| ----------- | ----------- | ------- |
| `<PepVM>` | Azure Stack Hub 实例上特权终结点 VM 的名称。 | "AzS-ERCS01" |
| `<CertificateFileLocation>` | X509 证书在磁盘上的位置。 | "d:\certs\sso.cer" |
| `<ApplicationObjectId>` | 分配给标识应用程序的标识符。 | "S-1-5-21-401916501-2345862468-1451220656-1451" |

1. 打开权限提升的 Windows PowerShell 会话，运行以下脚本：

    ```powershell
    # Sign in to PowerShell interactively, using credentials that have access to the VM running the Privileged Endpoint
    $Creds = Get-Credential

    # Create a new Certificate object from the identity application certificate exported as .cer file
    $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2("<CertificateFileLocation>")

    # Create a new PSSession to the PrivelegedEndpoint VM
    $Session = New-PSSession -ComputerName "<PepVm>" -ConfigurationName PrivilegedEndpoint -Credential $Creds

    # Use the privileged endpoint to update the certificate thumbprint, used by the service principal associated with the App Service identity application
    $SpObject = Invoke-Command -Session $Session -ScriptBlock {Set-GraphApplication -ApplicationIdentifier "<ApplicationObjectId>" -ClientCertificates $using:Cert}
    $Session | Remove-PSSession

    # Output the updated service principal details
    $SpObject

    ```

1. 脚本完成后，会显示更新后的应用注册信息，包括证书的指纹值。

    ```shell
    ApplicationIdentifier : S-1-5-21-401916501-2345862468-1451220656-1451
    ClientId              : 
    Thumbprint            : FDAA679BF9EDDD0CBB581F978457A37BFD73CA3B
    ApplicationName       : Azurestack-AppService-d93601c2-1ec0-4cac-8d1c-8ccde63ef308
    ClientSecret          : 
    PSComputerName        : AzS-ERCS01
    RunspaceId            : cb471c79-a0d3-40ec-90ba-89087d104510
    ```

## <a name="rotate-system-credentials"></a>轮换系统凭据

若要轮换 Azure Stack Hub 上的 Azure 应用服务中使用的系统凭据，请执行以下步骤：

1. 在 Azure Stack Hub 管理员门户中转到“应用服务管理”体验。

1. 转到“机密”菜单选项。

1. 选择“系统凭据”部分的“轮换”按钮。

1. 选择要轮换的系统凭据的“范围”。 操作员可以选择轮换所有角色或单个角色的系统凭据。

1. 指定**新的本地管理员用户名**和新**密码**。 然后确认**密码**并选择“确定”。

1. 这些凭据会按照要求在 Azure Stack Hub 上对应的整个 Azure 应用服务角色实例中轮换。 操作员可以使用“状态”按钮来查看该过程的状态。
