---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 07/25/2019
ms.date: 09/02/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 7466060cb3f37a7a12a6a143109655caa5011cd7
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70174553"
---
使用共享访问签名 (SAS)，可以授予对存储帐户中容器和 blob 的有限访问权限。 创建 SAS 时，需要指定其约束条件，包括允许客户端访问哪些 Azure 存储资源、它们对这些资源具有哪些权限，以及 SAS 的有效期。

每个 SAS 均使用密钥进行签名。 可通过以下两种方式之一对 SAS 进行签名：

- 使用 Azure Active Directory (Azure AD) 凭据创建的密钥。 使用 Azure AD 凭据签名的 SAS 是用户委托  SAS。
- 使用存储帐户密钥。 服务 SAS 和帐户 SAS 均使用存储帐户密钥进行签名。  
