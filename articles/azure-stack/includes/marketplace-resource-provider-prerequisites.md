---
author: WenJason
ms.author: v-jay
ms.service: azure-stack
ms.topic: include
origin.date: 12/09/2019
ms.date: 05/18/2020
ms.reviewer: bryanla
ms.lastreviewed: 12/09/2019
ms.openlocfilehash: a28636beedd2cd01cb21b74843368d622b8c04b9
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83423036"
---
如果已从 Azure Stack Hub 市场安装了资源提供程序，则可能已完成了常见的先决条件操作，可以跳过此部分。 否则，请先完成以下先决条件操作： 

1. [向 Azure 注册 Azure Stack Hub 实例](../operator/azure-stack-registration.md)（如果尚未这样做）。 此步骤是必需的，因为需要连接到 Azure 并将项目从 Azure 下载到市场。

2. 如果不熟悉 Azure Stack Hub 管理员门户的“市场管理”功能，请参阅[从 Azure 下载市场项并将其发布到 Azure Stack Hub](../operator/azure-stack-download-azure-marketplace-item.md)。 此文逐步讲解如何将项目从 Azure 下载到 Azure Stack Hub 市场。 它涵盖了联网场景和离线场景。 如果 Azure Stack Hub 实例已断开连接或部分连接，则在准备安装时还需要完成其他先决条件操作。

3. 更新 Azure Active Directory (Azure AD) 主目录。 从内部版本 1910 开始，必须使用新的部署资源提供程序 (DRP) 应用程序注册主目录租户。 此应用可以让 DRP 成功创建和注册资源提供程序。 如果此步骤未完成，则资源提供程序安装会失败。 

   - 将 Azure Stack Hub 实例成功更新到 1910 或更高版本后，请按照[克隆/下载 Azure Stack Hub 工具的说明](../operator/azure-stack-powershell-download.md)一文进行操作。 
   - 然后，请按照[更新 Azure Stack Hub Azure AD 主目录（在安装更新或新的资源提供程序之后）的说明](https://github.com/Azure/AzureStack-Tools/tree/master/Identity#updating-the-azure-stack-aad-home-directory-after-installing-updates-or-new-resource-providers)一文进行操作。 