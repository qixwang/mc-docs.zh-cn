---
title: Azure Analysis Services 资源和对象限制 | Azure
description: 本文介绍 Azure Analysis Services 服务器的资源和对象限制。
author: rockboyfor
ms.service: azure-analysis-services
ms.topic: conceptual
origin.date: 10/30/2019
ms.date: 11/25/2019
ms.author: v-yeche
ms.reviewer: minewiskan
ms.openlocfilehash: 321d62b5b369f7c8da4dca4c148c82cf6880c894
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74203636"
---
<!--Verify sucessfull-->
# <a name="analysis-services-resource-and-object-limits"></a>Azure Analysis Services 资源和对象限制

本文介绍资源和模型对象限制。

## <a name="tier-limits"></a>层限制

有关基本层和标准层的 QPU 和内存限制，请参阅 [Azure Analysis Services 定价页](https://www.azure.cn/pricing/details/analysis-services/)。

<!--Not Available on developer,-->
<!--Not Available on ### Developer tier-->
<!--Notice: Standared tier from S0,S1,S2,S4 in Mooncake-->

## <a name="object-limits"></a>对象限制

以下限制是理论限制。 性能会随以下数值的降低而降低。

|Object|最大大小/最大数量|  
|------------|----------------------------|  
|实例中的数据库|16,000|  
|数据库中表和列的总数|16,000|  
|表中的行|无限制<br /><br /> 警告：具有限制：表中单列的非重复值不能超过 1,999,999,997 个  。|  
|表中的层次结构|15,999|  
|层次结构中的级别数|15,999|  
|关系|8,000|  
|所有表中的键列数|15,999|  
|表中的度量值|2^31-1 = 2,147,483,647|  
|查询返回的单元|2^31-1 = 2,147,483,647|  
|源查询的记录大小|64 K|  
|对象名的长度|512 个字符|

<!-- Update_Description: wording update -->
