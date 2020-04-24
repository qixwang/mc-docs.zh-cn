---
title: 设置 gMSA 用于 Azure Service Fabric 容器服务
description: 现在了解如何为在 Azure Service Fabric 中运行的容器设置组托管服务帐户 (gMSA)。
ms.topic: conceptual
origin.date: 03/20/2019
ms.date: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: d4f433152590230dfb18b3db7a5a9d6e571898a6
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77540011"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>设置 gMSA 用于在 Service Fabric 上运行的 Windows 容器

若要设置 gMSA（组托管服务帐户），需将凭据规范文件 (`credspec`) 放置在群集的所有节点上。 可以在所有节点上使用 VM 扩展复制文件。  `credspec` 文件必须包含 gMSA 帐户信息。 有关 `credspec` 文件的详细信息，请参阅[创建凭据规范](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec)。在应用程序清单中指定凭据规范和 `Hostname` 标记。 `Hostname` 标记必须匹配容器在其下运行的 gMSA 帐户名称。  `Hostname` 标记使容器本身能够使用 Kerberos 身份验证向域中的其他服务进行身份验证。  应用程序清单中用于指定 `Hostname` 和 `credspec` 的示例在以下代码片段中显示：

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
有关后续步骤，请阅读以下文章：

* [将 Windows 容器部署到 Windows Server 2016 上的 Service Fabric](service-fabric-get-started-containers.md)
* [将 Docker 容器部署到 Linux 上的 Service Fabric](service-fabric-get-started-containers-linux.md)

<!-- Update_Description: update meta properties -->