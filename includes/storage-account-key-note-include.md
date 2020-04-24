---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 12/12/2019
ms.date: 01/06/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 1e19b65201ae75d6b0843d19840a8881e71b349b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75623699"
---
## <a name="protect-your-access-keys"></a>保护访问密钥

存储帐户访问密钥类似于存储帐户的根密码。 始终要小心保护访问密钥。 使用 Azure 密钥保管库安全地管理和轮换密钥。 避免将访问密钥分发给其他用户、对其进行硬编码或将其以纯文本形式保存在其他人可以访问的任何位置。 如果你认为访问密钥可能已泄露，请轮换密钥。

如果可能，请使用 Azure Active Directory (Azure AD) 而不是共享密钥来为针对 Blob 和队列存储的请求授权。 Azure AD 通过共享密钥提供更高的安全性和易用性。 有关使用 Azure AD 授权访问数据的详细信息，请参阅[使用 Azure Active Directory 授权访问 Azure Blob 和队列](../articles/storage/common/storage-auth-aad.md)。
