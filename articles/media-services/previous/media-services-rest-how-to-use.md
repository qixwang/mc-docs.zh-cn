---
title: 媒体服务操作 REST API 概述 | Microsoft 文档
description: 媒体服务操作 REST API 用于在媒体服务帐户中创建作业、资产、直播频道和其他资源。 本文提供了 Azure 媒体服务 v2 REST API 概述。
services: media-services
documentationcenter: ''
author: WenJason
manager: digimobile
editor: ''
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
origin.date: 03/20/2019
ms.date: 02/24/2020
ms.author: v-jay
ms.reviewer: johndeu
ms.openlocfilehash: 4f3687fbebbaef23ccdbd469fc5174ffcb65b9f5
ms.sourcegitcommit: f5bc5bf51a4ba589c94c390716fc5761024ff353
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77494220"
---
# <a name="media-services-operations-rest-api-overview"></a>媒体服务操作 REST API 概述 

> [!NOTE]
> 不会向媒体服务 v2 添加任何新特性或新功能。 <br/>查看最新版本：[媒体服务 v3](/media-services/latest/)。 另请参阅[从 v2 到 v3 的迁移指南](../latest/migrate-from-v2-to-v3.md)

**媒体服务操作 REST** API 用于在媒体服务帐户中创建作业、资产、实时频道和其他资源。 有关详细信息，请参阅 [Media Services Operations REST API reference](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)（媒体服务操作 REST API 参考）。

媒体服务提供一个可接受 JSON 或 atom+pub XML 格式的 REST API。 媒体服务 REST API 需要特定的 HTTP 标头（每个客户端在连接到媒体服务时必须发送这些标头）以及一组可选标头。 以下部分介绍创建请求和接收来自媒体服务的响应时可以使用的标头和 HTTP 谓词。

对媒体服务 REST API 的身份验证通过 Azure Active Directory 身份验证完成，如[通过 Azure AD 身份验证使用 REST 访问 Azure 媒体服务 API](media-services-rest-connect-with-aad.md)一文所述

## <a name="considerations"></a>注意事项

使用 REST 时需考虑下列事项：

* 查询实体时，一次返回的实体数限制为 1000 个，因为公共 REST v2 将查询结果数限制为 1000 个。 需要使用[此 .NET 示例](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities)和[此 REST API 示例](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)中所述的 Skip 和 Take (.NET)/ top (REST)    。 
* 使用 JSON 并指定在请求中使用 **__metadata** 关键字（例如，为了引用某个链接对象）时，必须将 **Accept** 标头设置为 [JSON 详细格式](https://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)（参阅以下示例）。 Odata 并不了解请求中的 __metadata 属性，除非将其设置为 verbose  。  
  
        POST https://media.chinacloudapi.cn/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.19
        Authorization: Bearer <ENCODED JWT TOKEN> 
        Host: media.chinacloudapi.cn
  
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.chinacloudapi.cn/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 

## <a name="standard-http-request-headers-supported-by-media-services"></a>媒体服务支持的标准 HTTP 请求标头
每次调用媒体服务时，都必须在请求中包括一组必需标头，并且还可以根据需要包括一组可选标头。 下表列出了必需的标头：

| 标头 | 类型 | Value |
| --- | --- | --- |
| 授权 |持有者 |持有者是唯一接受的授权机制。 该值还必须包括由 Azure Active Directory 提供的访问令牌。 |
| x-ms-version |小数 |2.17（或最新版本）|
| DataServiceVersion |小数 |3.0 |
| MaxDataServiceVersion |小数 |3.0 |

> [!NOTE]
> 由于媒体服务使用 OData 公开其 REST API，因此任何请求中均应包括 DataServiceVersion 和 MaxDataServiceVersion 标头；但是，如果未包括这些标头，当前媒体服务会假定使用的 DataServiceVersion 值为 3.0。
> 
> 

以下是一组可选标头：

| 标头 | 类型 | Value |
| --- | --- | --- |
| Date |RFC 1123 日期 |请求的时间戳 |
| Accept |内容类型 |响应的请求内容类型，如下所示：<p> -application/json;odata=verbose<p> - application/atom+xml<p> 响应可能会有不同的内容类型，例如 Blob 提取，成功的响应会在其中包含 Blob 流作为有效负载。 |
| Accept-Encoding |Gzip、deflate |GZIP 和 DEFLATE 编码（如果适用）。 注意：对于大型资源，媒体服务可能会忽略此标头并返回未经压缩的数据。 |
| Accept-Language |“en”、“es”等。 |指定响应的首选语言。 |
| Accept-Charset |字符集类型，如“UTF-8” |默认值为 UTF-8。 |
| X-HTTP-Method |HTTP 方法 |允许不支持 HTTP 方法（例如 PUT 或 DELETE）的客户端或防火墙使用这些通过 GET 调用隧道化的方法。 |
| Content-Type |内容类型 |PUT 或 POST 请求中请求正文的内容类型。 |
| client-request-id |String |调用方定义的值，用于标识给定请求。 如果指定，此值会包含在响应消息中，作为一种映射请求的方法。 <p><p>重要说明 <p>值的上限应为 2096b (2k)。 |

## <a name="standard-http-response-headers-supported-by-media-services"></a>媒体服务支持的标准 HTTP 响应标头
下面是可以根据所请求的资源以及要执行的操作返回的一组标头。

| 标头 | 类型 | Value |
| --- | --- | --- |
| request-id |String |当前操作的唯一标识符，由服务生成。 |
| client-request-id |String |调用方在原始请求（如果存在）中指定的标识符。 |
| Date |RFC 1123 日期 |处理请求的日期/时间。 |
| Content-Type |多种多样 |响应正文的内容类型。 |
| Content-Encoding |多种多样 |Gzip 或 deflate（视情况而定）。 |

## <a name="standard-http-verbs-supported-by-media-services"></a>媒体服务支持的标准 HTTP 谓词
下面是在提出 HTTP 请求时可以使用的 HTTP 谓词的完整列表：

| Verb | 说明 |
| --- | --- |
| GET |返回对象的当前值。 |
| POST |根据提供的数据创建对象，或提交命令。 |
| PUT |替换对象，或创建命名对象（如果适用）。 |
| DELETE |删除对象。 |
| MERGE |使用命名的属性更改更新现有对象。 |
| HEAD |为 GET 响应返回对象的元数据。 |

## <a name="discover-and-browse-the-media-services-entity-model"></a>发现和浏览媒体服务实体模型
为了使媒体服务实体易于发现，可使用 $metadata 操作。 使用该操作，可以检索所有有效的实体类型、实体属性、关联、函数、操作等。 将 $metadata 操作添加到媒体服务 REST API 终结点的末尾，可以访问此发现服务。

 /api/$metadata。

如果希望在浏览器中查看元数据，应在 URI 的末尾追加“?api-version=2.x”，或不要在请求中包括 x-ms-version 标头。

## <a name="authenticate-with-media-services-rest-using-azure-active-directory"></a>了解如何使用 Azure Active Directory 通过 媒体服务 REST 进行身份验证

REST API 身份验证是通过 Azure Active Directory (AAD) 实现的。
有关如何从 Azure 门户获取所需媒体服务帐户身份验证详细信息，请参阅[使用 Azure AD 身份验证访问 Azure 媒体服务 API](media-services-use-aad-auth-to-access-ams-api.md)。

有关如何编写可使用 Azure AD 身份验证连接到 REST API 的详细信息，请参阅文章[通过 Azure AD 身份验证使用 REST 访问 Azure 媒体服务 API](media-services-rest-connect-with-aad.md)。

## <a name="next-steps"></a>后续步骤
若要了解如何将 Azure AD 身份验证与媒体服务 REST API 配合使用，请参阅[通过 Azure AD 身份验证使用 REST 访问 Azure 媒体服务 API](media-services-rest-connect-with-aad.md)。

## <a name="media-services-learning-paths"></a>媒体服务学习路径
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

