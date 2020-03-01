---
title: 访问 Azure 媒体服务 API - Azure CLI | Microsoft Docs
description: 按照此操作指南文章中的步骤访问 Azure 媒体服务 API。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
origin.date: 05/15/2019
ms.date: 02/24/2020
ms.author: v-jay
ms.openlocfilehash: a644b9b89ecb521350d17dd2c25765b13b719048
ms.sourcegitcommit: f5bc5bf51a4ba589c94c390716fc5761024ff353
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77494575"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>使用 Azure CLI 访问 Azure 媒体服务 API
 
若要使用 Azure AD 服务主体身份验证连接到 Azure 媒体服务 API，应用程序需要请求具有以下参数的 Azure AD 令牌：

* Azure AD 租户终结点
* 媒体服务资源 URI
* REST 媒体服务的资源 URI
* Azure AD 应用程序值：客户端 ID 和客户端密码

> [!IMPORTANT]
> 有关详细说明，请参阅[访问媒体服务 v3 API](media-services-apis-overview.md#accessing-the-azure-media-services-api)。

本文介绍如何使用 Azure CLI 创建 Azure AD 应用程序和服务主体，以及获取访问 Azure 媒体服务资源所需的值。

## <a name="prerequisites"></a>必备条件 

[创建媒体服务帐户](create-account-cli-how-to.md)。

请务必记住用于资源组名称和媒体服务帐户名称的值。
 
[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="see-also"></a>另请参阅

- [缩放媒体预留单位 - CLI](media-reserved-units-cli-how-to.md)
- [创建媒体服务帐户 - CLI](create-account-cli-how-to.md) 
- [重置帐户凭据 - CLI](cli-reset-account-credentials.md)
- [创建资产 - CLI](cli-create-asset.md)
- [上传文件 - CLI](cli-upload-file-asset.md)
- [创建转换 - CLI](cli-create-transform.md)
- [对自定义转换进行编码 - CLI](custom-preset-cli-howto.md)
- [创建作业 - CLI](cli-create-jobs.md)
- [发布资产 - CLI](cli-publish-asset.md)
- [筛选器 - CLI](filters-dynamic-manifest-cli-howto.md)
- [Azure CLI](/cli/ams?view=azure-cli-latest)

## <a name="next-steps"></a>后续步骤

要从中流式传输内容的流式处理终结点必须处于“正在运行”状态。 以下 CLI 命令将启动默认的流式处理终结点：

`az ams streaming-endpoint start -n default -a <amsaccount> -g <amsResourceGroup>`

