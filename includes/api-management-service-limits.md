---
title: include 文件
description: include 文件
services: api-management
author: vladvino
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.topic: include
ms.date: 04/29/2020
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 9a1eaa79a99d848e5537d62219fcbab04013847e
ms.sourcegitcommit: 95efd248f5ee3701f671dbd5cfe0aec9c9959a24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516034"
---
| 资源 | 限制 |
| ---------------------------------------------------------------------- | -------------------------- |
| 最大缩放单位数 | 每个区域 10 个<sup>1</sup> |
| 缓存大小 | 每个单位 5 GiB<sup>2</sup> |
| 每个 HTTP 颁发机构的并行后端连接<sup>3</sup> | 每个单位 2,048 个<sup>4</sup> |
| 最大缓存响应大小 | 2 MiB |
| 最大策略文档大小 | 256 KiB<sup>5</sup> |
| 每个服务实例的最大自定义网关域<sup>6</sup> | 20 个 |
| 每个服务实例的最大 CA 证书数 | 10 个 |
| 每个订阅的最大服务实例数<sup>7</sup> | 20 个 |
| 每个服务实例的最大订阅数<sup>7</sup> | 500 |
| 每个服务实例的最大客户端证书数<sup>7</sup> | 50 |
| 每个服务实例的最大 API 数<sup>7</sup> | 50 |
| 每个服务实例的最大 API 操作数<sup>7</sup> | 1,000 |
| 最大总请求持续时间<sup>7</sup> | 30 秒 |
| 最大缓冲有效负载大小<sup>7</sup> | 2 MiB |
| 最大请求 URL 大小<sup>8</sup> | 4096 字节 |

<sup>1</sup>缩放限制取决于定价层。 若要查看定价层及其缩放限制，请参阅 [API 管理定价](https://www.azure.cn/pricing/details/api-management/)。<br/>
<sup>2</sup>每单位缓存大小取决于定价层。 若要查看定价层及其缩放限制，请参阅 [API 管理定价](https://www.azure.cn/pricing/details/api-management/)。<br/>
<sup>3</sup>除非被后端明确关闭，否则将共用并重新使用连接。<br/>
<sup>4</sup>此限制按“基本”、“标准”和“高级”层级的单位计算。 开发人员层限制为 1,024。 此限制不适用于消耗层。<br/>
<sup>5</sup>此限制适用于“基本”、“标准”和“高级”层级。 在消耗层中，策略文档大小限制为 16 KiB。<br/>
<sup>6</sup>此资源仅在“高级”层级中提供。<br/>
<sup>7</sup>此资源仅适用于消耗层。<br/>
<sup>8</sup>仅适用于消耗层。 包含最长 2048 字节的查询字符串。<br/>

