---
title: 标注策略 - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的标注策略。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 04/01/2020
ms.date: 07/01/2020
ms.openlocfilehash: 7702645648d0ff31a2a9375b4eeb929d240954e5
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470369"
---
# <a name="callout-policy"></a>标注策略

Azure 数据资源管理器群集可以在许多不同的方案中与外部服务通信。
群集管理员可以通过更新群集的标注策略来管理用于外部调用的授权域。

标注策略在群集级别进行管理，分为以下类型。
* `kusto` - 控制 Azure 数据资源管理器跨群集查询。
* `sql` - 控制 [SQL 插件](../query/sqlrequestplugin.md)。

* `webapi` - 控制其他外部 Web 调用。
* `sandbox_artifacts` - 控制沙盒插件 ([python](../query/pythonplugin.md) | [R](../query/rplugin.md))。

标注策略由以下内容组成。

* **CalloutType** - 定义标注类型，可以为 `kusto`、`sql` 或 `webapi`
* **CalloutUriRegex** - 指定标注域允许的正则表达式
* **CanCall** - 指示是否允许外部调用标注。

## <a name="predefined-callout-policies"></a>预定义标注策略

下表显示了一组预定义的标注策略，这些策略已在所有 Azure 数据资源管理器群集上进行了预配置，以使标注能够选择服务。

|服务      |云        |指定用途  |允许的域 |
|-------------|-------------|-------------|-------------|
|Kusto |`Mooncake` |跨群集查询 |`^[^.]*\.kusto\.chinacloudapi\.cn$` <br> `^[^.]*\.kustomfa\.chinacloudapi\.cn$` |
|Azure DB |`Mooncake` |SQL 请求 |`^[^.]*\.database\.chinacloudapi\.cn$` <br> `^[^.]*\.databasemfa\.chinacloudapi\.cn$` |

## <a name="control-commands"></a>控制命令

命令需要 [AllDatabasesAdmin](access-control/role-based-authorization.md) 权限。

**显示所有已配置的标注策略**

```kusto
.show cluster policy callout
```

**更改标注策略**

```kusto
.alter cluster policy callout @'[{"CalloutType": "webapi","CalloutUriRegex": "en\\.wikipedia\\.org","CanCall": true}]'
```

**添加一组允许的标注**

```kusto
.alter-merge cluster policy callout @'[{"CalloutType": "webapi","CalloutUriRegex": "en\\.wikipedia\\.org","CanCall": true}, {"CalloutType": "webapi","CalloutUriRegex": "bing\\.com","CanCall": true}]'
```

**删除所有非不可变的标注策略**

```kusto
.delete cluster policy callout
```
