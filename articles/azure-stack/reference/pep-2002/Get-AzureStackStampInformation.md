---
title: Azure Stack Hub 的 Get-AzureStackStampInformation 特权终结点
description: PowerShell Azure Stack 特权终结点 - Get-AzureStackStampInformation 参考
author: WenJason
ms.topic: reference
origin.date: 04/27/2020
ms.date: 06/22/2020
ms.author: v-jay
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: 5bf66017f93453258109b156749146616248156c
ms.sourcegitcommit: d86e169edf5affd28a1c1a4476d72b01a7fb421d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85096921"
---
# <a name="get-azurestackstampinformation"></a>Get-AzureStackStampInformation

## <a name="synopsis"></a>摘要
获取标记信息。

## <a name="syntax"></a>语法

```
Get-AzureStackStampInformation [-AsJob]
```

## <a name="description"></a>说明
导入 ECE 客户端模块，然后对 ECE 服务上的云角色启动操作计划。

## <a name="examples"></a>示例

### <a name="example-1"></a>示例 1
```
Get-AzureStackStampInformation
```

## <a name="parameters"></a>parameters

### <a name="-asjob"></a>-AsJob


```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```


## <a name="next-steps"></a>后续步骤

有关如何访问和使用特权终结点的信息，请参阅[使用 Azure Stack Hub 中的特权终结点](/azure-stack/operator/azure-stack-privileged-endpoint)。