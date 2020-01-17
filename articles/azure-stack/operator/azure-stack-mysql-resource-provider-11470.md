---
title: Azure Stack MySQL 资源提供程序 1.1.47.0 发行说明 | Microsoft Docs
description: 查看发行说明以了解 Azure Stack MySQL 资源提供程序 1.1.47.0 更新中的新增功能。
services: azure-stack
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 11/26/2019
ms.date: 01/13/2020
ms.author: v-jay
ms.reviewer: xiaofmao
ms.lastreviewed: 11/26/2019
ms.openlocfilehash: e24d52f95b4a7df15236bd005d86655476deaeda
ms.sourcegitcommit: 166549d64bbe28b28819d6046c93ee041f1d3bd7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75737987"
---
# <a name="mysql-resource-provider-11470-release-notes"></a>MySQL 资源提供程序 1.1.47.0 发行说明

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

本发行说明描述 MySQL 资源提供程序 1.1.47.0 版中的改进和已知问题。

## <a name="build-reference"></a>内部版本参考
下载 MySQL 资源提供程序二进制文件，然后运行自解压程序，将内容解压缩到一个临时目录。 资源提供程序有一个相应的 Azure Stack 最低内部版本。 下面列出了安装此 MySQL 资源提供程序版本所需的最低 Azure Stack 发行版：

> |最低 Azure Stack 版本|MySQL 资源提供程序版本|
> |-----|-----|
> |版本 1910 (1.1910.0.58)|[MySQL RP 版本 1.1.47.0](https://aka.ms/azurestackmysqlrp11470)|  
> |     |     |

> [!IMPORTANT]
> 在部署最新版本的 MySQL 资源提供程序之前，请先将支持的最低 Azure Stack 更新版应用到 Azure Stack 集成系统，或部署最新的 Azure Stack 开发工具包 (ASDK)。

## <a name="new-features-and-fixes"></a>新功能和修复

此版本的 Azure Stack MySQL 资源提供程序是一个修补程序版本，可使资源提供程序与 1910 更新中的某些最新门户更改兼容，没有任何新功能。

它还支持当前最新的 Azure Stack API 版本配置文件 2019-03-01-hybrid 和 Azure Stack PowerShell 模块 1.8.0。 因此，部署和更新期间无需安装模块的特定历史版本。

建议在将 Azure Stack 升级到 1910 版本后应用 MySQL 资源提供程序修补程序 1.1.47.0。

## <a name="known-issues"></a>已知问题

无。

## <a name="next-steps"></a>后续步骤
[详细了解 MySQL 资源提供程序](azure-stack-mysql-resource-provider.md)。

[准备部署 MySQL 资源提供程序](azure-stack-mysql-resource-provider-deploy.md#prerequisites)。

[从旧版升级 MySQL 资源提供程序](azure-stack-mysql-resource-provider-update.md)。 
