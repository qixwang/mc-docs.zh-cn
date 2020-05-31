---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 03/05/2020
ms.date: 06/01/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 956fea4aba087ed2559f0b4653d293766a7d2f29
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84199537"
---
Azure 存储提供多种类型的存储帐户。 每种类型支持不同的功能，并且具有自己的定价模型。 在创建存储帐户之前，需考虑到这些差异，以便确定最适合应用程序的帐户类型。 存储帐户的类型包括：

- **常规用途 v2 帐户**：Blob、文件、队列和表的基本存储帐户类型。 建议在大多数情况下使用 Azure 存储。
- **常规用途 v1 帐户**：Blob、文件、队列和表的旧帐户类型。 如果可能，请改用常规用途 v2 帐户。
- **Blob 存储帐户**：旧版仅限 Blob 存储帐户。 如果可能，请改用常规用途 v2 帐户。

下表介绍存储帐户的类型及其功能：

| 存储帐户类型 | 支持的服务                       | 支持的性能层      | 支持的访问层         | 复制选项               | 部署模型<div role="complementary" aria-labelledby="deployment-model"><sup>1</sup></div> | Encryption<div role="complementary" aria-labelledby="encryption"><sup>2</sup></div> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------|-----------------------------------|------------------------------|------------------------|
| 常规用途 V2   | Blob、文件、队列、表、磁盘和 Data Lake Gen2<div role="complementary" aria-labelledby="data-lake-gen2"><sup>5</sup></div>      | 标准、高级<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | 热、冷、存档<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS、GRS、RA-GRS | Resource Manager             | 加密              |
| 常规用途 V1   | Blob、文件、队列、表和磁盘       | 标准、高级<div role="complementary" aria-labelledby="premium-performance"><sup>4</sup></div> | 空值                            | LRS、GRS、RA-GRS                  | 资源管理器、经典    | 加密              |
| BlobStorage         | Blob（仅块 Blob 和追加 Blob） | 标准                      | 热、冷、存档<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS、GRS、RA-GRS                  | Resource Manager             | 加密              |

<div id="deployment-model"><sup>1</sup>建议使用 Azure 资源管理器部署模型。 使用经典部署模型的存储帐户仍可在某些位置创建，而现有的经典帐户仍然会受支持。 有关详细信息，请参阅 <a href="/azure-resource-manager/resource-manager-deployment-model">Azure 资源管理器与经典部署：了解部署模型和资源状态</a>。</div><br/>

<div id="encryption"><sup>2</sup>使用针对静态数据的存储服务加密 (SSE) 来加密所有存储帐户。 有关详细信息，请参阅<a href="/storage/common/storage-service-encryption">静态数据的 Azure 存储服务加密</a>。</div><br/>

<div id="archive"><sup>3</sup> 存档存储和 Blob 级别分层仅支持块 Blob。 存档层仅在单个 Blob 的级别可用，在存储帐户级别不可用。 有关详细信息，请参阅 <a href="/storage/blobs/storage-blob-storage-tiers">Azure Blob 存储：热、冷、存档存储层</a>。</div><br/>

<div id="premium-performance"><sup>4</sup>常规用途 v2 和常规用途 v1 帐户的高级性能仅适用于磁盘和页 blob。</div>

<div id="data-lake-gen2"><sup>5</sup>Azure Data Lake Storage Gen2 是一组专用于大数据分析的功能，基于 Azure Blob 存储而构建。 只有启用了分层命名空间的常规用途 V2 存储帐户才支持 Data Lake Storage Gen2。 有关 Data Lake Storage Gen2 的详细信息，请参阅 <a href="/storage/blobs/data-lake-storage-introduction">Azure Data Lake Storage Gen2 简介</a>。</div>
