---
author: IEvangelist
ms.author: v-tawe
origin.date: 10/02/2019
ms.date: 01/13/2020
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 089345f994d6c00a71d442108fda0fb280a8b888
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75631673"
---
此容器具有以下配置设置：

|必选|设置|目的|
|--|--|--|
|是|[ApiKey](#apikey-configuration-setting)|跟踪账单信息。|
|是|[Billing](#billing-configuration-setting)|指定 Azure 上服务资源的终结点 URI。|
|是|[Eula](#eula-setting)| 表示已接受容器的许可条款。|
|否|[Fluentd](#fluentd-settings)|将日志和（可选）指标数据写入 Fluentd 服务器。|
|否|HTTP 代理|配置 HTTP 代理以发出出站请求。|
|否|[Logging](#logging-settings)|为容器提供 ASP.NET Core 日志记录支持。 |
|否|[Mounts](#mount-settings)|从主计算机读取数据并将其写入到容器，以及从容器读回数据并将其写回到主计算机。|
