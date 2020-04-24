---
title: azcopy jobs show | Microsoft Docs
description: 本文提供有关 azcopy jobs show 命令的参考信息。
author: WenJason
ms.service: storage
ms.topic: reference
origin.date: 10/16/2019
ms.date: 11/25/2019
ms.author: v-jay
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 6a3c8fda0288bad903356ae0dddd174168c8aa3d
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74328803"
---
# <a name="azcopy-jobs-show"></a>azcopy jobs show

显示有关给定作业 ID 的详细信息。

## <a name="synopsis"></a>概要

如果仅提供不带标志的作业 ID，则返回作业的进度摘要。

运行此命令时显示的字节计数和完成百分比仅反映作业中已完成的文件。 它们不反映部分完成的文件。

如果设置了 `with-status` 标志，则将显示作业中具有给定值的传输列表。

```azcopy
azcopy jobs show [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>相关概念性文章

- [AzCopy 入门](storage-use-azcopy-v10.md)
- [使用 AzCopy 和 Blob 存储传输数据](storage-use-azcopy-blobs.md)
- [使用 AzCopy 和文件存储传输数据](storage-use-azcopy-files.md)
- [对 AzCopy 进行配置、优化和故障排除](storage-use-azcopy-configure.md)

## <a name="options"></a>选项

|选项|说明|
|--|--|
|-h、--help|显示 show 命令的帮助内容。|
|--with-status 字符串|仅列出具有此状态的作业传输，可用值：Started、Success、Failed|

## <a name="options-inherited-from-parent-commands"></a>从父命令继承的选项

|选项|说明|
|---|---|
|--cap-mbps uint32|以兆位/秒为单位限制传输速率。 瞬间吞吐量可能与上限略有不同。 如果此选项设置为零，或者省略，则吞吐量不受限制。|
|--output-type string|命令输出的格式。 选项包括：text、json。 默认值为“text”。|

## <a name="see-also"></a>另请参阅

- [azcopy jobs](storage-ref-azcopy-jobs.md)
