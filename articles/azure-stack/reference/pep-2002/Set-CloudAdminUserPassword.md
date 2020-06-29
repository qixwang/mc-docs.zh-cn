---
title: Azure Stack Hub 的 Set-CloudAdminUserPassword 特权终结点
description: PowerShell Azure Stack 特权终结点 - Set-CloudAdminUserPassword 参考信息
author: WenJason
ms.topic: reference
origin.date: 04/27/2020
ms.date: 06/22/2020
ms.author: v-jay
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: 5f628a874fdf225fa72616fdb25076dd9aa71dee
ms.sourcegitcommit: d86e169edf5affd28a1c1a4476d72b01a7fb421d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85096824"
---
# <a name="set-cloudadminuserpassword"></a>Set-CloudAdminUserPassword

## <a name="syntax"></a>语法

```
Set-CloudAdminUserPassword [[-UserName] <Object>] [[-CurrentPassword] <Object>] [[-NewPassword] <Object>]
 [-AsJob]
```

## <a name="parameters"></a>parameters

### <a name="-currentpassword"></a>-CurrentPassword
 

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

### <a name="-newpassword"></a>-NewPassword
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-username"></a>-UserName
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
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

有关如何访问和使用特权终结点的信息，请参阅[使用 Azure Stack Hub 中的特权终结点](/azure-stack/operator/azure-stack-privileged-endpoint)。