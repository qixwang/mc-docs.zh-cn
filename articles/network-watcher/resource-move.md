---
title: 移动 Azure 网络观察程序资源 | Azure
description: 跨区域移动 Azure 网络观察程序资源
services: network-watcher
documentationcenter: na
author: rockboyfor
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 07/27/2020
ms.date: 08/10/2020
ms.testscope: no
ms.testdate: ''
ms.author: v-yeche
ms.openlocfilehash: aed77dcd3bc101c22c267806fea2cdbe9347df54
ms.sourcegitcommit: 3eadca6821ef679d8ac6ca2dc46d6a13aac211cd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87548096"
---
<!--Verified successfully-->
<!--Charactor contents only-->

# <a name="moving-azure-network-watcher-resources-across-regions"></a>跨区域移动 Azure 网络观察程序资源

## <a name="moving-the-network-watcher-resource"></a>移动网络观察程序资源
网络观察程序资源代表网络观察程序的后端服务，由 Azure 完全托管。 客户无需管理它。 此资源不支持移动操作。

## <a name="moving-child-resources-of-network-watcher"></a>移动网络观察程序的子资源
对于 `*networkWatcher*` 资源类型的任何子资源，当前都不支持跨区域移动资源。

## <a name="next-steps"></a>后续步骤
* 阅读[网络观察程序概述](/network-watcher/network-watcher-monitoring-overview)
* 查看[网络观察程序常见问题解答](/network-watcher/frequently-asked-questions)

<!-- Update_Description: new article about resource move -->
<!--NEW.date: 08/10/2020-->