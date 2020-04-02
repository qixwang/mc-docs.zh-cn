---
title: azcopy copy | Microsoft Docs
description: 本文提供有关 azcopy copy 命令的参考信息。
author: WenJason
ms.service: storage
ms.topic: reference
origin.date: 10/16/2019
ms.date: 03/30/2020
ms.author: v-jay
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: ae1241bedb7a7ba002701dbb822c225670353e60
ms.sourcegitcommit: 90d01d08faf8adb20083363a8e4e5aab139cd9b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80290428"
---
# <a name="azcopy-copy"></a>azcopy copy

将源数据复制到目标位置。

## <a name="synopsis"></a>摘要

将源数据复制到目标位置。 支持的方向如下：

  - 本地 <-> Azure Blob（SAS 或 OAuth 身份验证）
  - 本地 <-> Azure 文件存储（共享/目录 SAS 身份验证）
  - 本地 <-> ADLS Gen 2（SAS、OAuth 或共享密钥身份验证）
  - Azure Blob（SAS 或公共）-> Azure Blob（SAS 或 OAuth 身份验证）
  - Azure Blob（SAS 或公共）-> Azure 文件存储 (SAS)
  - Azure 文件存储 (SAS) -> Azure 文件存储 (SAS)
  - Azure 文件存储 (SAS) -> Azure Blob（SAS 或 OAuth 身份验证）
  - AWS S3（访问密钥）-> Azure 块 Blob（SAS 或 OAuth 身份验证）

有关详细信息，请参阅示例。

## <a name="related-conceptual-articles"></a>相关概念性文章

- [AzCopy 入门](storage-use-azcopy-v10.md)
- [使用 AzCopy 和 Blob 存储传输数据](storage-use-azcopy-blobs.md)
- [使用 AzCopy 和文件存储传输数据](storage-use-azcopy-files.md)
- [对 AzCopy 进行配置、优化和故障排除](storage-use-azcopy-configure.md)

## <a name="advanced"></a>高级

从本地磁盘上传时，AzCopy 会根据文件扩展名或内容（如果未指定扩展名）自动检测文件的内容类型。

内置的查找表较小，但在 Unix 上，已通过采用以下一个或多个名称的本地系统 mime.types 文件（如果可用）扩充了该查找表：

- /etc/mime.types
- /etc/apache2/mime.types
- /etc/apache/mime.types

在 Windows 上，MIME 类型是从注册表提取的。 可以借助一个标志禁用此功能。 请参阅“标志”部分。

如果使用命令行设置环境变量，则可以在命令行历史记录中读取该变量。 请考虑从命令行历史记录中清除包含凭据的变量。 若要防止变量出现在历史记录中，可以使用脚本提示用户输入凭据，并设置环境变量。

```
azcopy copy [source] [destination] [flags]
```

## <a name="examples"></a>示例

使用 OAuth 身份验证上传单个文件。 如果尚未登录到 AzCopy，请在运行以下命令之前运行 azcopy login 命令。

- azcopy cp "/path/to/file.txt" "https://[account].blob.core.chinacloudapi.cn/[container]/[path/to/blob]"

与前面的示例相同，但这一次此命令还会计算文件内容的 MD5 哈希，并将其另存为 Blob 的 Content-MD5 属性：

- azcopy cp "/path/to/file.txt" "https://[account].blob.core.chinacloudapi.cn/[container]/[path/to/blob]" --put-md5

使用 SAS 令牌上传单个文件：

- azcopy cp "/path/to/file.txt" "https://[account].blob.core.chinacloudapi.cn/[container]/[path/to/blob]?[SAS]"

使用 SAS 令牌和管道（仅限块 Blob）上传单个文件：
  
- cat "/path/to/file.txt" | azcopy cp "https://[account].blob.core.chinacloudapi.cn/[container]/[path/to/blob]?[SAS]"

使用 SAS 令牌上传整个目录：
  
- azcopy cp "/path/to/dir" "https://[account].blob.core.chinacloudapi.cn/[container]/[path/to/directory]?[SAS]" --recursive=true

或

- azcopy cp "/path/to/dir" "https://[account].blob.core.chinacloudapi.cn/[container]/[path/to/directory]?[SAS]" --recursive=true --put-md5

使用 SAS 令牌和通配符 (*) 上传一组文件：

- azcopy cp "/path/*foo/* bar/*.pdf" "https://[account].blob.core.chinacloudapi.cn/[container]/[path/to/directory]?[SAS]"

使用 SAS 令牌和通配符 (*) 上传文件和目录：

- azcopy cp "/path/*foo/* bar*" "https://[account].blob.core.chinacloudapi.cn/[container]/[path/to/directory]?[SAS]" --recursive=true

使用 OAuth 身份验证下载单个文件。 如果尚未登录到 AzCopy，请在运行以下命令之前运行 azcopy login 命令。

- azcopy cp "https://[account].blob.core.chinacloudapi.cn/[container]/[path/to/blob]" "/path/to/file.txt"

使用 SAS 令牌下载单个文件：

- azcopy cp "https://[account].blob.core.chinacloudapi.cn/[container]/[path/to/blob]?[SAS]" "/path/to/file.txt"

使用 SAS 令牌下载单个文件，然后通过管道将输出传送到文件（仅限块 Blob）：
  
- azcopy cp "https://[account].blob.core.chinacloudapi.cn/[container]/[path/to/blob]?[SAS]" > "/path/to/file.txt"

使用 SAS 令牌下载整个目录：
  
- azcopy cp "https://[account].blob.core.chinacloudapi.cn/[container]/[path/to/directory]?[SAS]" "/path/to/dir" --recursive=true

有关在 URL 中使用通配符 (*) 的说明：

仅支持通过两种方式在 URL 中使用通配符。 

- 紧接在 URL 的最后一个正斜杠 (/) 后面使用一个通配符。 这会将目录中的所有文件直接复制到目标，而不会将其放入子目录。

- 也可以在容器名称中使用一个通配符，前提是 URL 只引用容器而不引用 Blob。 可以使用此方法从一部分容器中获取文件。

下载目录的内容，而不复制包含的目录本身。

- azcopy cp "https://[srcaccount].blob.core.chinacloudapi.cn/[container]/[path/to/folder]/*?[SAS]" "/path/to/dir"

下载整个存储帐户。

- azcopy cp "https://[srcaccount].blob.core.chinacloudapi.cn/" "/path/to/dir" --recursive

在容器名称中使用通配符 (*)，下载存储帐户中的一部分容器。

- azcopy cp "https://[srcaccount].blob.core.chinacloudapi.cn/[container*name]" "/path/to/dir" --recursive

使用 SAS 令牌将单个 Blob 复制到另一个 Blob。

- azcopy cp "https://[srcaccount].blob.core.chinacloudapi.cn/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.chinacloudapi.cn/[container]/[path/to/blob]?[SAS]"

使用 SAS 令牌和 OAuth 令牌将单个 Blob 复制到另一个 Blob。 必须在源帐户 URL 的末尾使用 SAS 令牌，但如果使用 azcopy login 命令登录到 AzCopy，则目标帐户不需要 SAS 令牌。 

- azcopy cp "https://[srcaccount].blob.core.chinacloudapi.cn/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.chinacloudapi.cn/[container]/[path/to/blob]"

使用 SAS 令牌将一个 Blob 虚拟目录复制到另一个 Blob：

- azcopy cp "https://[srcaccount].blob.core.chinacloudapi.cn/[container]/[path/to/directory]?[SAS]" "https://[destaccount].blob.core.chinacloudapi.cn/[container]/[path/to/directory]?[SAS]" --recursive=true

使用 SAS 令牌将一个存储帐户中的所有 Blob 容器、目录和 Blob 复制到另一个存储帐户：

- azcopy cp "https://[srcaccount].blob.core.chinacloudapi.cn?[SAS]" "https://[destaccount].blob.core.chinacloudapi.cn?[SAS]" --recursive=true

使用访问密钥和 SAS 令牌将单个对象复制到 Amazon Web Services (AWS) S3 中的 Blob 存储。 首先，为 AWS S3 源设置环境变量 AWS_ACCESS_KEY_ID 和 AWS_SECRET_ACCESS_KEY。
  
- azcopy cp "https://s3.amazonaws.com/ [bucket]/[object]" "https://[destaccount].blob.core.chinacloudapi.cn/[container]/[path/to/blob]?[SAS]"

使用访问密钥和 SAS 令牌将整个目录复制到 AWS S3 中的 Blob 存储。 首先，为 AWS S3 源设置环境变量 AWS_ACCESS_KEY_ID 和 AWS_SECRET_ACCESS_KEY。

- azcopy cp "https://s3.amazonaws.com/ [bucket]/[folder]" "https://[destaccount].blob.core.chinacloudapi.cn/[container]/[path/to/directory]?[SAS]" --recursive=true

请参阅 https://docs.aws.amazon.com/AmazonS3/latest/user-guide/using-folders.html 以更好地了解 [folder] 占位符。

使用访问密钥和 SAS 令牌将所有桶复制到 Amazon Web Services (AWS) 中的 Blob 存储。 首先，为 AWS S3 源设置环境变量 AWS_ACCESS_KEY_ID 和 AWS_SECRET_ACCESS_KEY。

- azcopy cp "https://s3.amazonaws.com/ " "https://[destaccount].blob.core.chinacloudapi.cn?[SAS]" --recursive=true

使用访问密钥和 SAS 令牌将所有桶复制到 Amazon Web Services (AWS) 区域中的 Blob 存储。 首先，为 AWS S3 源设置环境变量 AWS_ACCESS_KEY_ID 和 AWS_SECRET_ACCESS_KEY。

- azcopy cp "https://s3- [region].amazonaws.com/" "https://[destaccount].blob.core.chinacloudapi.cn?[SAS]" --recursive=true

在桶名称中使用通配符 (*) 来复制一部分桶。 如前面的示例所示，需要提供访问密钥和 SAS 令牌。 请确保为 AWS S3 源设置环境变量 AWS_ACCESS_KEY_ID 和 AWS_SECRET_ACCESS_KEY。

- azcopy cp "https://s3.amazonaws.com/ [bucket*name]/" "https://[destaccount].blob.core.chinacloudapi.cn?[SAS]" --recursive=true

## <a name="options"></a>选项

**--blob-type** 字符串                     定义目标中的 Blob 类型。 此选项用于上传 Blob 以及在帐户之间进行复制（默认值为“Detect”）。 有效值包括“Detect”、“BlockBlob”、“PageBlob”和“AppendBlob”。 在帐户之间复制时，使用值“Detect”会导致 AzCopy 使用源 Blob 的类型来确定目标 Blob 的类型。 上传文件时，“Detect”会根据文件扩展名确定文件是 VHD 文件还是 VHDX 文件。 如果文件是 VHD 或 VHDX 文件，则 AzCopy 会将该文件视为页 Blob。 （默认值为“Detect”）

**--block-blob-tier** 字符串               将块 Blob 直接上传到所选的[访问层](../blobs/storage-blob-storage-tiers.md)。 （默认值为“None”）。 有效值包括“None”、“Hot”、“Cool”和“Archive”。 如果传递“None”或未传递任何层，则 blob 将继承存储帐户的层。

**--block-size-mb** 浮点数                  在上传到 Azure 存储以及从 Azure 存储下载时使用此块大小（以 MiB 为单位）。 默认值是根据文件大小自动计算的。 允许使用小数（例如：0.25）。

**--cache-control** 字符串                 设置 cache-control 标头。 下载时返回。

**--check-length**                         传输后检查目标上的文件长度。 如果源与目标之间不匹配，则将传输标记为失败。 （默认值为 true）

**--check-md5** 字符串                     指定下载时验证 MD5 哈希的严格程度。 仅在下载时可用。 可用选项：NoCheck、LogOnly、FailIfDifferent、FailIfDifferentOrMissing。 （默认值为“FailIfDifferent”）

**--content-disposition** 字符串           设置 content-disposition 标头。 下载时返回。

**--content-encoding** 字符串              设置 content-encoding 标头。 下载时返回。

**--content-language** 字符串              设置 content-language 标头。 下载时返回。

**--content-type** 字符串                  指定文件的内容类型。 暗指 no-guess-mime-type。 下载时返回。

**--decompress**                           下载时自动解压缩文件（如果 content-encoding 指示文件已压缩）。 支持的 content-encoding 值为“gzip”和“deflate”。 不需要提供“.gz”/“.gzip”或“.zz”文件扩展名，但如果存在，会将其删除。

**--exclude-attributes** 字符串            （仅限 Windows）排除其属性与属性列表相匹配的文件。 例如：A;S;R

**--exclude-blob-type** 字符串             （可选）指定从容器或帐户复制 Blob 时要排除的 Blob 类型 (BlockBlob/PageBlob/AppendBlob)。 此标志不适用于将数据从非 Azure 服务复制到 Azure 服务。 应使用“;”分隔多个 Blob。

**--exclude-path** 字符串                  复制时排除这些路径。 此选项不支持通配符 (*)。 检查相对路径前缀（例如：myFolder;myFolder/subDirName/file.pdf）。 与帐户遍历结合使用时，路径不包含容器名称。

**--exclude-pattern** 字符串               复制时排除这些文件。 此选项支持通配符 (*)

**--follow-symlinks**                      从本地文件系统上传时遵循符号链接。

**--from-to** 字符串                       （可选）指定源-目标组合。 例如：LocalBlob、BlobLocal、LocalBlobFS。

**-h、--help**                                 copy 命令的帮助

**--include-attributes** 字符串            （仅限 Windows）包括其属性与属性列表相匹配的文件。 例如：A;S;R

**--include-path** 字符串                  复制时仅包括这些路径。 此选项不支持通配符 (*)。 检查相对路径前缀（例如：myFolder;myFolder/subDirName/file.pdf）。

**--include-pattern** 字符串               复制时仅包括这些文件。 此选项支持通配符 (*)。 使用“;”分隔文件。

**--log-level** 字符串                     定义日志文件的日志详细程度，可用级别：INFO（所有请求/响应）、WARNING（响应缓慢）、ERROR（仅限失败的请求）和 NONE（无输出日志）。 （默认值为“INFO”）

**--metadata** 字符串                      将这些键值对作为元数据上传到 Azure 存储。

**--no-guess-mime-type**                   阻止 AzCopy 根据文件的扩展名或内容来检测内容类型。

**--overwrite** 字符串                     如果此标志设置为 true，则会覆盖目标上有冲突的文件和 Blob。 可能的值包括“true”、“false”、“ifSourceNewer”和“prompt”。 （默认值为“true”）

**--page-blob-tier** 字符串                使用此 Blob 层将页 Blob 上传到 Azure 存储。 （默认值为“None”）

**--preserve-last-modified-time**          仅当目标为文件系统时才可用。

**--put-md5**                             创建每个文件的 MD5 哈希，并将该哈希另存为目标 Blob 或文件的 Content-MD5 属性。 （默认不会创建哈希。）仅在上传时可用。

**--recursive**                            从本地文件系统上传时以递归方式检查子目录。

**--s2s-detect-source-changed**           检查源在枚举后是否已更改。

**--s2s-handle-invalid-metadata** 字符串   指定如何处理无效的元数据键。 可用选项：ExcludeIfInvalid、FailIfInvalid、RenameIfInvalid。 （默认值为“ExcludeIfInvalid”）

**--s2s-preserve-access-tier**             在服务之间复制过程中保留访问层 请参阅 [Azure Blob 存储：热、冷和存档访问层](/storage/blobs/storage-blob-storage-tiers)，确保目标存储帐户支持设置访问层。 如果不支持设置访问层，请使用 s2sPreserveAccessTier=false 来绕过访问层的复制。 （默认值为 true）

**--s2s-preserve-properties**              在服务之间复制过程中保留完整属性。 对于 AWS S3 和 Azure 文件存储的非单一文件源，列出操作不会返回对象和文件的完整属性。 若要保留完整属性，AzCopy 需要对每个对象或文件发送一个附加的请求。 （默认值为 true）

## <a name="options-inherited-from-parent-commands"></a>从父命令继承的选项

**--cap-mbps uint32**      以兆位/秒为单位限制传输速率。 瞬间吞吐量可能与上限略有不同。 如果此选项设置为零，或者省略，则吞吐量不受限制。

**--output-type** 字符串   命令输出的格式。 选项包括：text、json。 默认值为“text”。 （默认值为“text”）

## <a name="see-also"></a>另请参阅

- [azcopy](storage-ref-azcopy.md)
