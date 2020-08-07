---
title: current_principal_details() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 current_principal_details()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 11/08/2019
ms.date: 07/31/2020
ms.openlocfilehash: f0019451b9f95c8145db440de38f688a72c64ef2
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509247"
---
# <a name="current_principal_details"></a>current_principal_details()

返回运行查询的主体的详细信息。

## <a name="syntax"></a>语法

`current_principal_details()`

## <a name="returns"></a>返回

作为 `dynamic` 的当前主体的详细信息。

## <a name="example"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
print d=current_principal_details()
```

|d|
|---|
|{<br>  "UserPrincipalName": "user@fabrikam.com",<br>  "IdentityProvider": "https://sts.windows.net",<br>  "Authority":"72f988bf-86f1-41af-91ab-2d7cd011db47",<br>  "Mfa":"True",<br>  "Type":"AadUser",<br>  "DisplayName":"James Smith (upn: user@fabrikam.com)",<br>  "ObjectId":"346e950e-4a62-42bf-96f5-4cf4eac3f11e",<br>  "FQN": null,<br>  "Notes": null<br>}|
