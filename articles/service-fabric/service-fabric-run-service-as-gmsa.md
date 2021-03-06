---
title: 以 gMSA 帐户身份运行 Azure Service Fabric 服务
description: 了解如何在 Service Fabric Windows 独立群集上以组托管服务帐户 (gMSA) 身份运行服务。
author: rockboyfor
ms.topic: how-to
origin.date: 03/29/2018
ms.date: 02/24/2020
ms.author: v-yeche
ms.custom: sfrev
ms.openlocfilehash: 575732f31a5c88fa2fe593e0ffb74e926ce452ce
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77540095"
---
# <a name="run-a-service-as-a-group-managed-service-account"></a>以组托管服务帐户身份运行服务

在 Windows Server 独立群集上，可以使用 *RunAs* 策略以组托管服务帐户 (gMSA) 的身份来运行服务。  默认情况下，Service Fabric 应用程序在运行 `Fabric.exe` 进程的帐户之下运行。 即使在共享托管环境中以不同帐户身份运行应用程序，也可确保运行的应用程序彼此更安全。 使用 gMSA 时，没有密码或加密的密码存储在应用程序清单中。  还可以采用 [Active Directory 用户或组](service-fabric-run-service-as-ad-user-or-group.md)身份运行服务。

以下示例演示如何创建一个名为 *svc-Test$* 的 gMSA 帐户、如何将该托管服务帐户部署到群集节点，以及如何配置用户主体。

> [!NOTE]
> 将 gMSA 与独立 Service Fabric 群集配合使用需要你所在域中的本地 Active Directory（而不是 Azure Active Directory (Azure AD)）。

先决条件：

- 域需要 KDS 根密钥。
- 域中必须至少有一个 Windows Server 2012（或 R2）DC。

1. 让 Active Directory 域管理员使用 `New-ADServiceAccount` cmdlet 创建组托管服务帐户，并确保 `PrincipalsAllowedToRetrieveManagedPassword` 包括所有 Service Fabric 群集节点。 `AccountName`、`DnsHostName` 和 `ServicePrincipalName` 必须是唯一的。

    ```powershell
    New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
    ```

2. 在每个 Service Fabric 群集节点（例如，`SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`）上，安装并测试 gMSA。

    ```powershell
    Add-WindowsFeature RSAT-AD-PowerShell
    Install-AdServiceAccount svc-Test$
    Test-AdServiceAccount svc-Test$
    ```

3. 配置用户主体，并配置 `RunAsPolicy` 以引用[用户](/service-fabric/service-fabric-cluster-fabric-settings#runas)。

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <ServiceManifestImport>
          <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
          <ConfigOverrides />
          <Policies>
              <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
          </Policies>
        </ServiceManifestImport>
      <Principals>
        <Users>
          <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
        </Users>
      </Principals>
    </ApplicationManifest>
    ```

> [!NOTE]
> 如果将 RunAs 策略应用到服务，且服务清单使用 HTTP 协议声明终结点资源，则必须指定 SecurityAccessPolicy  。  有关详细信息，请参阅[为 HTTP 和 HTTPS 终结点分配安全访问策略](service-fabric-assign-policy-to-endpoint.md)。
>

以下文章会指导你完成后续步骤：

- [了解应用程序模型](service-fabric-application-model.md)
- [在服务清单中指定资源](service-fabric-service-manifest-resources.md)
- [部署应用程序](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png

<!-- Update_Description: update meta properties, wording update, update link -->