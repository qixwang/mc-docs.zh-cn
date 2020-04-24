---
title: azcopy list | Microsoft Docs
description: 本文提供有关 azcopy list 命令的参考信息。
author: WenJason
ms.service: storage
ms.topic: reference
origin.date: 10/16/2019
ms.date: 11/25/2019
ms.author: v-jay
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: f4e87f811f3602faa45da17e49998ffed57c1756
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74328797"
---
# <a name="azcopy-list"></a>azcopy list

列出给定资源中的实体。

## <a name="synopsis"></a>概要

当前版本仅支持 Blob 容器。

```azcopy
azcopy list [containerURL] [flags]
```

## <a name="related-conceptual-articles"></a>相关概念性文章

- [AzCopy 入门](storage-use-azcopy-v10.md)
- [使用 AzCopy 和 Blob 存储传输数据](storage-use-azcopy-blobs.md)
- [使用 AzCopy 和文件存储传输数据](storage-use-azcopy-files.md)
- [对 AzCopy 进行配置、优化和故障排除](storage-use-azcopy-configure.md)

## <a name="examples"></a>示例

```azcopy
azcopy list [containerURL]
```

## <a name="options"></a>选项

|选项|说明|
|--|--|
|-h、--help|显示 list 命令的帮助内容。|
|--machine-readable|列出文件大小（以字节为单位）。|
|--mega-units|显示单位为 1000，而不是 1024。|
|--running-tally|计算文件总数及其大小。|

## <a name="options-inherited-from-parent-commands"></a>从父命令继承的选项

|选项|说明|
|---|---|
|--cap-mbps uint32|以兆位/秒为单位限制传输速率。 瞬间吞吐量可能与上限略有不同。 如果此选项设置为零，或者省略，则吞吐量不受限制。|
|--output-type string|命令输出的格式。 选项包括：text、json。 默认值为“text”。|

## <a name="see-also"></a>另请参阅

- [azcopy](storage-ref-azcopy.md)
