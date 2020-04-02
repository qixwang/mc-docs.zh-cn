---
title: 使用 PowerShell 查看托管标识的服务主体 - Azure AD
description: 使用 PowerShell 查看托管标识的服务主体的分步说明。
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/24/2020
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2209517a1e11149e96ca0a680fefddb3266363d7
ms.sourcegitcommit: 6568c59433d7e80ab06e9fe76d4791f761ed6775
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2020
ms.locfileid: "80243054"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>使用 PowerShell 查看托管标识的服务主体

Azure 资源的托管标识在 Azure Active Directory 中为 Azure 服务提供了一个自动托管标识。 此标识可用于通过支持 Azure AD 身份验证的任何服务的身份验证，这样就无需在代码中插入凭据了。 

本文介绍如何使用 PowerShell 查看托管标识的服务主体。

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>先决条件

- 如果不熟悉 Azure 资源的托管标识，请查阅[概述部分](overview.md)。
- 如果还没有 Azure 帐户，请注册[试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。
- 在[虚拟机](/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity)或[应用程序](/app-service/overview-managed-identity#add-a-system-assigned-identity)上启用系统分配的标识。
- 安装最新版本的 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)

## <a name="view-the-service-principal"></a>查看服务主体

以下命令演示了如何查看启用了系统分配的标识的 VM 或应用程序的服务主体。 将 `<VM or application name>` 替换为自己的值。

```powershell
Get-AzADServicePrincipal -DisplayName <VM or application name>
```

## <a name="next-steps"></a>后续步骤

有关使用 PowerShell 查看 Azure AD 服务主体的详细信息，请参阅 [Get-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/get-azadserviceprincipal)。



