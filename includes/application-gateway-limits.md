---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 01/16/2020
ms.author: v-junlch
ms.openlocfilehash: fd9ccdc2b53f4de629203806dbe03c9d72a6106c
ms.sourcegitcommit: 48d51745ca18de7fa05b77501b4a9bf16cea2068
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76118109"
---
| 资源 | 默认/最大限制 | 注意 |
| --- | --- | --- |
| Azure 应用程序网关 |每个订阅 1,000 个 | |
| 前端 IP 配置 |2 |公共和专用各 1 个 |
| 前端端口 |100<sup>1</sup> | |
| 后端地址池 |100<sup>1</sup> | |
| 每个池的后端服务器 |1,200 | |
| HTTP 侦听器 |100<sup>1</sup> | |
| HTTP 负载均衡规则 |100<sup>1</sup> | |
| 后端 HTTP 设置 |100<sup>1</sup> | |
| 每个网关的实例 |32 | |
| SSL 证书 |100<sup>1</sup> |每个 HTTP 侦听器 1 个 |
| 最大 SSL 证书大小 |V1 SKU - 10 KB<br>V2 SKU - 16 KB| |
| 身份验证证书 |100 | |
| 受信任的根证书 |100 | |
| 请求超时最小值 |1 秒 | |
| 请求超时最大值 |24 小时 | |
| 站点数 |100<sup>1</sup> |每个 HTTP 侦听器 1 个 |
| 每个侦听器的 URL 映射 |1 | |
| 每个 URL 映射基于路径的最大规则数|100||
| 重定向配置数 |100<sup>1</sup>| |
| 并发 WebSocket 连接 |中型网关 20k<br> 大型网关 50k| |
| 最大 URL 长度|32KB| |
| HTTP/2 的最大标头大小 |4KB| |
| 最大文件上传大小：标准 |2 GB | |
| 最大文件上传大小 WAF |v1 中型 WAF 网关，100 MB<br>v1 大型 WAF 网关，500 MB<br>v2 WAF，750 MB| |
| WAF 正文大小限制，不带文件|128 KB||
| 最大 WAF 自定义规则|100||
| 最大 WAF 排除项数|100||

<sup>1</sup> 对于启用了 WAF 的 SKU，建议将资源数限制为 40 以实现最佳性能。

