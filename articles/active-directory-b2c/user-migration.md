---
title: 用户迁移方法
titleSuffix: Azure AD B2C
description: 介绍有关使用 Azure AD 图形 API 和选择性地使用 Azure AD B2C 自定义策略执行用户迁移的核心概念和高级概念。
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/04/2020
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: 1dae9609b27b932aaffe3c32c7cba5cdb6b86cfb
ms.sourcegitcommit: 888cbc10f2348de401d4839a732586cf266883bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028259"
---
# <a name="migrate-users-to-azure-active-directory-b2c"></a>将用户迁移到 Azure Active Directory B2C

将标识提供者迁移到 Azure Active Directory B2C (Azure AD B2C)C 时，可能还需要迁移用户帐户。 本文介绍如何将现有用户帐户从任何标识提供者迁移到 Azure AD B2C。 本文并未阐述约定俗成的内容，而只是介绍了几种方案。 开发人员负责判断每种方法是否适用。

## <a name="user-migration-flows"></a>用户迁移流

使用 Azure AD B2C 可以通过 [Azure AD 图形 API][B2C-GraphQuickStart] 迁移用户。 用户迁移过程划分为两个流：

- **预迁移**：如果对用户凭据（用户名和密码）拥有明确的访问权限，或者凭据已加密但可将其解密，则适合使用此流。 预迁移过程涉及到从旧的标识提供者读取用户，并在 Azure AD B2C 目录中创建新帐户。

- **预迁移和密码重置**：如果无法访问用户的密码，则适合使用此流。 例如：
  - 密码以 HASH 格式存储。
  - 密码存储在你无权访问的标识提供者中。 旧标识提供者通过调用 Web 服务来验证用户凭据。

在这两个流中，首先需要运行预迁移过程，从旧标识提供者读取用户，并在 Azure AD B2C 目录中创建新帐户。 如果没有密码，可以使用随机生成的密码创建帐户。 然后要求用户更改密码，或者，当用户首次登录时，Azure AD B2C 会要求用户重置密码。

## <a name="password-policy"></a>密码策略

本地帐户的 Azure AD B2C 密码策略基于 Azure AD 策略。 Azure AD B2C 的注册或登录和密码重置策略使用“强”密码强度，并且不会让任何密码过期。 有关详细信息，请参阅 [Azure AD 密码策略][AD-PasswordPolicies]。

如果要迁移的帐户使用的密码强度弱于 [Azure AD B2C 强制实施的强密码强度][AD-PasswordPolicies]，可以禁用强密码要求。 若要更改默认密码策略，请将 `passwordPolicies` 属性设置为 `DisableStrongPassword`。 例如，可按如下所示修改创建用户请求：

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

## <a name="step-1-use-azure-ad-graph-api-to-migrate-users"></a>步骤 1：使用 Azure AD 图形 API 迁移用户

通过图形 API 创建 Azure AD B2C 用户帐户（使用密码或随机密码）。 本部分介绍使用图形 API 在 Azure AD B2C 目录中创建用户帐户的过程。

### <a name="step-11-register-your-application-in-your-tenant"></a>步骤 1.1：在租户中注册应用程序

若要与图形 API 通信，首先必须创建具有管理特权的服务帐户。 在 Azure AD 中，注册应用程序并启用对目录的写入访问。 应用程序凭据是**应用程序 ID** 和**应用程序机密**。 应用程序代表自身而不是用户来调用图形 API。

首先，注册一个可用于管理任务（例如用户迁移）的应用程序。

[!INCLUDE [active-directory-b2c-appreg-mgmt](../../includes/active-directory-b2c-appreg-mgmt.md)]

### <a name="step-12-grant-administrative-permission-to-your-application"></a>步骤 1.2：向应用程序授予管理权限

接下来，向应用程序授予所需的 Azure AD 图形 API 权限，使其能够写入到目录。

[!INCLUDE [active-directory-b2c-permissions-directory](../../includes/active-directory-b2c-permissions-directory.md)]

### <a name="step-13-create-the-application-secret"></a>步骤 1.3：创建应用程序机密

创建一个客户端机密（密钥），供后续步骤中配置的用户迁移应用程序使用。

[!INCLUDE [active-directory-b2c-client-secret](../../includes/active-directory-b2c-client-secret.md)]

现已创建一个有权在 Azure AD B2C 租户中创建、读取和更新用户的应用程序。

### <a name="step-14-optional-environment-cleanup"></a>步骤 1.4：（可选）清理环境

“读取和写入目录数据”权限不包括删除用户的权限。   如果想要为应用程序提供删除用户（以清理环境）的功能，必须执行额外的步骤，这涉及到运行 PowerShell 来设置“用户帐户管理员”权限。 否则，可以跳到下一部分。

> [!IMPORTANT]
> 必须使用 B2C 租户本地的 B2C 租户管理员帐户。  帐户名语法为 *admin\@contosob2c.partner.onmschina.cn*。

在需要 [Azure AD PowerShell V2 模块][AD-Powershell]的此 PowerShell 脚本中执行以下操作：

1. 连接到联机服务。 为此，请在 Windows PowerShell 命令提示符下运行 cmdlet `Connect-AzureAD` 并提供凭据。

1. 使用**应用程序 ID** 为应用程序分配用户帐户管理员角色。 这些角色具有已知的标识符，因此，我们要做的就是在脚本中输入自己的**应用程序 ID**。

```powershell
# NOTE: This script REQUIRES the Azure AD PowerShell V2 module
#       https://docs.microsoft.com/powershell/azure/active-directory/install-adv2

Connect-AzureAD -AzureEnvironmentName AzureChinaCloud

$AppId = "<Your application ID>"

# Fetch Azure AD application to assign to role
$roleMember = Get-AzureADServicePrincipal -Filter "AppId eq '$AppId'"

# Fetch User Account Administrator role instance
$role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}

# If role instance does not exist, instantiate it based on the role template
if ($role -eq $null) {
    # Instantiate an instance of the role template
    $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq 'User Account Administrator'}
    Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId

    # Fetch User Account Administrator role instance again
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}
}

# Add application to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
```

将 `$AppId` 值更改为自己的 Azure AD **应用程序 ID**。

## <a name="step-2-pre-migration-application-sample"></a>步骤 2：迁移前的应用程序示例

可以在社区维护的 `azure-ad-b2c/user-migration` GitHub 存储库中找到迁移前的代码示例：

[azure-ad-b2c/user-migration/pre-migration][UserMigrationSample-code] (GitHub)

### <a name="step-21-edit-the-migration-data-file"></a>步骤 2.1：编辑迁移数据文件

示例应用使用包含虚构用户数据的 JSON 文件。 成功运行示例后，可以更改代码以使用自己数据库中的数据。 或者，可将用户配置文件导出到 JSON 文件，然后将应用设置为使用该文件。

若要编辑 JSON 文件，请打开 `AADB2C.UserMigration.sln` Visual Studio 解决方案。 在 `AADB2C.UserMigration` 项目中打开 `UsersData.json` 文件。

![显示两个用户的 JSON 块的 UsersData.json 文件部分](./media/user-migration/pre-migration-data-file.png)

可以看到，该文件包含用户实体的列表。 每个用户实体包含以下属性：

- 电子邮件
- displayName
- firstName
- lastName
- 密码（不能为空）

> [!NOTE]
> 编译时，Visual Studio 会将该文件复制到 `bin` 目录。

### <a name="step-22-configure-the-application-settings"></a>步骤 2.2：配置应用程序设置

在 `AADB2C.UserMigration` 项目下，打开 *App.config* 文件。 将以下应用设置替换为自己的值：

```XML
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Client Secret Key from above}" />
    <add key="MigrationFile" value="{Name of a JSON file containing the users' data; for example, UsersData.json}" />
    <add key="BlobStorageConnectionString" value="{Your connection Azure table string}" />
</appSettings>
```

> [!NOTE]
> - 后续部分会介绍 Azure 表连接字符串的用法。
> - B2C 租户的名称是在创建租户期间输入的域，已显示在 Azure 门户中。 租户名称通常以后缀 *.partner.onmschina.cn* 结尾（例如 *contosob2c.partner.onmschina.cn*）。

### <a name="step-23-run-the-pre-migration-process"></a>步骤 2.3：运行预迁移过程

右键单击 `AADB2C.UserMigration` 解决方案并重新生成示例。 如果成功，则现在 `AADB2C.UserMigration\bin\Debug\net461` 中应该有一个可执行文件 `UserMigration.exe`。 若要运行迁移过程，请使用以下命令行参数之一：

- 若要**使用密码迁移用户**，请使用 `UserMigration.exe 1` 命令。

- 若要**使用随机密码迁移用户**，请使用 `UserMigration.exe 2` 命令。 此操作还会创建 Azure 表实体。 稍后可以配置策略来调用 REST API 服务。 该服务使用 Azure 表来跟踪和管理迁移过程。

![显示 UserMigration.exe 命令的输出的命令提示符窗口](./media/user-migration/pre-migration-demo.png)

### <a name="step-24-check-the-pre-migration-process"></a>步骤 2.4：检查预迁移过程

若要验证迁移，请使用以下两种方法之一：

- 若要按显示名称搜索用户，请使用 Azure 门户：

   1. 打开“Azure AD B2C”并选择“用户”。  
   1. 在搜索框中，键入用户的显示名称，并查看该用户的配置文件。

- 若要按登录电子邮件地址检索用户，请使用该示例应用程序：

   1. 运行以下命令：

      ```Console
          UserMigration.exe 3 {email address} > UserProfile.json
      ```

      > [!TIP]
      > 还可以使用以下命令按显示名称检索用户：`UserMigration.exe 4 "<Display name>"`。

   1. 在 JSON 编辑器中打开 UserProfile.json 文件以查看用户信息。

      ![在 Visual Studio Code 编辑器中打开的 UserProfile.json 文件](./media/user-migration/pre-migration-get-by-email2.png)

### <a name="step-25-optional-environment-cleanup"></a>步骤 2.5：（可选）清理环境

若要清理 Azure AD 租户并从 Azure AD 目录中删除用户，请运行 `UserMigration.exe 5` 命令。

> [!NOTE]
> * 若要清理租户，请为应用程序配置“用户帐户管理员”权限。
> * 示例迁移应用会清理 JSON 文件中列出的所有用户。

### <a name="step-26-sign-in-with-migrated-users-with-password"></a>步骤 2.6：以迁移用户的身份登录（使用密码）

使用用户密码运行预迁移过程后，帐户可供使用，并且用户能够使用 Azure AD B2C 登录到应用程序。 如果无权访问用户密码，请转到下一部分。

## <a name="step-3-help-users-reset-their-password"></a>步骤 3：帮助用户重置其密码

如果使用随机密码迁移了用户，则用户必须重置其密码。 若要帮助用户重置密码，请发送一封欢迎电子邮件并在其中包含用于重置密码的链接。

若要获取密码重置策略的链接，请执行以下步骤： 此过程假设事先已创建密码重置自定义策略。

1. 使用 [Azure 门户](https://portal.azure.cn)右上部分中的“目录 + 订阅”筛选器，选择包含你的 Azure AD B2C 租户的目录。 
1. 在左侧菜单（或者“所有服务”）中选择“Azure AD B2C”。  
1. 在“策略”下，选择“Identity Experience Framework”。  
1. 选择密码重置策略。 例如 *B2C_1A_PasswordReset*。
1. 在“选择应用程序”下拉列表中选择应用程序。 

    > [!NOTE]
    > 如果选择“立即运行”，需要在租户中至少注册一个应用程序。  若要了解如何注册应用程序，请参阅[教程：在 Azure Active Directory B2C 中注册应用程序][B2C-AppRegister]。

1. 复制“立即运行终结点”文本框中显示的 URL，并将其发送给用户。 

    ![密码重置策略页，其中突出显示了“立即运行终结点”](./media/user-migration/pre-migration-policy-uri.png)


[AD-PasswordPolicies]: /active-directory/active-directory-passwords-policy
[AD-Powershell]: https://docs.microsoft.com/powershell/azure/active-directory/install-adv2
[AppService-Deploy]: https://docs.microsoft.com/aspnet/core/tutorials/publish-to-azure-webapp-using-vs
[B2C-AppRegister]: /active-directory-b2c/active-directory-b2c-app-registration
[B2C-GetStarted]: /active-directory-b2c/active-directory-b2c-get-started
[B2C-GraphQuickStart]: /active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet
[B2C-NavContext]: /active-directory-b2c/active-directory-b2c-navigate-to-b2c-context
[Portal]: https://portal.azure.cn/
[UserMigrationSample-code]: https://github.com/azure-ad-b2c/user-migration/tree/master/pre-migration/source-code
[UserMigrationSample-policy]: https://github.com/azure-ad-b2c/user-migration/tree/master/pre-migration/policy

