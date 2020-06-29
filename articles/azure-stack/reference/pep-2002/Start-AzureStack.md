---
title: Azure Stack Hub 的 Start-AzureStack 特权终结点
description: PowerShell Azure Stack 特权终结点 - Start-AzureStack 参考
author: WenJason
ms.topic: reference
origin.date: 04/27/2020
ms.date: 06/22/2020
ms.author: v-jay
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: 8a767b321467327b5b6807b51768fae2fc95676e
ms.sourcegitcommit: d86e169edf5affd28a1c1a4476d72b01a7fb421d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85096810"
---
# <a name="start-azurestack"></a>Start-AzureStack

## <a name="synopsis"></a>摘要
启动所有 Azure Stack Hub 服务。

## <a name="syntax"></a>语法

```
Start-AzureStack [[-TimeoutInSecs] <Object>] [-AsJob]
```

## <a name="description"></a>说明
启动所有 Azure Stack Hub 服务。

## <a name="examples"></a>示例

### <a name="example-1"></a>示例 1
```powershell
PS C:\> 
```



## <a name="parameters"></a>parameters

### <a name="-timeoutinsecs"></a>-TimeoutInSecs
最长时间，超过后即停止执行。

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: 2400
Accept pipeline input: False
Accept wildcard characters: False
```

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