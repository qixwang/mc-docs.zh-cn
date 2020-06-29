---
title: Azure Stack Hub 的 Remove-GraphApplication 特权终结点
description: PowerShell Azure Stack 特权终结点参考 - Remove-GraphApplication
author: WenJason
ms.topic: reference
origin.date: 04/27/2020
ms.date: 06/22/2020
ms.author: v-jay
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: 981a1de574b54b713ae3d8b229a28b6dd4adc42c
ms.sourcegitcommit: d86e169edf5affd28a1c1a4476d72b01a7fb421d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85096973"
---
# <a name="remove-graphapplication"></a>Remove-GraphApplication

## <a name="synopsis"></a>摘要
Remove-GraphApplication 是一个包装器函数，用于对 AD FS 调用 AD FS Graph cmdlet。

## <a name="syntax"></a>语法

```
Remove-GraphApplication [[-ApplicationIdentifier] <Object>] [-AsJob]
```

## <a name="description"></a>说明
对 AD FS 调用 Remove-GraphApplicationGroup 可删除 AD FS 计算机上指定的应用程序。

## <a name="examples"></a>示例

### <a name="example-1"></a>示例 1
```
Remove-GraphApplication -ApplicationIdentifier "Application-Identifier-123456"
```

## <a name="parameters"></a>parameters

### <a name="-applicationidentifier"></a>-ApplicationIdentifier
要删除的应用程序的标识符

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
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

若要了解如何访问和使用特权终结点，请参阅[使用 Azure Stack Hub 中的特权终结点](/azure-stack/operator/azure-stack-privileged-endpoint)。