---
title: azcopy jobs clean | Microsoft Docs
description: 本文提供有关 azcopy jobs clean 命令的参考信息。
author: WenJason
ms.service: storage
ms.topic: reference
origin.date: 10/16/2019
ms.date: 11/25/2019
ms.author: v-jay
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 24ebc3c44fd79c0b009f16a0cb9c00873590dca3
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74328805"
---
# <a name="azcopy-jobs-clean"></a>azcopy jobs clean

删除所有作业的所有日志和计划文件

```
azcopy jobs clean [flags]
```

## <a name="related-conceptual-articles"></a>相关概念性文章

- [AzCopy 入门](storage-use-azcopy-v10.md)
- [使用 AzCopy 和 Blob 存储传输数据](storage-use-azcopy-blobs.md)
- [使用 AzCopy 和文件存储传输数据](storage-use-azcopy-files.md)
- [对 AzCopy 进行配置、优化和故障排除](storage-use-azcopy-configure.md)

## <a name="examples"></a>示例

```
  azcopy jobs clean --with-status=completed
```

## <a name="options"></a>选项

**-h, --help**                clean 命令的帮助。

**--with-status** 字符串   仅删除具有此状态的作业，可用值：Canceled、Completed、Failed、InProgress、All（默认值为“All”）

## <a name="options-inherited-from-parent-commands"></a>从父命令继承的选项

**--cap-mbps uint32**      以兆位/秒为单位限制传输速率。 瞬间吞吐量可能与上限略有不同。 如果此选项设置为零，或者省略，则吞吐量不受限制。

**--output-type** 字符串   命令输出的格式。 选项包括：text、json。 默认值为“text”。 （默认值为“text”）

## <a name="see-also"></a>另请参阅

- [azcopy jobs](storage-ref-azcopy-jobs.md)
