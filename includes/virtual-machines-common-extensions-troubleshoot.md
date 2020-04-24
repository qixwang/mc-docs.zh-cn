---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 380b5b5c66ab19523ad71832141be5efb4de971b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63850416"
---
## <a name="overview-of-azure-resource-manager-templates"></a>Azure 资源管理器模板概述

> [!NOTE]
> 必须修改从 GitHub 存储库“azure-quickstart-templates”下载或参考的模板，以适应 Azure 中国云环境。 例如，替换某些终结点（将“blob.core.windows.net”替换为“blob.core.chinacloudapi.cn”，将“cloudapp.azure.com”替换为“chinacloudapp.cn”）；必要时更改某些不受支持的 VM 映像、VM 大小、SKU 以及资源提供程序的 API 版本。

Azure 资源管理器模板允许用户通过定义资源之间的依赖关系，使用 JSON 语言以声明方式指定 Azure IaaS 基础结构。

若要详细了解如何创作可使用扩展的模板，请参阅[创作扩展模板](../articles/virtual-machines/windows/template-description.md?toc=%2fvirtual-machines%2fwindows%2ftoc.json)。

本文介绍如何对一些常见的 VM 扩展故障进行故障排除。

<!-- Update_Description: update meta properties, wording update -->