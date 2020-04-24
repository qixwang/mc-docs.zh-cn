---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 02/01/2019
ms.date: 03/04/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 8f6539c32a1ffc85fa54b5a7c803cfc35f08bcb9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63821030"
---
使用提升的权限打开 PowerShell 控制台。



可以在本地运行 Azure PowerShell，请连接到 Azure 帐户。 Connect-AzureRmAccount cmdlet 会提示输入凭据  。 进行身份验证后，它会下载帐户设置，以便 Azure PowerShell 可以使用这些设置。

```azurepowershell
Connect-AzAccount -Environment AzureChinaCloud
```

如果有多个订阅，请获取 Azure 订阅的列表。

```azurepowershell
Get-AzSubscription
```

指定要使用的订阅。

```azurepowershell
Select-AzSubscription -SubscriptionName "Name of subscription"
```
