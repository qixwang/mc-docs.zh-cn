---
title: Azure Key Vault 将保管库移到不同的区域 | Microsoft Docs
description: 有关将密钥保管库移到不同区域的指南。
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
origin.date: 04/24/2020
ms.date: 06/02/2020
ms.author: v-tawe
Customer intent: As a key vault administrator, I want to move my vault to another region.
ms.openlocfilehash: db58fc572f9a4455f9bd24286e4e5dea91e78bc0
ms.sourcegitcommit: 9811bf312e0d037cb530eb16c8d85238fd276949
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84275718"
---
# <a name="moving-an-azure-key-vault-across-regions"></a>在区域之间移动 Azure Key Vault

## <a name="overview"></a>概述

Key Vault 不支持允许将密钥保管库移到另一个区域的资源移动操作。 如果你的业务需要将密钥保管库移到另一个区域，可参阅本文将要介绍的解决方案。 每个选项都有局限性，在生产环境中尝试这些解决方案之前，了解这些解决方案可能会产生的结果至关重要。

在需要将密钥保管库移到另一个区域的情况下，解决方案是在所需的区域中创建一个新的密钥保管库，并手动将现有密钥保管库中的每个机密复制到新的密钥保管库。 此操作可以通过下面列出的任一方式完成：

## <a name="design-considerations"></a>设计注意事项

* Key Vault 名称是全局独一无二的。 你将不能重复使用相同的保管库名称。

* 你将需要在新的密钥保管库中重新配置访问策略和网络配置设置。

* 你将需要在新的密钥保管库中重新配置软删除和清除保护。

* 备份和还原操作不会保留自动轮换设置，你可能需要重新配置这些设置。

## <a name="option-1---use-the-key-vault-backup-and-restore-commands"></a>选项 1 - 使用密钥保管库的备份和还原命令

可以使用备份命令来备份保管库中的每个机密、密钥和证书。 机密将以加密 blob 的形式下载。 然后，可以将该 blob 还原到新的密钥保管库中。 下面的链接中记录了这些命令。

[Azure Key Vault 命令](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-6.13.0#key_vault)

### <a name="limitations"></a>限制

* 如果在一个地理位置备份密钥保管库，则不能将其还原到另一个地理位置。

* 备份命令会备份每个机密的所有版本。 如果你的机密有大量旧版本（大于 10 个），则请求可能会超过允许的最大请求大小，操作可能会失败。

## <a name="option-2---manually-download-and-upload-secrets"></a>选项 2 - 手动下载和上传机密

可以手动下载某些机密类型。 例如，可以 .pfx 文件的形式下载证书。 此选项可消除某些机密类型（如证书）的地理限制。 可以将 .pfx 文件上传到任何区域中的任何密钥保管库。 将以非密码保护格式下载机密。 你的机密在移动过程中离开 Key Vault 后，你将负责保护这些机密。
