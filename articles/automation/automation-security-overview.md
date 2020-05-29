---
title: Azure 自动化中的身份验证简介
description: 本文概述了 Azure 自动化中自动化帐户的自动化安全性以及可供使用的不同身份验证方法。
keywords: 自动化安全性, 安全的自动化; 自动化身份验证
services: automation
ms.subservice: process-automation
origin.date: 04/23/2020
ms.date: 05/25/2020
ms.topic: conceptual
ms.openlocfilehash: 02181c95719cd1f462655edf5530c72fa199ea7e
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801253"
---
# <a name="introduction-to-authentication-in-azure-automation"></a>Azure 自动化中的身份验证简介

Azure 自动化让可以通过其他云提供程序（如 Amazon Web Services (AWS)）针对 Azure、本地中的资源来自动执行任务。 为了使 Runbook 执行所需操作，Runbook 必须有权使用订阅中所需的最小权限来安全地访问资源。

本文介绍 Azure 自动化支持的各种身份验证方案，以及如何根据需要管理的单个或多个环境来入门。  

## <a name="automation-account-overview"></a>自动化帐户概述

首次启动 Azure 自动化时，必须创建至少一个自动化帐户。 使用自动化帐户，可以将自动化资源（Runbook、资产、配置）与其他自动化帐户中包含的资源隔离。 可以使用自动化帐户将资源隔离到独立的逻辑环境中。 例如，可以在开发环境中使用一个帐户，在生产环境中使用另一个帐户，并在本地环境中使用另一个账户。 Azure 自动化帐户不同于 Azure 帐户或在 Azure 订阅中创建的帐户。

每个自动化帐户的自动化资源都与单个 Azure 区域关联，但自动化帐户可以管理订阅中的所有资源。 在不同区域中创建自动化帐户的主要原因是，策略要求数据和资源隔离到特定的区域。

所有使用 Azure Resource Manager 和 Azure 自动化中的 Azure cmdlet 对资源执行的任务必须使用 Azure Active Directory 组织标识基于凭据的身份验证向 Azure 进行身份验证。 Azure 自动化中的运行方式帐户提供身份验证，以使用 Azure cmdlet 管理 Azure 中的资源。 创建运行方式帐户时，将在 Azure Active Directory (AD) 中创建新的服务主体用户，并在订阅级别向此用户分配“参与者”角色。 对于在 Azure 虚拟机上使用混合 Runbook 辅助角色的 Runbook，可以使用[通过托管标识进行的 runbook 身份验证](automation-hrw-run-runbooks.md#runbook-auth-managed-identities)而不是运行方式帐户向 Azure 资源进行身份验证。

默认情况下，运行方式帐户的服务主体没有读取 Azure AD 的权限。 如果希望添加读取或管理 Azure AD 的权限，需要在“API 权限”下对服务主体授予该权限。 若要了解详细信息，请参阅[添加用于访问 Web API 的权限](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)。

基于角色的访问控制在 Azure Resource Manager 中可用，向 Azure AD 用户帐户和运行方式帐户授予允许的操作，并对该服务主体进行身份验证。 请阅读 [Azure 自动化中基于角色的访问控制](automation-role-based-access-control.md)一文，详细了解如何开发自动化权限管理模型。  

在数据中心的混合 Runbook 辅助角色上运行或针对其他云环境（如 AWS）中的计算服务运行的 Runbook 不能使用通常用于向 Azure 资源进行 Runbook 身份验证的方法。 这是因为这些资源在 Azure 外部运行，因此，它们需要在自动化中定义自己的安全凭据，以便向需要在本地访问的资源进行身份验证。 若要详细了解如何对 runbook 辅助角色进行 runbook 身份验证，请参阅[对混合 Runbook 辅助角色进行 Runbook 身份验证](automation-hrw-run-runbooks.md)。 

## <a name="next-steps"></a>后续步骤

* [从 Azure 门户创建自动化帐户](automation-create-standalone-account.md)。

* [使用 Azure 资源管理器模板创建自动化帐户](automation-create-account-template.md)。
