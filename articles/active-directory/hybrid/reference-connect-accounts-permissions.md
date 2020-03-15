---
title: Azure AD Connect：帐户和权限 | Microsoft Docs
description: 本主题介绍使用和创建的帐户以及所需的权限。
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.reviewer: cychua
ms.assetid: b93e595b-354a-479d-85ec-a95553dd9cc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
origin.date: 10/03/2019
ms.date: 11/13/2019
ms.subservice: hybrid
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: 089dbe2083de00c9586bb8b511b6c75ae49dc6a9
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79290978"
---
# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect：帐户和权限

## <a name="accounts-used-for-azure-ad-connect"></a>用于 Azure AD Connect 的帐户

![帐户概述](./media/reference-connect-accounts-permissions/account5.png)

Azure AD Connect 使用 3 个帐户，将信息从本地或 Windows Server Active Directory 同步到 Azure Active Directory。  这些帐户是：

- AD DS 连接器帐户：用于将信息读/写到 Windows Server Active Directory 

- ADSync 服务帐户：用于运行同步服务和访问 SQL 数据库 

- Azure AD 连接器帐户：用于将信息写入 Azure AD 

除了用于运行 Azure AD Connect 的这三个帐户外，还需要以下其他帐户以安装 Azure AD Connect。  其中包括：

- **本地管理员帐户**：将安装 Azure AD Connect 并且在计算机上具有本地管理员权限的管理员。

- **AD DS 企业管理员帐户**：可以选择使用此帐户创建上面的“AD DS 连接器帐户”。

- Azure AD 全局管理员帐户：用于创建 Azure AD 连接器帐户和配置 Azure AD  。

- SQL SA 帐户（可选）：用于使用完整版 SQL Server 时创建 ADSync 数据库  。  此 SQL Server 对 Azure AD Connect 安装而言可能是本地或远程的。  此帐户可能是企业管理员的帐户。  现在，可以由 SQL 管理员在带外进行数据库预配，然后由具有数据库所有者权限的 Azure AD Connect 管理员完成安装。  有关详细信息，请参阅[使用 SQL 委派的管理员权限安装 Azure AD Connect](how-to-connect-install-sql-delegation.md)


>[!IMPORTANT]
> 从内部版本 1.4.###.# 起，不再支持使用企业管理员或域管理员帐户作为 AD DS 连接器帐户。  如果在指定“使用现有帐户”  时尝试输入是企业管理员或域管理员的帐户，你将收到错误。

> [!NOTE]
> 支持从 ESAE 管理林（也称为“红林”）管理 Azure AD Connect 中使用的管理帐户。
> 专用管理林允许组织在具有比生产环境更强的安全控制的环境中托管管理帐户、工作站和组。
> 若要详细了解专用管理林，请参阅 [ESAE 管理林设计方法](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material#esae-administrative-forest-design-approach)
>>>>>>> e683a61b0ed62ae739941410f658a127534e2481

> [!NOTE]
> 初始设置后不需要全局管理员角色，唯一需要的帐户将是“目录同步帐户”  角色帐户。 这并不意味着你只需要删除具有全局管理员角色的帐户。 最好将角色更改为功能较弱的角色，因为如果需要再次运行向导，完全删除帐户可能会带来问题。 减小角色的权限后，如果你需要再次利用 Azure AD Connect 向导，则始终可以重新提升权限。 

## <a name="installing-azure-ad-connect"></a>安装 Azure AD Connect
Azure AD Connect 安装向导提供提供两种不同的路径：

* 在“快速设置”中，此向导需要更多权限。  这样便可以轻松设置配置，而无需创建用户或配置权限。
* 在“自定义设置”中，此向导可提供更多选择和选项。 但是，在某些情况下，需要确保自己具有相应的权限。



## <a name="express-settings-installation"></a>快速设置安装
在“快速设置”中，安装向导要求提供以下内容：

  - AD DS 企业管理员凭据
  - Azure AD 全局管理员凭据

### <a name="ad-ds-enterprise-admin-credentials"></a>AD DS 企业管理员凭据
AD DS 企业管理员帐户用于配置本地 Active Directory。 这些凭据只能在安装期间使用，而不能在安装完成后使用。 由企业管理员而不是域管理员确保可以在所有域中设置 Active Directory 中的权限。

如果从 DirSync 升级，AD DS 企业管理员凭据可用于重置 DirSync 所用帐户的密码。 此外，还需要 Azure AD 全局管理员凭据。

### <a name="azure-ad-global-admin-credentials"></a>Azure AD 全局管理员凭据
这些凭据只能在安装期间使用，而不能在安装完成后使用。 它用于创建 Azure AD 连接器帐户，以便将更改同步到 Azure AD。 该帐户还会在 Azure AD 中启用同步作为功能。

### <a name="ad-ds-connector-account-required-permissions-for-express-settings"></a>AD DS 连接器帐户需要快速设置权限
创建 AD DS 连接器帐户，用于读取和写入 Windows Server AD，如果由快速设置创建，该帐户具有以下权限：

| 权限 | 用途 |
| --- | --- |
| <li>复制目录更改</li><li>复制所有目录更改 |密码哈希同步 |
| 读取/写入所有用户属性 |导入和执行 Exchange 混合部署 |
| 读取/写入所有 iNetOrgPerson 属性 |导入和执行 Exchange 混合部署 |
| 读取/写入所有组属性 |导入和执行 Exchange 混合部署 |
| 读取/写入所有联系人属性 |导入和执行 Exchange 混合部署 |

### <a name="express-installation-wizard-summary"></a>快速安装向导摘要

![快速安装](./media/reference-connect-accounts-permissions/express.png)

以下是有关快速安装向导页、所收集凭据及其用途的摘要。

| 向导页 | 收集的凭据 | 所需的权限 | 用途 |
| --- | --- | --- | --- |
| 不适用 |运行安装向导的用户 |本地服务器的管理员 |<li>创建用于运行同步服务的 ADSync 服务帐户。 |
| 连接到 Azure AD |Azure AD 目录凭据 |Azure AD 中的全局管理员角色 |<li>在 Azure AD 目录中启用同步。</li>  <li>创建在 Azure AD 中用于持续同步操作的 Azure AD 连接器帐户。</li> |
| 连接到 AD DS |本地 Active Directory 凭据 |Active Directory 中企业管理员 (EA) 组的成员 |<li>在 Active Directory 中创建 AD DS 连接器帐户并向其授予权限。 同步期间，所创建的该帐户用于读取和写入目录信息。</li> |


## <a name="custom-installation-settings"></a>自定义安装设置

通过自定义设置安装，此向导可提供更多选择和选项。 

### <a name="custom-installation-wizard-summary"></a>自定义安装向导摘要

以下是有关自定义安装向导页、所收集凭据及其用途的摘要。

![快速安装](./media/reference-connect-accounts-permissions/customize.png)

| 向导页 | 收集的凭据 | 所需的权限 | 用途 |
| --- | --- | --- | --- |
| 不适用 |运行安装向导的用户 |<li>本地服务器的管理员</li><li>只有 SQL 中的系统管理员 (SA) 才可使用 SQL Server 完整版。</li> |默认情况下，将创建充当同步引擎服务帐户的本地帐户。 只有在管理员未指定特定帐户时才创建该帐户。 |
| 安装同步服务，服务帐户选项 |AD 或本地用户帐户凭据 |用户，权限由安装向导授予 |如果管理员指定了帐户，则此帐户用作同步服务的服务帐户。 |
| 连接到 Azure AD |Azure AD 目录凭据 |Azure AD 中的全局管理员角色 |<li>在 Azure AD 目录中启用同步。</li>  <li>创建在 Azure AD 中用于持续同步操作的 Azure AD 连接器帐户。</li> |
| 连接目录 |要连接到 Azure AD 的每个林的本地 Active Directory 凭据 |权限随所启用的功能而定，可在“创建 AD DS 连接器帐户”中查找 |在同步期间，会使用此帐户读取和写入目录信息。 |
| AD FS 服务器 |对于列表中的每个服务器，如果运行向导的用户的登录凭据权限不足，因而无法连接，则向导会收集凭据 |域管理员 |安装和配置 AD FS 服务器角色。 |
| Web 应用程序代理服务器 |对于列表中的每个服务器，如果运行向导的用户的登录凭据权限不足，因而无法连接，则向导会收集凭据 |目标计算机上的本地管理员 |安装和配置 WAP 服务器角色。 |
| 代理信任凭据 |联合身份验证服务信任凭据（代理用来注册 FS 信任证书的凭据） |作为 AD FS 服务器本地管理员的域帐户 |初始注册 FS-WAP 信任证书。 |
| “AD FS 服务帐户”页上的“使用域用户帐户选项” |AD 用户帐户凭据 |域用户 |提供了其凭据的 Azure AD 用户帐户用作 AD FS 服务的登录帐户。 |

### <a name="create-the-ad-ds-connector-account"></a>创建 AD DS 连接器帐户

>[!IMPORTANT]
>内部版本 1.1.880.0（发布于 2018 年 8 月）中引入了名为 ADSyncConfig.psm1 的新 PowerShell 模块，其中包括有助于为 Azure AD DS 连接器帐户配置正确 Active Directory 权限的 cmdlet 集合  。
>


“连接目录”  页上指定的帐户必须在安装之前存在于 Active Directory 中。  Azure AD Connect 版本 1.1.524.0 及更高版本提供了相应选项，让 Azure AD Connect 向导创建用于连接 Active Directory 的 AD DS 连接器帐户  。  

还必须向它授予所需的权限。 安装向导不会验证权限，任何问题只能在同步期间发现。

需要哪些权限取决于启用的可选功能。 如果有多个域，则必须对林中的所有域授予权限。 如果未启用任何一项功能，默认的 **域用户** 权限就已足够。

| 功能 | 权限 |
| --- | --- |
| ms DS ConsistencyGuid 功能 |对[设计概念 - 使用 ms-DS-ConsistencyGuid 作为 sourceAnchor](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor) 中所述的 ms-DS-ConsistencyGuid 属性的写入权限。 | 
| 密码哈希同步 |<li>复制目录更改</li>  <li>复制所有目录更改 |
| Exchange 混合部署 |针对用户、组和联系人的属性的写入权限，详见[Exchange 混合写回](reference-connect-sync-attributes-synchronized.md#exchange-hybrid-writeback)。 |
| Exchange 邮件公共文件夹 |对 [Exchange 邮件公共文件夹](reference-connect-sync-attributes-synchronized.md#exchange-mail-public-folder)中所述的公共文件夹属性的读取权限。 | 

## <a name="upgrade"></a>升级
从 Azure AD Connect 的一个版本升级到新版本时，需要拥有以下权限：

>[!IMPORTANT]
>从版本 1.1.484 开始，Azure AD Connect 引入了一个回归 bug，导致需要 sysadmin 权限才能升级 SQL 数据库。  在内部版本 1.1.647 中解决了此 bug。  若要升级到此版本，需要 sysadmin 权限。  Dbo 权限是不够的。  如果尝试在没有 sysadmin 权限的情况下升级 Azure AD Connect，升级将失败，之后 Azure AD Connect 将不再正常工作。  Microsoft 已意识到此问题，并在努力更正此问题。


| 主体 | 所需的权限 | 用途 |
| --- | --- | --- |
| 运行安装向导的用户 |本地服务器的管理员 |更新二进制文件 |
| 运行安装向导的用户 |ADSyncAdmins 的成员 |对同步规则和其他配置进行更改。 |
| 运行安装向导的用户 |如果使用完整的 SQL Server：需要同步引擎数据库的 DBO（或类似权限） |进行数据库级别的更改，例如使用新列更新表。 |

## <a name="more-about-the-created-accounts"></a>有关所创建帐户的详细信息
### <a name="ad-ds-connector-account"></a>AD DS 连接器帐户
如果使用快速设置，则会在 Active Directory 中创建用于同步的帐户。 创建的帐户位于用户容器的林根域中，其名称前缀为 **MSOL_** 。 该帐户带有永不过期的长复杂密码。 如果域中有密码策略，请确保允许此帐户使用长密码和复杂密码。

![AD 帐户](./media/reference-connect-accounts-permissions/adsyncserviceaccount.png)

如果使用自定义设置，则需要负责在开始安装之前创建帐户。  请参阅“创建 AD DS 连接器帐户”。

### <a name="adsync-service-account"></a>ADSync 服务帐户
同步服务可在不同的帐户下运行。 它可以在**虚拟服务帐户** (VSA)、**组托管服务帐户** (gMSA/sMSA) 或普通用户帐户下运行。 执行全新安装时，会发现支持的选项已随着 2017 年 4 月版 Connect 的发布而发生变化。 如果从早期版本的 Azure AD Connect 升级，这些附加选项不可用。

| 帐户的类型 | 安装选项 | 说明 |
| --- | --- | --- |
| [虚拟服务帐户](#virtual-service-account) | 2017 年 4 月版和更高版本中的快速和自定义安装 | 此选项用于所有快速安装，但域控制器上的安装除外。 对于自定义安装，除非使用了其他选项，否则它便是默认选项。 |
| [组托管服务帐户](#group-managed-service-account) | 2017 年 4 月版和更高版本中的自定义安装 | 如果使用远程 SQL 服务器，则我们建议使用组托管服务帐户。 |
| [用户帐户](#user-account) | 2017 年 4 月版和更高版本中的快速和自定义安装 | 仅当在 Windows Server 2008 和域控制器上安装时，才会在安装期间创建带有 AAD_ 前缀的用户帐户。 |
| [用户帐户](#user-account) | 2017 年 3 月版和更低版本中的快速和自定义安装 | 安装期间创建带有 AAD_ 前缀的本地帐户。 使用自定义安装时，可以指定另一个帐户。 |

如果配合 2017 年 3 月版或更低版本使用 Connect，则不应重置服务帐户中的密码，否则出于安全原因，Windows 会销毁加密密钥。 在不重新安装 Azure AD Connect 的情况下，无法将帐户更改为其他任何帐户。 如果从 2017 年 4 月版或更高版本升级到某个版本，则支持更改服务帐户的密码，但无法更改使用的帐户。

> [!Important]
> 只能在首次安装时设置服务帐户。 安装完成后，不支持更改服务帐户。

下面是同步服务帐户的默认、建议和支持的选项表格。

图例：

- **粗体**表示默认选项，在大多数情况下也是建议的选项。
- *斜体*指示建议的选项（如果不是默认的选项。
- 2008 - 在 Windows Server 2008 上安装时的默认选项
- 非粗体 - 支持的选项
- 本地帐户 - 服务器上的本地用户帐户
- 域帐户 - 域用户帐户
- sMSA - [独立的托管服务帐户](https://technet.microsoft.com/library/dd548356.aspx)
- gMSA - [组托管服务帐户](https://technet.microsoft.com/library/hh831782.aspx)

| | LocalDB</br>Express | LocalDB/LocalSQL</br>“自定义” | 远程 SQL</br>“自定义” |
| --- | --- | --- | --- |
| **独立/工作组计算机** | 不支持 | **VSA**</br>本地帐户 (2008)</br>本地帐户 |  不支持 |
| **已加入域的计算机** | **VSA**</br>本地帐户 (2008) | **VSA**</br>本地帐户 (2008)</br>本地帐户</br>域帐户</br>sMSA,gMSA | **gMSA**</br>域帐户 |
| **域控制器** | **域帐户** | *gMSA*</br>**域帐户**</br>sMSA| *gMSA*</br>**域帐户**|

#### <a name="virtual-service-account"></a>虚拟服务帐户
虚拟服务帐户是一种特殊类型的帐户，它不带有密码，由 Windows 管理。

![VSA](./media/reference-connect-accounts-permissions/aadsyncvsa.png)

VSA 适用于同步引擎与 SQL 位于同一台服务器上的场合。 如果使用远程 SQL，则建议改用组托管服务帐户。

此功能需要 Windows Server 2008 R2 或更高版本。 如果在 Windows Server 2008 上安装 Azure AD Connect，则安装将回退以改用[用户帐户](#user-account)。

#### <a name="group-managed-service-account"></a>组托管服务帐户
如果使用远程 SQL Server，则建议使用**组托管服务帐户**。 有关如何为组托管服务帐户准备 Active Directory 的详细信息，请参阅[组托管服务帐户概述](https://technet.microsoft.com/library/hh831782.aspx)。

若要使用此选项，请[安装所需的组件](how-to-connect-install-custom.md#install-required-components)页上选择“使用现有的服务帐户”，然后选择“托管服务帐户”。    
![VSA](./media/reference-connect-accounts-permissions/serviceaccount.png)  
也支持使用[独立的托管服务帐户](https://technet.microsoft.com/library/dd548356.aspx)。 但是，这些帐户只能在本地计算机上使用，因此使用这些帐户相对默认虚拟服务帐户而言并没有好处。

此功能需要 Windows Server 2012 或更高版本。 如果需要使用早期的操作系统并使用远程 SQL，则必须使用[用户帐户](#user-account)。

#### <a name="user-account"></a>用户帐户
本地服务帐户由安装向导创建（除非在自定义设置指定了要使用的帐户）。 该帐户具有 **AAD_** 前缀，可用作实际同步服务的运行帐户。 如果在域控制器上安装 Azure AD Connect，则会在该域中创建帐户。 在以下情况下，**AAD_** 服务帐户必须位于域中：
   - 使用运行 SQL Server 的远程服务器
   - 使用需要身份验证的代理

![同步服务帐户](./media/reference-connect-accounts-permissions/syncserviceaccount.png)

该帐户带有永不过期的长复杂密码。

此帐户用于以安全方式存储其他帐户的密码。 其他这些帐户密码会以加密形式存储在数据库中。 通过使用 Windows 数据保护 API (DPAPI) 的密钥加密服务来保护加密密钥的私钥。

如果使用完整的 SQL Server，服务帐户是为同步引擎创建的数据库的 DBO。 如果使用其他权限，服务无法按预期工作。 此外会创建 SQL 登录名。

该帐户也会获取对文件、注册表项和与同步引擎相关的其他对象的权限。

### <a name="azure-ad-connector-account"></a>Azure AD 连接器帐户
会在 Azure AD 中创建帐户供同步服务使用。 可以根据显示名称来识别此帐户。

![AD 帐户](./media/reference-connect-accounts-permissions/aadsyncserviceaccount2.png)

使用该帐户的服务器名称可以根据用户名的第二个部分来识别。 在上图中，服务器名称为 DC1。 如果部署了暂存服务器，每个服务器都有自身的帐户。

该帐户带有永不过期的长复杂密码。 系统为其授予了特殊角色“目录同步帐户”  ，该角色仅可执行目录同步任务。 此特殊内置角色不能在 Azure AD Connect 向导之外授予。 Azure 门户显示具有“用户”角色  的此帐户。

Azure AD 将同步服务帐户数目限制为 20 个。 若要在 Azure AD 中获取现有 Azure AD 服务帐户的列表，请运行以下 Azure AD PowerShell cmdlet：`Get-AzureADDirectoryRole | where {$_.DisplayName -eq "Directory Synchronization Accounts"} | Get-AzureADDirectoryRoleMember`

若要删除未使用的 Azure AD 服务帐户，请运行以下 Azure AD PowerShell cmdlet：`Remove-AzureADUser -ObjectId <ObjectId-of-the-account-you-wish-to-remove>`

>[!NOTE]
>在使用上述 PowerShell 命令之前，需要安装 [Azure Active Directory PowerShell for Graph 模块](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0#installing-the-azure-ad-module)并使用 [Connect-AzureAD](https://docs.microsoft.com/powershell/module/azuread/connect-azuread?view=azureadps-2.0) 连接到 Azure AD 实例

有关如何管理或重置 Azure AD Connect 帐户密码的更多信息，请参阅[管理 Azure AD Connect 帐户](how-to-connect-azureadaccount.md)

## <a name="related-documentation"></a>相关文档
如果尚未阅读文档了解如何[将本地标识与 Azure Active Directory 集成](whatis-hybrid-identity.md)，请查看下表获取相关主题的链接。

|主题 |链接|  
| --- | --- |
|下载 Azure AD Connect | [下载 Azure AD Connect](https://go.microsoft.com/fwlink/?LinkId=615771)|
|使用快速设置安装 | [Azure AD Connect 的快速安装](how-to-connect-install-express.md)|
|使用自定义设置安装 | [Azure AD Connect 的自定义安装](./how-to-connect-install-custom.md)|
|从 DirSync 升级 | [从 Azure AD 同步工具 (DirSync) 升级](how-to-dirsync-upgrade-get-started.md)|
|安装后 | [验证安装并分配许可证](how-to-connect-post-installation.md)|

## <a name="next-steps"></a>后续步骤
了解有关[将本地标识与 Azure Active Directory 集成](whatis-hybrid-identity.md)的详细信息。

<!-- Update_Description: wording update -->