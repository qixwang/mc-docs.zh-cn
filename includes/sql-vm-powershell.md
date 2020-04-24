---
author: rockboyfor
ms.service: virtual-machines-sql
ms.topic: include
origin.date: 11/25/2018
ms.date: 02/18/2019
ms.author: v-yeche
ms.openlocfilehash: c81c6740d6c85adedfa4a31d195f853008659c58
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "65835781"
---
## <a name="start-your-powershell-session"></a>启动 PowerShell 会话

运行 [**Connect-Az Account**](https://docs.microsoft.com/powershell/module/az.accounts/connect-azmaccount) cmdlet，然后便会看到可输入凭据的登录屏幕。 使用与登录 Azure 门户相同的凭据。

```powershell
Connect-AzAccount -Environment AzureChinaCloud
```

如果有多个订阅，请使用 [**Set-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) cmdlet 选择 PowerShell 会话应使用的订阅。 若要查看当前 PowerShell 会话正在使用哪个订阅，请运行 [**Get-AzContext**](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext)。 若要查看所有订阅，请运行 [**Get-AzSubscription**](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription)。

```powershell
Set-AzContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```

<!-- Update_Description: wording update, update link -->