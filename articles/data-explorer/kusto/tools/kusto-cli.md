---
title: Kusto CLI - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 Kusto CLI。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/24/2020
ms.date: 07/01/2020
ms.openlocfilehash: 6dac07aa872afb16e5d1ad5ac63c3d23e4da92f4
ms.sourcegitcommit: c17e965d4ffd82fd7cd86b2648fcb0053a65df00
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86470410"
---
# <a name="kusto-cli"></a>Kusto CLI

Kusto.Cli 是一个命令行实用工具，用于将请求发送到 Kusto 并显示结果。 它可以采用以下几种模式运行：

* REPL 模式：用户输入查询和命令后，此工具将显示结果，然后等待下一个用户查询/命令。
  （“REPL”代表“read/eval/print/loop”，即读取/求值/打印/循环。）

* 执行模式：用户输入一个或多个查询和命令以作为命令行参数运行。 参数会按顺序自动运行，结果将输出到控制台。 （可选）在所有输入查询和命令运行后，此工具将进入 REPL 模式。

* 脚本模式：与执行模式类似，但使用的是通过“脚本”文件指定的查询和命令。

Kusto.Cli 主要用于针对通常需要编写代码的 Kusto 服务自动执行任务。 例如，C# 程序或 PowerShell 脚本。

## <a name="get-the-tool"></a>获取工具

Kusto.Cli 是 NuGet 包 `Microsoft.Azure.Kusto.Tools` 的一部分，[可以下载](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Tools/)。
下载后，将包的 `tools` 文件夹提取到目标文件夹。 不需要进行其他安装，因为它是 xcopy 可安装的。

## <a name="run-the-tool"></a>运行工具

Kusto.Cli 至少需要一个命令行参数才能运行。 通常，该参数是该工具应连接到的 Kusto 服务的连接字符串。
有关详细信息，请参阅 [Kusto 连接字符串](../api/connection-strings/kusto.md)。 如果不使用命令行参数来运行工具，而是使用未知参数集或使用 `/help` 开关，控制台上将显示一条帮助消息。

例如，使用以下命令运行 Kusto.Cli。 命令将连接到 `help` Kusto 服务，并将数据库上下文设置为 `Samples` 数据库：

```
Kusto.Cli.exe "https://help.kusto.chinacloudapi.cn/Samples;Fed=true"
```

> [!NOTE]
> 使用双引号将连接字符串括起来，防止 shell 应用程序（例如 PowerShell）错误地解释分号 (`;`) 和类似的字符。

## <a name="command-line-arguments"></a>命令行参数

`Kusto.Cli.exe` ConnectionString [Switches] 

*ConnectionString*
* [Kusto 连接字符串](../api/connection-strings/kusto.md)，可保存所有 Kusto 连接信息。
  默认为 `net.tcp://localhost/NetDefaultDB`。

`-execute:`QueryOrCommand
* 如果已指定，则在执行模式下运行 Kusto.Cli，并且会运行指定的查询或命令。 此开关可以重复，查询/命令会按出现的顺序运行。
  此开关不能与 `-script` 或 `-scriptml`一起使用。

`-keepRunning:`EnableKeepRunning
* 如果指定为 `true` 或 `false`，在处理完所有 `-script` 或 `-execute` 值后，它将启用或禁用 REPL 模式。

`-script:`ScriptFile
* 如果已指定，则在脚本模式下运行 Kusto.Cli。 将加载指定的脚本文件，并按顺序运行其中的查询或命令。
  当行以 `&` 或 `&&` 组合结尾时除外，换行符用于分隔查询/命令，如下所述。
  此开关不能与 `-execute` 一起使用。

`-scriptml:`ScriptFile
* 如果已指定，则在脚本模式下运行 Kusto.Cli。 将加载指定的脚本文件，并按顺序运行其中的查询或命令。
  整个脚本文件会被视为单个查询或命令。
  此开关不能与 `-execute` 一起使用。

`-echo:`EnableEchoMode
* 如果指定为 `true` 或 `false`，它将启用或禁用回显模式。
  启用回显模式时，将在输出中重复每个查询或命令。

`-transcript:`TranscriptFile  
* 如果已指定，则将程序输出写入 TranscriptFile。

`-logToConsole:`EnableLogToConsole
* 如果指定为 `true` 或 `false`，它将启用或禁用在控制台上显示程序输出。

`-lineMode:`EnableLineMode
* 如果已指定，则在设置为 `true` 时的默认行输入模式和设置为 `false` 时的块输入模式之间切换。 有关这两种模式的说明，请参阅下文，这两种模式确定了处理换行符的方式。

**示例**

```
Kusto.Cli.exe "https://kustolab.kusto.chinacloudapi.cn/;Fed=true" -script:"c:\mycommands.txt"
```

> [!NOTE] 
> 冒号和参数值之间不应有空格

## <a name="directives"></a>指令

Kusto.Cli 在工具中运行多个指令，而不是将其发送到服务进行处理。

|指令                      |描述|
|-------------------------------|-----------|
|`?`<br>`#h`<br>`#help`         |获取简短的帮助消息|
|`q`<br>`#quit`<br>`#exit`      |退出工具|
|`#a`<br>`#abort`               |退出工具失败|
|`#clip`                        |下一个查询或命令的结果将复制到剪贴板|
|`#cls`                         |清除控制台屏幕|
|`#connect` [ConnectionString]|连接到不同的 Kusto 服务（如果省略 ConnectionString，将显示当前的服务）|
|`#crp` [Name [`=` Value]]    |设置客户端请求属性的值，或者仅显示此值，或显示所有值|
|`#crp` (`-list` \| `-doc`) [Prefix]|按前缀列出或全部列出客户端请求属性|
|`#dbcontext` [DatabaseName]  |将查询和命令使用的“上下文”数据库更改为 DatabaseName。 如果省略，将显示当前上下文|
|`ke` Text                    |将指定的文本发送到正在运行的 Kusto.Explorer 进程|
|`#loop` Count Text          |多次运行文本|
|`#qp` [Name [`=` Value]]    |设置查询参数的值，或仅显示此值，或显示所有值。 将剪裁掉开头/结尾的单/双引号|
|`#save` Filename             |下一个查询或命令的结果将保存到指定的 CSV 文件|
|`#script` Filename           |执行指定的脚本|
|`#scriptml` Filename         |执行指定的多行脚本|

## <a name="line-input-mode-and-block-input-mode"></a>行输入模式和块输入模式

默认情况下，Kusto.Cli 在行输入模式下运行。 每个换行符被解释为查询/命令之间的分隔符，并且该行会立即被发送执行。

在此模式下，你可以将较长的查询或命令分解为多行。 `&` 字符作为换行符之前行的最后一个字符，使 Kusto.Cli 继续读取下一行。 `&&` 字符作为换行符之前行的最后一个字符，使 Kusto.Cli 忽略该换行符并继续读取下一行。

Kusto.Cli 还支持在块输入模式下运行。 通过使用命令行开关 `-lineMode:false` 或使用指令 `#blockmode`，你可以指示 Kusto.Cli 假定每一行都是前一行的延续，使查询和命令仅由空输入行分隔。

## <a name="comments"></a>注释

Kusto.Cli 将开始新行的 `//` 字符串解释为注释行。 它将忽略该行的其余部分并继续读取下一行。

## <a name="tool-only-options"></a>仅限工具的选项

命令                        | 效果                                                                            | 当前
--------------------------------|-----------------------------------------------------------------------------------|-----------
#timeon|#timeoff                | 启用/禁用选项 `timing`：显示请求花费的时间                    | TRUE
#tableon|#tableoff              | 启用/禁用选项 `tableView`：将结果集的格式设置为表                  | TRUE
#marson|#marsoff                | 启用/禁用选项 `marsView`：显示倒数第二个结果集          | FALSE
#resultson|#resultsoff          | 启用/禁用选项 `outputResultsSet`：显示结果集                 | TRUE
#prettyon|#prettyoff            | 启用/禁用选项 `prettyErrors`：清理错误                             | TRUE
#markdownon|#markdownoff        | 启用/禁用选项 `markdownView`：将表的格式设置为 MarkDown                   | FALSE
#progressiveon|#progressiveoff  | 启用/禁用选项 `progressiveView`：询问并显示渐进式结果  | FALSE
#linemode|#blockmode            | 启用/禁用选项 `lineMode`：单行输入模式                          | TRUE

命令                              | 效果                                                                                                         | 默认
--------------------------------------|----------------------------------------------------------------------------------------------------------------|-----------
#cridon|#cridoff                      | （启用|禁用选项 `crid`：发送请求之前显示 ClientRequestId）                          | FALSE
#csvheaderson|#csvheadersoff          | （启用|禁用选项 `csvHeaders`：在 CSV 输出中包含标头）                                            | TRUE
#focuson|#focusoff                    | （启用|禁用选项 `focus`：删除所有多余的内容并专注于正确的内容）                        | FALSE
#linemode|#blockmode                  | （启用|禁用选项 `lineMode`：单行输入模式）                                                      | TRUE
#markdownon|#markdownoff              | （启用|禁用选项 `markdownView`：将表的格式设置为 MarkDown）                                              | FALSE
#marson|#marsoff                      | （启用|禁用选项 `marsView`：显示倒数第二个结果集）                                      | FALSE
#prettyon|#prettyoff                  | （启用|禁用选项 `prettyErrors`：清理错误）                                                        | TRUE
#querystreamingon|#querystreamingoff  | （启用|禁用选项 `queryStreaming`：使用 queryStreaming 终结点（仅限 Kusto 团队））                    | FALSE
#resultson|#resultsoff                | （启用|禁用选项 `outputResultsSet`：显示结果集）                                            | TRUE
#tableon|#tableoff                    | （启用|禁用选项 `tableView`：将结果集的格式设置为表）                                              | TRUE
#timeon|#timeoff                      | （启用|禁用选项 `timing`：显示请求花费的时间）                               | TRUE
#typeon|#typeoff                      | （启用|禁用选项 `typeView`：在表视图中显示每个列的类型。 强制 Streaming=true）| TRUE
#v2protocolon|#v2protocoloff          | （启用|禁用选项 `v2protocol`：使用 v2 查询协议，而不是 v1）                                        | TRUE

## <a name="use-kustocli-to-export-results-as-csv"></a>使用 Kusto.Cli 将结果导出为 CSV

Kusto.Cli 有一个特殊的客户端命令 `#save`，该命令将下一个查询结果导出到 CSV 格式的本地文件。 例如，以下行会将 `StormEvents` 表中的 10 条记录导出到 `help.kusto.chinacloudapi.cn` 群集，`Samples` 数据库：

```
Kusto.Cli.exe @help/Samples -execute:"#save c:\temp\test.log" -execute:"StormEvents | take 10"
```

## <a name="use-kustocli-to-control-a-running-instance-of-kustoexplorer"></a>使用 Kusto.Cli 控制 Kusto.Explorer 的运行实例

你可以指示 Kusto.Cli 与在计算机上运行的 Kusto.Explorer 的“主”实例通信，并向其发送查询。 对于想运行多个查询但不想重复启动 Kusto.Explorer 进程的程序，此机制很有用。 在下面的示例中，Kusto.Cli 用于对 help 群集运行查询：

```
#connect cluster('help').database('Samples')

#ke StormEvents | count
```

语法非常简单：`#ke`，后跟空格和要运行的查询。
查询随后会发送到 Kusto.Explorer 的主实例（如果存在），并会在 Kusto.Cli 中设置当前的群集/数据库集。
 
