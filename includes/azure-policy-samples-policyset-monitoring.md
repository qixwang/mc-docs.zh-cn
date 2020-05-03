---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
origin.date: 04/03/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 53d8c77f4b5947ef11f92ad067b0640077f69882
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82595255"
---
|名称 |说明 |策略 |版本 |
|---|---|---|---|
|[启用用于虚拟机规模集的 Azure Monitor](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |在指定范围（管理组、订阅或资源组）内启用用于虚拟机规模集的 Azure Monitor。 将 Log Analytics 工作区用作参数。 注意：如果规模集 upgradePolicy 设置为“Manual”，则需要通过对规模集调用升级将扩展应用到集中的所有 VM。 在 CLI 中，这将是 az vmss update-instances。 |6 |1.0.1 |
|[启用用于 VM 的 Azure Monitor](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |在指定范围（管理组、订阅或资源组）内启用用于虚拟机 (VM) 的 Azure Monitor。 将 Log Analytics 工作区用作参数。 |6 |1.0.1 |
