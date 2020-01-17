---
title: Azure Stack SQL 资源提供程序 1.1.47.0 发行说明 | Microsoft Docs
description: 了解 Azure Stack SQL 资源提供程序最新更新版的信息，包括任何已知问题和下载位置。
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
ms.openlocfilehash: 13de71f146548714d898c3f4d4c93a92f3f5d992
ms.sourcegitcommit: 166549d64bbe28b28819d6046c93ee041f1d3bd7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75737958"
---
# <a name="sql-resource-provider-11470-release-notes"></a>SQL 资源提供程序 1.1.47.0 发行说明

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

本发行说明介绍 SQL 资源提供程序 1.1.47.0 版中的改进和已知问题。

## <a name="build-reference"></a>内部版本参考
下载 SQL 资源提供程序二进制文件，然后运行自解压程序，将内容解压缩到一个临时目录。 资源提供程序有一个相应的 Azure Stack 最低内部版本。 下面列出了安装此 SQL 资源提供程序版本所需的最低 Azure Stack 发行版：

> |最低 Azure Stack 版本|SQL 资源提供程序版本|
> |-----|-----|
> |版本 1910 (1.1910.0.58)|[SQL RP 版本 1.1.47.0](https://aka.ms/azurestacksqlrp11470)|  
> |     |     |

> [!IMPORTANT]
> 在部署最新版本的 SQL 资源提供程序之前，请先将支持的最低 Azure Stack 更新版应用到 Azure Stack 集成系统，或部署最新的 Azure Stack 开发工具包 (ASDK)。

## <a name="new-features-and-fixes"></a>新功能和修复

此版本的 Azure Stack SQL 资源提供程序是一个修补程序版本，可使资源提供程序与版本 1910 更新中的某些最新门户更改兼容，没有任何新功能。

它还支持当前最新的 Azure Stack API 版本配置文件 2019-03-01-hybrid 和 Azure Stack PowerShell 模块 1.8.0。 因此，部署和更新期间无需安装模块的特定历史版本。

Azure Stack 升级到 1910 更新后，请按照资源提供程序更新过程应用 SQL 资源提供程序修补程序 1.1.47.0。 它将帮助解决管理员门户中的已知问题，在该门户中，SQL 资源提供程序中的容量监视一直保持加载状态。

## <a name="known-issues"></a>已知问题

无。

## <a name="next-steps"></a>后续步骤
[详细了解 SQL 资源提供程序](azure-stack-sql-resource-provider.md)。

[准备部署 SQL 资源提供程序](azure-stack-sql-resource-provider-deploy.md#prerequisites)。

[从旧版升级 SQL 资源提供程序](azure-stack-sql-resource-provider-update.md)。 
