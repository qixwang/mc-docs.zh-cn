---
title: azcopy jobs resume | Microsoft Docs
description: 本文提供有关 azcopy jobs resume 命令的参考信息。
author: WenJason
ms.service: storage
ms.topic: reference
origin.date: 10/16/2019
ms.date: 11/25/2019
ms.author: v-jay
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: b746a67ce3c5ee39f9d125b9642ed1ca23f2572b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74328802"
---
# <a name="azcopy-jobs-resume"></a>azcopy jobs resume

恢复具有给定作业 ID 的现有作业。

## <a name="synopsis"></a>概要

```azcopy
azcopy jobs resume [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>相关概念性文章

- [AzCopy 入门](storage-use-azcopy-v10.md)
- [使用 AzCopy 和 Blob 存储传输数据](storage-use-azcopy-blobs.md)
- [使用 AzCopy 和文件存储传输数据](storage-use-azcopy-files.md)
- [对 AzCopy 进行配置、优化和故障排除](storage-use-azcopy-configure.md)

## <a name="options"></a>选项

|选项|说明|
|--|--|
|--destination-sas 字符串|给定 JobId 的目标的目标 SAS。|
|--exclude 字符串|筛选器：恢复作业时排除这些失败的传输。 文件应由 ";" 分隔。|
|-h、--help|显示 resume 命令的帮助内容。|
|--include 字符串|筛选器：恢复作业时仅包括这些失败的传输。 文件应由 ";" 分隔。|
|--source-sas 字符串 |给定 JobId 的源的源 SAS。|

## <a name="options-inherited-from-parent-commands"></a>从父命令继承的选项

|选项|说明|
|---|---|
|--cap-mbps uint32|以兆位/秒为单位限制传输速率。 瞬间吞吐量可能与上限略有不同。 如果此选项设置为零，或者省略，则吞吐量不受限制。|
|--output-type string|命令输出的格式。 选项包括：text、json。 默认值为“text”。|

## <a name="see-also"></a>另请参阅

- [azcopy jobs](storage-ref-azcopy-jobs.md)
