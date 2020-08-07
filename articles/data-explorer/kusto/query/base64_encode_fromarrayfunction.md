---
title: Base64_encode_fromarray() - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 base64_encode_fromarray()。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/11/2020
ms.date: 07/31/2020
ms.openlocfilehash: ff1725c9ee72c22902cb0d2de8e629848899021f
ms.sourcegitcommit: 4e1bc2e9b2a12dbcc05c52db5dbd1ae290aeb18d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87509524"
---
# <a name="base64_encode_fromarray"></a>base64_encode_fromarray()

从字节数组对 base64 字符串进行编码。

## <a name="syntax"></a>语法

`base64_encode_fromarray(`*BytesArray*`)`

## <a name="arguments"></a>参数

* *BytesArray*：输入要编码为 base64 字符串的字节数组。

## <a name="returns"></a>返回

返回从字节数组进行编码的 base64 字符串。

* 若要将 base64 字符串解码为 UTF-8 字符串，请参阅 [base64_decode_tostring()](base64_decode_tostringfunction.md)
* 若要将字符串编码为 base64 字符串，请参阅 [base64_encode_tostring()](base64_encode_tostringfunction.md)
* 此函数是 [base64_decode_toarray()](base64_decode_toarrayfunction.md) 的反函数

## <a name="example"></a>示例

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
let bytes_array = toscalar(print base64_decode_toarray("S3VzdG8="));
print decoded_base64_string = base64_encode_fromarray(bytes_array)
```

|decoded_base64_string|
|---|
|S3VzdG8=|


尝试从无效 UTF-8 编码字符串生成的无效字节数组中对 base64 字符串进行编码将返回 null：

<!-- csl: https://help.kusto.chinacloudapi.cn/Samples -->
```kusto
let empty_bytes_array = toscalar(print base64_decode_toarray("U3RyaW5n0KHR0tGA0L7Rh9C60LA"));
print empty_string = base64_encode_fromarray(empty_bytes_array)
```

|empty_string|
|---|
||
