---
title: 轮换 Azure Stack Hub 上的应用服务的机密和证书
description: 了解如何转换 Azure Stack Hub 上的 Azure 应用服务使用的机密和证书
author: WenJason
ms.topic: article
origin.date: 01/10/2020
ms.date: 02/24/2020
ms.author: v-jay
ms.reviewer: anwestg
ms.lastreviewed: 01/10/2020
ms.openlocfilehash: 15558b3c4e1d8d6f7b2d70acaccd428ee5fb3f4d
ms.sourcegitcommit: afe972418a883551e36ede8deae32ba6528fb8dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77541127"
---
# <a name="rotate-app-service-on-azure-stack-hub-secrets-and-certificates"></a>轮换 Azure Stack Hub 上的应用服务的机密和证书

本文中的说明仅适用于 Azure Stack Hub 上的 Azure 应用服务。  Azure Stack Hub 的集中式机密轮换过程不包括 Azure Stack Hub 上的 Azure 应用服务机密轮换。  操作员可以监视系统中机密的有效性、上次更新日期，以及机密过期之前的剩余时间。

> [!Important]
> 操作员不会在 Azure Stack Hub 仪表板上收到机密过期警报，因为 Azure Stack Hub 上的 Azure 应用服务未与 Azure Stack Hub 警报服务集成。  操作员必须在 Azure Stack Hub 管理员门户中，使用 Azure Stack Hub 上的 Azure 应用服务管理体验定期监视其机密。

本文档包含轮换以下机密的过程：

* Azure Stack Hub 上的 Azure 应用服务中使用的加密密钥；
* Azure Stack Hub 上的 Azure 应用服务用来与宿主数据库和计量数据库交互的数据库连接凭据；
* Azure Stack Hub 上的 Azure 应用服务用来保护终结点的证书；
* Azure Stack Hub 上的 Azure 应用服务基础结构角色的系统凭据。

## <a name="rotate-encryption-keys"></a>轮换加密密钥

若要轮换 Azure Stack Hub 上的 Azure 应用服务中使用的加密密钥，请完成以下步骤：

1. 在 Azure Stack Hub 管理员门户中转到“应用服务管理”体验。

1. 导航到“机密”菜单选项 

1. 单击“加密密钥”部分中的“轮换”按钮 

1. 单击“确定”启动轮换过程。 

1. 加密密钥将会轮换，所有角色实例将会更新。 操作员可以使用“状态”按钮来监视该过程的状态。 

## <a name="rotate-connection-strings"></a>轮换连接字符串

若要更新应用服务宿主数据库和计量数据库的数据库连接字符串凭据，请完成以下步骤：

1. 在 Azure Stack Hub 管理员门户中转到“应用服务管理”体验。

1. 导航到“机密”菜单选项 

1. 单击“连接字符串”部分中的“轮换”按钮 

1. 提供 **SQL SA 用户名**和**密码**，然后单击“确定”启动轮换过程。  

1. 凭据将在整个 Azure 应用服务角色实例中轮换。 操作员可以使用“状态”按钮来监视该过程的状态。 

## <a name="rotate-certificates"></a>轮换证书

若要轮换 Azure Stack Hub 上的 Azure 应用服务中使用的证书，请完成以下步骤：

1. 在 Azure Stack Hub 管理员门户中转到“应用服务管理”体验。

1. 导航到“机密”菜单选项 

1. 单击“证书”部分中的“轮换”按钮 

1. 为要轮换的证书提供**证书文件**和关联的**密码**，然后单击“确定”。 

1. 这些证书将在 Azure Stack Hub 上的 Azure 应用服务角色实例中根据需要轮换。  操作员可以使用“状态”按钮来监视该过程的状态。 

## <a name="rotate-system-credentials"></a>轮换系统凭据

若要轮换 Azure Stack Hub 上的 Azure 应用服务中使用的系统凭据，请完成以下步骤：

1. 在 Azure Stack Hub 管理员门户中转到“应用服务管理”体验。

1. 导航到“机密”菜单选项 

1. 单击“系统凭据”部分中的“轮换”按钮 

1. 选择要轮换系统凭据的“范围”。   操作员可以选择轮换所有角色或单个角色的系统凭据。

1. 指定**本地管理员用户名**、新的**密码**，并确认该**密码**，然后单击“确定” 

1. 这些凭据将在相应的 Azure Stack Hub 上的 Azure 应用服务角色实例中根据需要轮换。  操作员可以使用“状态”按钮来监视该过程的状态。 



