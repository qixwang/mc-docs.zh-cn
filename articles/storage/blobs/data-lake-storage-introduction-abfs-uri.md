---
title: 使用 Azure Data Lake Storage Gen2 URI
description: 使用 Azure Data Lake Storage Gen2 URI
services: storage
author: WenJason
ms.topic: conceptual
ms.author: v-jay
origin.date: 12/06/2018
ms.date: 05/27/2019
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.reviewer: jamesbak
ms.openlocfilehash: dea419a5f0de4353c01c250d5393e0f887a80667
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "66004058"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>使用 Azure Data Lake Storage Gen2 URI

通过方案标识符 `abfs`（Azure Blob 文件系统）可以知道与 Azure Data Lake Storage Gen2 兼容的 [Hadoop 文件系统](https://www.aosabook.org/en/hdfs.html)驱动程序。 与其他 Hadoop 文件系统驱动程序一样，ABFS 驱动程序使用 URI 格式寻址支持 Data Lake Storage Gen2 的帐户中的文件和目录。

## <a name="uri-syntax"></a>URI 语法

Data Lake Storage Gen2 的 URI 语法依赖于存储帐户是否设置为将 Data Lake Storage Gen2 设为默认文件系统。

如果希望寻址的支持 Data Lake Storage Gen2 的帐户在帐户创建期间**未**设为默认文件系统，则简写 URI 语法为：

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.chinacloudapi.cn/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **方案标识符**：`abfs` 协议用作方案标识符。 可以选择是否使用安全套接字层 (SSL) 连接进行连接。 使用 `abfss` 通过安全套接字层连接进行连接。

2. **文件系统**：保存文件和文件夹的父位置。 这与 Azure 存储 Blob 服务中的“容器”相同。

3. **帐户名称**：创建期间为存储帐户提供的名称。

4. **路径**：目录结构采用正斜杠分隔 (`/`) 表示形式。

5. **文件名称**：单个文件的名称。 如果对目录寻址，则此参数是可选的。

但是，如果希望寻址的帐户在帐户创建期间设为默认文件系统，则简写 URI 语法为：

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **路径**：目录结构采用正斜杠分隔 (`/`) 表示形式。

2. **文件名称**：单个文件的名称。


## <a name="next-steps"></a>后续步骤

- [将 Azure Data Lake Storage Gen2 用于 Azure HDInsight 群集](/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fstorage%2fblobs%2ftoc.json)
