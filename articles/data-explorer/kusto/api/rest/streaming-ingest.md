---
title: 流式引入 HTTP 请求 - Azure 数据资源管理器
description: 本文介绍了 Azure 数据资源管理器中的流式引入 HTTP 请求。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/24/2020
ms.date: 07/01/2020
ms.openlocfilehash: e97e3ae2f6c755726cfdffe556ce42d860a5bb29
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226306"
---
# <a name="streaming-ingestion-http-request"></a>流式引入 HTTP 请求

## <a name="request-verb-and-resource"></a>请求谓词和资源

|操作    |HTTP 谓词|HTTP 资源                                               |
|----------|---------|------------------------------------------------------------|
|引入    |POST     |`/v1/rest/ingest/{database}/{table}?{additional parameters}`|

## <a name="request-parameters"></a>请求参数

| 参数    | 说明                                                                 | 必需/可选 |
|--------------|-----------------------------------------------------------------------------|-------------------|
| `{database}` |   引入请求的目标数据库名称                     |  必须         |
| `{table}`    |   引入请求的目标表名称                        |  必须         |

## <a name="additional-parameters"></a>其他参数

其他参数的格式为 URL 查询 `{name}={value}` 对（以“&”字符分隔）。

| 参数    | 说明                                                                          | 必需/可选   |
|--------------|--------------------------------------------------------------------------------------|---------------------|
|`streamFormat`| 指定请求正文中数据的格式。 值应为以下项之一：`CSV`、`TSV`、`SCsv`、`SOHsv`、`PSV`、`JSON`、`MultiJSON`、`Avro`。 有关详细信息，请参阅[支持的数据格式](../../../ingestion-supported-formats.md)。| 必须 |
|`mappingName` | 表上定义的预创建引入映射的名称。 有关详细信息，请参阅[数据映射](../../management/mappings.md)。 管理表上预创建映射的方法在[此处](../../management/create-ingestion-mapping-command.md)进行了说明。| 可选，但如果 `streamFormat` 为 `JSON`、`MultiJSON` 或 `Avro` 之一，则是必需的|  |
              
例如，若要将 CSV 格式的数据引入到数据库 `Test` 中的表 `Logs`，请使用：

```
POST https://help.kusto.chinacloudapi.cn/v1/rest/ingest/Test/Logs?streamFormat=Csv HTTP/1.1
```

若要使用预创建的映射 `mylogmapping` 引入 JSON 格式的数据，请使用：

```
POST https://help.kusto.chinacloudapi.cn/v1/rest/ingest/Test/Logs?streamFormat=Json&mappingName=mylogmapping HTTP/1.1
```

## <a name="request-headers"></a>请求标头

下表包含用于查询和管理操作的常用标头。

|标准标头   | 说明                                                                               | 必需/可选 | 
|------------------|-------------------------------------------------------------------------------------------|-------------------|
|`Accept`          | 将此值设为 `application/json`。                                                     | 可选          |
|`Accept-Encoding` | 支持的编码为 `gzip` 和 `deflate`。                                             | 可选          | 
|`Authorization`   | 请参阅[身份验证](./authentication.md)。                                                | 必须          |
|`Connection`      | 启用 `Keep-Alive`。                                                                      | 可选          |
|`Content-Length`  | 在已知时指定请求正文长度。                                              | 可选          |
|`Content-Encoding`| 设为 `gzip`，但是正文必须经过 gzip 压缩                                        | 可选          |
|`Expect`          | 设置为 `100-Continue`。                                                                    | 可选          |
|`Host`            | 设为已向其发送请求的域名（如 `help.kusto.chinacloudapi.cn`）。 | 必须          |

下表包含用于查询和管理操作的常用自定义标头。 除非另有说明，否则这些标头仅用于遥测目的，而不会影响功能。

|自定义标头           |说明                                                                           | 必需/可选 |
|------------------------|----------------------------------------------------------------------------------------------------------|
|`x-ms-app`              |发出请求的应用程序的（易记）名称。                            | 可选          |
|`x-ms-user`             |发出请求的用户的（易记）名称。                                   | 可选          |
|`x-ms-user-id`          |与 `x-ms-user` 相同。                                                                  | 可选          |
|`x-ms-client-request-id`|请求的唯一标识符。                                                  | 可选          |
|`x-ms-client-version`   |发出请求的客户端的（易记）版本标识符。 在用于标识请求的场景（例如，取消正在运行的查询）中是必需的。                                                        | 可选/必需  |

## <a name="body"></a>正文

正文是要引入的实际数据。 文本格式应使用 UTF-8 编码。

## <a name="examples"></a>示例

以下示例演示了引入 JSON 内容的 HTTP POST 请求：

```txt
POST https://help.kusto.chinacloudapi.cn/v1/rest/ingest/Test/Logs?streamFormat=Json&mappingName=mylogmapping HTTP/1.1
```

请求标头：

```txt
Authorization: Bearer ...AzureActiveDirectoryAccessToken...
Accept-Encoding: deflate
Accept-Encoding: gzip
Connection: Keep-Alive
Content-Length: 161
Host: help.kusto.chinacloudapi.cn
x-ms-client-request-id: MyApp.Ingest;5c0656b9-37c9-4e3a-a671-5f83e6843fce
x-ms-user-id: alex@contoso.com
x-ms-app: MyApp
```

请求正文：

```txt
{"Timestamp":"2018-11-14 11:34","Level":"Info","EventText":"Nothing Happened"}
{"Timestamp":"2018-11-14 11:35","Level":"Error","EventText":"Something Happened"}
```

以下示例演示了引入相同压缩数据的 HTTP POST 请求。

```txt
POST https://help.kusto.chinacloudapi.cn/v1/rest/ingest/Test/Logs?streamFormat=Json&mappingName=mylogmapping HTTP/1.1
```

请求标头：

```txt
Authorization: Bearer ...AzureActiveDirectoryAccessToken...
Accept-Encoding: deflate
Accept-Encoding: gzip
Connection: Keep-Alive
Content-Length: 116
Content-Encoding: gzip
Host: help.kusto.chinacloudapi.cn
x-ms-client-request-id: MyApp.Ingest;5c0656b9-37c9-4e3a-a671-5f83e6843fce
x-ms-user-id: alex@contoso.com
x-ms-app: MyApp
```

请求正文：

```
... binary data ...
```
