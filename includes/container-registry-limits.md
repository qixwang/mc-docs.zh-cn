---
title: include 文件
description: include 文件
services: container-registry
author: rockboyfor
ms.service: container-registry
ms.topic: include
origin.date: 03/11/2020
ms.date: 05/18/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: c2d8deef8f8c8f9910367fc5e3380525ae53ec2a
ms.sourcegitcommit: 8d56bc6baeb42d675695ecef1909d76f5c4a6ae3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83406222"
---
| 资源 | 基本 | 标准 | 高级 |
|---|---|---|---|
| 存储<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| 最大映像层大小 | 200 GiB | 200 GiB | 200 GiB |
| 每分钟读取操作数<sup>2、3</sup> | 1,000 | 3,000 | 10,000 |
| 每分钟写入操作数<sup>2、4</sup> | 100 | 500 | 2,000 |
| 下载带宽 (MBps)<sup>2</sup> | 30 | 60 | 100 |
| 上传带宽 (MBps)<sup>2</sup> | 10 个 | 20 个 | 50 |
| Webhook | 2 | 10 个 | 500 |
| 异地复制 | 空值 | 空值 | [支持][geo-replication] |
| 内容信任 | 空值 | 空值 | [支持][content-trust] |
| 客户管理的密钥 | 空值 | 空值 | [预览][cmk] |
| 存储库范围内的权限 | 空值 | 空值 | [预览][token]|
| &bull; 令牌 | 空值 | 空值 | 20,000 |
| &bull; 范围映射 | 空值 | 空值 | 20,000 |
| &bull; 每个范围映射的存储库 | 空值 | 空值 | 500 |

<sup>1</sup>指定的存储空间上限是每层包含的存储量。 对于超出这些限制的图像存储，将每日针对每 GiB 进行额外收费。 有关费率的信息，请参阅 [Azure 容器注册表定价][pricing]。

<sup>2</sup>*读取操作数*、*写入操作数*和*带宽*是最小估计值。 Azure 容器注册表根据使用需求努力提高性能。

<sup>3</sup>[docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) 根据映像中的层数和清单检索行为转换为多个读取操作。

<sup>4</sup>[docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) 根据必须推送的层数转换为多个写入操作。 `docker push` 包含 ReadOps，用于检索现有映像的清单。

<!-- LINKS - External -->

[pricing]: https://www.azure.cn/pricing/details/container-registry/

<!-- LINKS - Internal -->

[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md

<!-- Not Available on [vnet]: ../articles/container-registry/container-registry-vnet.md-->
<!-- Not Available on [plink]: ../articles/container-registry/container-registry-private-link.md-->

[cmk]: ../articles/container-registry/container-registry-customer-managed-keys.md
[token]: ../articles/container-registry/container-registry-repository-scoped-permissions.md

<!-- Update_Description: update meta properties, wording update, update link -->