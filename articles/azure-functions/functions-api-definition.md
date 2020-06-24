---
title: Azure Functions 中的 OpenAPI 元数据 | Microsoft Docs
description: Azure Functions 中的 OpenAPI 支持概述
services: functions
author: alexkarcher-msft
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
origin.date: 03/23/2017
ms.date: 06/05/2020
ms.author: v-junlch
ms.openlocfilehash: 0db4d818552949bdd6000f1ba6614c360da0223c
ms.sourcegitcommit: f1a76ee3242698123a3d77f44c860db040b48f70
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84563689"
---
# <a name="openapi-20-metadata-support-in-azure-functions-preview"></a>Azure Functions 中的 OpenAPI 2.0 元数据支持（预览版）
Azure Functions 中的 OpenAPI 2.0（以前称为 Swagger）元数据支持一项预览版功能，可用于在 Function App 中编写 OpenAPI 2.0 定义。 随后可使用 Function App 托管该文件。

> [!IMPORTANT]
> OpenAPI 预览功能目前仅在 1.x 运行时可用。 若要了解如何创建 1.x 函数应用，[可参阅此处](./functions-versions.md#creating-1x-apps)。

通过 [OpenAPI 元数据](https://swagger.io/)，大量其他软件可使用托管 REST API 的函数。 此软件包括 Microsoft 产品/服务（如 [Azure 应用服务的 API 应用功能](../app-service/overview.md)）、第三方开发人员工具（如 [Postman](https://www.getpostman.com/docs/importing_swagger)），以及[更多包](https://swagger.io/tools/)。

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="enable-openapi-definition-support"></a><a name="enable"></a>启用 OpenAPI 定义支持
可在函数应用的“平台功能”的“API 定义”页中配置所有 OpenAPI 设置**** ****。

> [!NOTE]
> beta 版本运行时当前不支持函数 API 定义功能。

要生成托管的 OpenAPI 定义和快速入门定义，请将“API 定义源”设置为“函数(预览版)”**** ****。 外部 URL 允许函数使用托管在其他位置的 OpenAPI 定义。****

## <a name="generate-a-swagger-skeleton-from-your-functions-metadata"></a><a name="generate-definition"></a>通过函数元数据生成 Swagger 框架
模板可帮助你开始编写第一个 OpenAPI 定义。 定义模板功能通过使用 function.json 文件中的所有元数据，为每个 HTTP 触发器函数创建稀疏的 OpenAPI 定义。 将需要按 [OpenAPI 规范](https://swagger.io/specification/)填写 API 详细信息，如请求和响应模板。

### <a name="available-templates"></a><a name="templates"></a>可用模板

|名称| 说明 |
|:-----|:-----|
|生成的定义|一个 OpenAPI 定义，内含可从函数的现有元数据中推断出的大量信息。|

### <a name="included-metadata-in-the-generated-definition"></a><a name="quickstart-details"></a>生成的定义中包含的元数据

下表显示了 Azure 门户设置，并显示 function.json 在映射到生成的 Swagger 框架时包含的相应数据。

|Swagger.json|门户 UI|Function.json|
|:----|:-----|:-----|
|[主机](https://swagger.io/specification/#fixed-fields-15)|“Function App 设置” > “应用服务设置” > “概述” > “URL”**** **** **** ****|*不存在*
|[路径](https://swagger.io/specification/#paths-object-29)|“集成” > “选择 HTTP 方法”**** ****|绑定：路由
|[路径项](https://swagger.io/specification/#path-item-object-32)|“集成” > “路由模板”**** ****|绑定：方法
|[安全性](https://swagger.io/specification/#security-scheme-object-112)|**“键”**|*不存在*|

> [!NOTE]
> x-ms-summary 扩展在逻辑应用中提供显示名称。
>

## <a name="next-steps"></a>后续步骤
* [Azure Functions GitHub 存储库](https://github.com/Azure/Azure-Functions/)。 查看 Functions 存储库，针对 API 定义支持预览提供反馈。 为要更新的任何内容制作一个 GitHub 发布。
* [Azure Functions 开发人员参考](functions-reference.md)。 了解如何编码函数以及如何定义触发器和绑定。

<!-- Update_Description: link update -->