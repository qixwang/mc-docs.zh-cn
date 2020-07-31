---
title: Azure AD 域服务的常见部署场景 | Microsoft Docs
description: 了解 Azure Active Directory 域服务提供价值和满足业务需求的一些常见场景和用例。
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: c5216ec9-4c4f-4b7e-830b-9d70cf176b20
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: v-junlch
ms.openlocfilehash: 790f5a8d98c184cf082b42aa5274080e84ff0f6c
ms.sourcegitcommit: fe9ccd3bffde0dd2b528b98a24c6b3a8cbe370bc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86472625"
---
# <a name="common-use-cases-and-scenarios-for-azure-active-directory-domain-services"></a>Azure Active Directory 域服务的常见用例和场景

Azure Active Directory 域服务 (Azure AD DS) 提供托管域服务，例如域加入、组策略、轻型目录访问协议 (LDAP) 和 Kerberos/NTLM 身份验证。 Azure AD DS 与现有 Azure AD 租户集成，因此用户可使用其现有凭据登录。 可以使用这些域服务，而无需在云中部署、管理和修补域控制器，从而更顺畅地将本地资源直接迁移到 Azure。

本文概述 Azure AD DS 提供价值并满足这些需求的一些常见业务场景。

## <a name="secure-administration-of-azure-virtual-machines"></a>安全管理 Azure 虚拟机

为了使你能够使用一组 AD 凭据，可以将 Azure 虚拟机 (VM) 加入 Azure AD DS 托管域。 此方法可减少凭据管理问题，例如维护每个 VM 上的本地管理员帐户或环境之间的帐户和密码。

还可以使用组策略来管理和保护已加入托管域的 VM。 可将所需的安全基准应用到 VM，根据企业安全指导原则锁定这些 VM。 例如，可以使用组策略管理功能来限制可在 VM 上启动的应用程序类型。

![以简化的方式管理 Azure 虚拟机](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

我们来看一下常见示例场景。 由于服务器和其他基础结构已达到生命周期，Contoso 想要将目前托管在本地的应用程序迁移到云中。 他们当前的 IT 标准强制要求托管企业应用程序的服务器必须加入域，并使用组策略进行管理。 Contoso 的 IT 管理员更希望将 Azure 中部署的 VM 加入域，这样用户随后便可使用其企业凭据登录，从而使管理更轻松。 加入域时，还可以根据所需的安全基准，使用组策略对象 (GPO) 配置 VM。 Contoso 不希望在 Azure 中部署、监视和管理自己的域控制器。

Azure AD DS 非常适合这种情况。 通过托管域，你可以将 VM 加入域、使用一组凭据并应用组策略。 托管域无需配置和维护自己的域控制器。

### <a name="deployment-notes"></a>部署注意事项

以下部署注意事项适用于此示例用例：

* 默认情况下，托管域使用单个平面组织单位 (OU) 结构。 所有已加入域的 VM 均位于单个 OU 中。 如果需要，可以创建自定义 OU。
* Azure AD DS 使用分别适用于用户和计算机容器的内置 GPO。 若要进行其他控制，可以创建自定义 GPO，并将其目标设为自定义 OU。
* Azure AD DS 支持基本 AD 计算机对象架构。 无法扩展计算机对象的架构。

## <a name="lift-and-shift-on-premises-applications-that-use-ldap-bind-authentication"></a>直接迁移使用 LDAP 绑定身份验证的本地应用程序

有一个示例场景，多年前，Contoso 从一家 ISV 采购了一个本地应用程序。 该应用程序目前已被 ISV 置于维护模式，请求对该应用程序进行更改将代价不菲，让人难以负担。 此应用程序有一个基于 Web 的前端，该前端使用 Web 表单收集用户凭据，并向本地 AD DS 环境执行 LDAP 绑定来验证用户的身份。

![LDAP 绑定](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso 希望将此应用程序迁移到 Azure。 此应用程序应继续照常工作，无需进行任何更改。 此外，用户应该能够使用其现有的企业凭据进行身份验证，且无需其他培训。 应用程序运行的位置应向最终用户公开。

对于这种情况，Azure AD DS 允许应用程序在身份验证过程中执行 LDAP 绑定。 旧版本地应用程序可以直接迁移到 Azure，并继续无缝验证用户身份，而无需更改任何配置或用户体验。

### <a name="deployment-notes"></a>部署注意事项

以下部署注意事项适用于此示例用例：

* 确保应用程序无需在目录中修改/写入数据。 不支持对托管域进行 LDAP 写入访问。
* 无法直接针对托管域更改密码。 最终用户可以使用 Azure AD 的自助密码更改机制或针对本地目录更改其密码。 随后这些更改会自动同步，并出现在托管域中。

## <a name="lift-and-shift-on-premises-applications-that-use-ldap-read-to-access-the-directory"></a>直接迁移使用 LDAP 读取访问目录的本地应用程序

与上一个示例场景一样，我们假设大约十年前，Contoso 开发了一个本地业务线 (LOB) 应用程序。 此应用程序可识别目录，并旨在使用 LDAP 从 AD DS 读取有关用户的信息/属性。 该应用程序不会修改属性，也不会在目录中写入数据。

Contoso 想要将此应用程序迁移到 Azure，并淘汰目前托管此应用程序的已过时本地硬件。 无法重新编写该应用程序，让其使用现代目录 API，例如基于 REST 的 Microsoft 图形 API。 直接迁移选项可以满足需要，因为它可以在不修改代码或重新编写应用程序的情况下，将应用程序迁移到云中运行。

为对这种场景提供帮助，Azure AD DS 允许应用程序针对托管域执行 LDAP 读取，以获取所需的属性信息。 无需重新编写应用程序，通过直接迁移到 Azure，用户可以继续使用该应用，而不会意识到其运行位置发生了变化。

### <a name="deployment-notes"></a>部署注意事项

以下部署注意事项适用于此示例用例：

* 确保应用程序无需在目录中修改/写入数据。 不支持对托管域进行 LDAP 写入访问。
* 确保应用程序不需要自定义/扩展的 Active Directory 架构。 Azure AD DS 不支持架构扩展。

## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure"></a>将本地服务或守护程序应用程序迁移到 Azure

某些应用程序包含多个层，其中有一个层需要对后端层（如数据库）执行经身份验证的调用。 在这些情况下，通常会使用 AD 服务帐户。 将应用程序直接迁移到 Azure 时，通过 Azure AD DS，你能够以相同的方式继续使用服务帐户。 可以选择使用已从本地目录同步到 Azure AD 的相同服务帐户，也可以创建自定义 OU，然后在该 OU 中创建一个单独的服务帐户。 使用其中任意一种方法，应用程序都将继续以相同的方式对其他层和服务进行经身份验证的调用。

![使用 WIA 的服务帐户](./media/active-directory-domain-services-scenarios/wia-service-account.png)

在此示例场景中，Contoso 有一个定制的软件保管库应用程序，其中包含 Web 前端、SQL 服务器和后端 FTP 服务器。 使用服务帐户的 Windows 集成身份验证在 FTP 服务器中对 Web 前端进行身份验证。 该 Web 前端设置为以服务帐户的身份运行。 后端服务器配置为通过服务帐户授权访问 Web 前端。 Contoso 不希望在云中部署和管理自己的域控制器 VM，从而将此应用程序迁移到 Azure。

对于此场景，托管 Web 前端的服务器、SQL 服务器和 FTP 服务器的服务器可以迁移到 Azure VM 并加入托管域。 然后，这些 VM 可以使用其本地目录中的相同服务帐户，以对应用进行身份验证，这将通过 Azure AD 使用 Azure AD Connect 进行同步。

### <a name="deployment-notes"></a>部署注意事项

以下部署注意事项适用于此示例用例：

* 确保应用程序使用用户名和密码进行身份验证。 Azure AD DS 不支持基于证书或智能卡的身份验证。
* 无法直接针对托管域更改密码。 最终用户可以使用 Azure AD 的自助密码更改机制或针对本地目录更改其密码。 随后这些更改会自动同步，并出现在托管域中。

## <a name="windows-server-remote-desktop-services-deployments-in-azure"></a>Azure 中的 Windows Server 远程桌面服务部署

可以使用 Azure AD DS 向 Azure 中部署的远程桌面服务器提供托管域服务。 有关此部署场景的详细信息，请参阅[如何将 Azure AD 域服务与 RDS 部署集成][windows-rds]。

## <a name="next-steps"></a>后续步骤

若要开始使用，请[创建并配置 Azure Active Directory 域服务托管域][tutorial-create-instance]

<!-- INTERNAL LINKS -->
[tutorial-create-instance]: tutorial-create-instance.md

<!-- EXTERNAL LINKS -->
[windows-rds]: https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds

