---
title: 排查 Azure 数据工厂问题 | Microsoft Docs
description: 了解如何排查 Azure 数据工厂中的外部控制活动问题。
services: data-factory
author: WenJason
ms.service: data-factory
ms.topic: troubleshooting
origin.date: 8/26/2019
ms.date: 05/11/2020
ms.author: v-jay
ms.reviewer: craigg
ms.openlocfilehash: 653e57ce712454ef8bb0c883b3ebd408c64a881b
ms.sourcegitcommit: f8d6fa25642171d406a1a6ad6e72159810187933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82197762"
---
# <a name="troubleshoot-azure-data-factory"></a>排查 Azure 数据工厂问题
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

本文探讨 Azure 数据工厂中的外部控制活动的常用故障排除方法。

## <a name="connector-and-copy-activity"></a>连接器和复制活动

如果遇到连接器问题（例如，使用复制活动时遇到错误），请参阅[排查 Azure 数据工厂连接器问题](connector-troubleshoot-guide.md)。

## <a name="azure-functions"></a>Azure 函数

### <a name="error-code--3602"></a>错误代码：3602

- **消息**：`Invalid HttpMethod: '%method;'.`

- **原因：** 活动有效负载中指定的 Http 方法不受 Azure 函数活动的支持。

- **建议**：支持的 Http 方法为 PUT、POST、GET、DELETE、OPTIONS、HEAD 和 TRACE。


### <a name="error-code--3603"></a>错误代码：3603

- **消息**：`Response Content is not a valid JObject.`

- **原因：** 调用的 Azure 函数未在响应中返回 JSON 有效负载。 ADF Azure 函数活动仅支持 JSON 响应内容。

- **建议**：更新 Azure 函数以返回有效的 JSON 有效负载，例如，C# 函数可以返回 (ActionResult)new OkObjectResult("{\"Id\":\"123\"}");


### <a name="error-code--3606"></a>错误代码：3606

- **消息**：Azure 函数活动缺少函数密钥。

- **原因：** Azure 函数活动定义不完整。

- **建议**：请检查输入的 AzureFunction 活动 JSON 定义是否包含名为“functionKey”的属性。


### <a name="error-code--3607"></a>错误代码：3607

- **消息**：`Azure function activity missing function name.`

- **原因：** Azure 函数活动定义不完整。

- **建议**：请检查输入的 AzureFunction 活动 JSON 定义是否包含名为“functionName”的属性。


### <a name="error-code--3608"></a>错误代码：3608

- **消息**：`Call to provided Azure function '%FunctionName;' failed with status-'%statusCode;' and message - '%message;'.`

- **原因：** 活动定义中的 Azure 函数详细信息可能不正确。

- **建议**：修复 Azure 函数详细信息，然后重试。


### <a name="error-code--3609"></a>错误代码：3609

- **消息**：`Azure function activity missing functionAppUrl.`

- **原因：** Azure 函数活动定义不完整。

- **建议**：请检查输入的 AzureFunction 活动 JSON 定义是否包含名为“functionAppUrl”的属性。


### <a name="error-code--3610"></a>错误代码：3610

- **消息**：`There was an error while calling endpoint.`

- **原因：** 函数 URL 可能不正确。

- **建议**：请确保活动 JSON 中的“functionAppUrl”值正确，然后重试。


### <a name="error-code--3611"></a>错误代码：3611

- **消息**：`Azure function activity missing Method in JSON.`

- **原因：** Azure 函数活动定义不完整。

- **建议**：请检查输入的 AzureFunction 活动 JSON 定义是否包含名为“method”的属性。


### <a name="error-code--3612"></a>错误代码：3612

- **消息**：`Azure function activity missing LinkedService definition in JSON.`

- **原因：** Azure 函数活动定义不完整。

- **建议**：请检查输入的 AzureFunction 活动 JSON 定义是否包含链接服务详细信息。

## <a name="common"></a>通用

### <a name="error-code--2103"></a>错误代码：2103

- **消息**：`Please provide value for the required property '%propertyName;'.`

- **原因：** 未提供属性的值，但此值在方案中是必需的。

- **建议**：提供消息中的值，然后重试。


### <a name="error-code--2104"></a>错误代码：2104

- **消息**：`The type of the property '%propertyName;' is incorrect.`

- **原因：** 提供的属性的类型不符合预期。

- **建议**：请修复属性的类型，然后重试。


### <a name="error-code--2105"></a>错误代码：2105

- **消息**：`An invalid json is provided for property '%propertyName;'. Encountered an error while trying to parse: '%message;'.`

- **原因：** 属性的值无效或未采用预期的格式。

- **建议**：请查看属性的文档，确保提供的值采用预期的格式和类型。


### <a name="error-code--2106"></a>错误代码：2106

- **消息**：`The storage connection string is invalid. %errorMessage;`

- **原因：** 存储的连接字符串无效或格式不正确。

- **建议**：请转到 Azure 门户，找到你的存储，复制连接字符串，然后粘贴到链接服务中并重试。


### <a name="error-code--2108"></a>错误代码：2108

- **消息**：`Error calling the endpoint '%url;'. Response status code: '%code;'`

- **原因：** 某个根本问题（例如网络连接、DNS 失败、服务器证书验证或超时）导致请求失败。

- **建议**：使用 Fiddler/Postman 验证请求。


### <a name="error-code--2110"></a>错误代码：2110

- **消息**：`The linked service type '%linkedServiceType;' is not supported for '%executorType;' activities.`

- **原因：** 活动中指定了错误的链接服务。

- **建议**：请确保链接服务类型是活动支持的类型之一。 例如，对于 HDI 活动，链接服务类型可以是 HDInsight 或 HDInsightOnDemand。


### <a name="error-code--2111"></a>错误代码：2111

- **消息**：`The type of the property '%propertyName;' is incorrect. The expected type is %expectedType;.`

- **原因：** 提供的属性的类型不符合预期。

- **建议**：请修复属性的类型，然后重试。


### <a name="error-code--2112"></a>错误代码：2112

- **消息**：`The cloud type is unsupported or could not be determined for storage from the EndpointSuffix '%endpointSuffix;'.`

- **原因：** 云类型不受支持，或者对于 EndpointSuffix 中的存储无法确定。

- **建议**：请使用另一个云中的存储，然后重试。


### <a name="error-code--2128"></a>错误代码：2128

- **消息**：`No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **原因：** 网络连接、DNS 失败、服务器证书验证或超时。

- **建议**：验证尝试访问的终结点是否响应请求。 可以使用 Fiddler/Postman 等工具。



## <a name="custom"></a>“自定义”

下表适用于 Azure Batch。
      
### <a name="error-code--2500"></a>错误代码：2500

- **消息**：`Hit unexpected exception and execution failed.`

- **原因**：`Can't launch command, or the program returned an error code.`

- **建议**：确保可执行文件存在。 如果程序已启动，请确保已将 *stdout .txt* 和 *stderr.txt* 上传到存储帐户。 良好的做法是在代码中发出详细日志以进行调试。


### <a name="error-code--2501"></a>错误代码：2501

- **消息**：`Cannot access user batch account; please check batch account settings.`

- **原因：** Batch 访问密钥或池名称不正确。

- **建议**：验证链接服务中的池名称和 Batch 访问密钥。


### <a name="error-code--2502"></a>错误代码：2502

- **消息**：`Cannot access user storage account; please check storage account settings.`

- **原因：** 存储帐户名称或访问密钥不正确。

- **建议**：验证链接服务中的存储帐户名称和访问密钥。


### <a name="error-code--2504"></a>错误代码：2504

- **消息**：`Operation returned an invalid status code 'BadRequest'.`

- **原因：** 自定义活动的 folderPath 中的文件过多。 resourceFiles 的总大小不能超过 32,768 个字符。

- **建议**：删除不必要的文件。 或者压缩文件，并添加一个 unzip 命令来解压缩文件。 例如，使用 `powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;  $folder\yourProgram.exe`


### <a name="error-code--2505"></a>错误代码：2505

- **消息**：`Cannot create Shared Access Signature unless Account Key credentials are used.`

- **原因：** 自定义活动仅支持使用访问密钥的存储帐户。

- **建议**：请参阅错误说明。


### <a name="error-code--2507"></a>错误代码：2507

- **消息**：`The folder path does not exist or is empty: ...`

- **原因：** 存储帐户的指定路径下没有文件。

- **建议**：文件夹路径必须包含要运行的可执行文件。


### <a name="error-code--2508"></a>错误代码：2508

- **消息**：`There are duplicate files in the resource folder.`

- **原因：** folderPath 的不同子文件夹中存在多个同名的文件。

- **建议**：自定义活动在 folderPath 下平展文件夹结构。  如果需要保留文件夹结构，请压缩文件，并使用一个 unzip 命令将其解压缩到 Azure Batch 中。 例如，使用 `powershell.exe -nologo -noprofile   -command "& { Add-Type -A 'System.IO.Compression.FileSystem';   [IO.Compression.ZipFile]::ExtractToDirectory($zipFile, $folder); }" ;   $folder\yourProgram.exe`


### <a name="error-code--2509"></a>错误代码：2509

- **消息**：`Batch   url ... is invalid; it must be in Uri format.`

- **原因：** Batch URL 必须类似于 `https://mybatchaccount.chinaeast.batch.azure.cn`

- **建议**：请参阅错误说明。


### <a name="error-code--2510"></a>错误代码：2510

- **消息**：`An   error occurred while sending the request.`

- **原因：** Batch URL 无效。

- **建议**：验证 Batch URL。
            

## <a name="hdinsight"></a>HDInsight

### <a name="error-code--200"></a>错误代码：200

- **消息**：`Unexpected error happened: '%error;'.`

- **原因：** 出现内部服务问题。

- **建议**：请联系 ADF 支持部门以获得更多帮助。


### <a name="error-code--201"></a>错误代码：201

- **消息**：`JobType %jobType; is not found.`

- **原因：** 某个新作业类型不受 ADF 的支持。

- **建议**：请联系 ADF 支持团队以获得更多帮助。


### <a name="error-code--202"></a>错误代码：202

- **消息**：`Failed to create on demand HDI cluster. Cluster name or linked service name: '%clusterName;', error: '%message;'`

- **原因：** 错误消息应会显示问题的详细信息。

- **建议**：错误消息应有助于排查问题。 如果没有足够的信息，请联系 ADF 支持部门以获得更多帮助。


### <a name="error-code--203"></a>错误代码：203

- **消息**：`Failed to delete on demand HDI cluster. Cluster name or linked service name: '%clusterName;', error: '%message;'`

- **原因：** 错误消息应会显示问题的详细信息。

- **建议**：错误消息应有助于排查问题。 如果没有足够的信息，请联系 ADF 支持部门以获得更多帮助。


### <a name="error-code--204"></a>错误代码：204

- **消息**：`The resumption token is missing for runId '%runId;'.`

- **原因：** 出现内部服务问题。

- **建议**：请联系 ADF 支持部门以获得更多帮助。


### <a name="error-code--205"></a>错误代码：205

- **消息**：`Failed to prepare cluster for LinkedService '%linkedServiceName;', the current resource status is '%status;'.`

- **原因：** 创建 HDI 按需群集时出错。

- **建议**：请联系 ADF 支持部门以获得更多帮助。


### <a name="error-code--206"></a>错误代码：206

- **消息**：`The batch ID for Spark job is invalid. Please retry your job, and if the problem persists, contact the ADF support for further assistance.`

- **原因：** 导致此错误的服务出现了内部问题。

- **建议**：这可能是暂时性的问题。 请重试作业，如果问题依旧出现，请联系 ADF 支持部门以获得更多帮助。


### <a name="error-code--207"></a>错误代码：207

- **消息**：`Could not determine the region from the provided storage account. Please try using another primary storage account for the on demand HDI or contact ADF support team and provide the activity run ID.`

- **原因：** 尝试从主存储帐户确定区域时出现内部错误。

- **建议**：尝试另一个存储。 如果此解决方法不可接受，请联系 ADF 支持团队以获得更多帮助。


### <a name="error-code--208"></a>错误代码：208

- **消息**：`Service Principal or the MSI authenticator are not instantiated. Please consider providing a Service Principal in the HDI on demand linked service which has permissions to create an HDInsight cluster in the provided subscription and try again. In case if this is not an acceptable solution, contact ADF support team for further assistance.`

- **原因：** 尝试读取服务主体或实例化 MSI 身份验证时出现内部错误。

- **建议**：请考虑提供一个有权在所提供订阅中创建 HDInsight 群集的服务主体，然后重试。 如果此解决方法不可接受，请联系 ADF 支持团队以获得更多帮助。


### <a name="error-code--2300"></a>错误代码：2300

- **消息**：`Failed to submit the job '%jobId;' to the cluster '%cluster;'. Error: %errorMessage;.`

<br>

- **原因：** 如果错误消息包含类似于“无法解析远程名称”的消息，可能意味着提供的群集 URI 无效。


- **建议**：请确保未删除该群集，并且提供的 URI 正确。 在浏览器中打开该 URI 时，应会看到 Ambari UI。 如果该群集位于虚拟网络中，则 URI 应是专用 URI。 若要打开它，请使用同一虚拟网络中的 VM。 有关详细信息，请参阅[此文](/hdinsight/hdinsight-plan-virtual-network-deployment#directly-connect-to-apache-hadoop-services)。
                  

<br>

- **原因：** 如果错误消息包含类似于“已取消任务”的消息，则意味着作业提交超时。

- **建议**：这可能是普通的 HDInsight 连接问题或网络连接问题。 首先确认是否可以从任何浏览器打开 HDInsight Ambari UI。 确认凭据仍然有效。 如果使用自承载集成运行时 (IR)，请确保从安装了自承载 IR 的 VM 或计算机执行此操作。 然后再次尝试从数据工厂提交作业。 如果仍然失败，请联系数据工厂团队获得支持。

<br>

- **原因：** 如果错误消息中包含类似于“用户 admin 在 Ambari 中被锁定”或“未授权:Ambari 用户名或密码不正确”的消息，则意味着 HDInsight 的凭据不正确或已过期。

- **建议**：请更正凭据，然后重新部署链接服务。 首先在任何浏览器中打开群集 URI 并尝试登录，确保凭据在 HDInsight 中有效。 如果凭据无效，可从 Azure 门户重置凭据。

<br>

- **原因：** 如果错误消息包含类似于“502 - Web 服务器在充当网关或代理服务器时收到了无效响应”的消息，则此错误是 HDInsight 服务返回的。


- **建议**：查看 Azure HDInsight 故障排除文档，例如 https://hdinsight.github.io/ambari/ambari-ui-502-error.html 、 https://hdinsight.github.io/spark/spark-thriftserver-errors.html 、 https://docs.azure.cn/application-gateway/application-gateway-troubleshooting-502 。
                  

<br>

- **原因：** 如果错误消息包含类似于“无法为提交作业请求提供服务，因为 templeton 服务正忙于处理过多的提交作业请求”或“队列 root.joblauncher 已包含 500 个应用程序，无法接受应用程序提交”，则意味着同时将过多的作业提交到 HDInsight。

- **建议**：请考虑限制提交到 HDInsight 的并发作业数。 如果这些作业是同一活动提交的，请参阅“数据工厂活动并发性”。 更改触发器，将并发管道运行分散到不同的时间。 请参阅 HDInsight 文档，根据错误中的建议调整 templeton.parallellism.job.submit。


### <a name="error-code--2301"></a>错误代码：2301

- **消息**：`Could not get the status of the application '%physicalJobId;' from the HDInsight service. Received the following error: %message;. Please refer to HDInsight troubleshooting documentation or contact their support for further assistance.`

- **原因：** HDInsight 群集或服务出现问题。


- **建议**：如果 ADF 尝试获取正在运行的作业的状态时未收到 HDInsight 群集的响应，则会发生此错误。 此错误可能是群集本身的问题造成的，也可能是 HDInsight 服务中断造成的。 请参阅 https://docs.azure.cn/hdinsight/hdinsight-troubleshoot-guide 上的 HDInsight 故障排除文档，或联系支持人员以获得更多帮助。
                


### <a name="error-code--2302"></a>错误代码：2302

- **消息**：`Hadoop job failed with exit code '%exitCode;'. See '%logPath;/stderr' for more details. Alternatively, open the Ambari UI on the HDI cluster and find the logs for the job '%jobId;'. Contact HDInsight team for further support.`

- **原因：** 作业已提交到 HDI 群集，但在群集上失败。

- **建议**：遵循活动运行输出中的 Yarn 日志链接操作，并查看 HDI 输出中的错误。 如果需要，请联系 HDInsight 团队以获得支持。


### <a name="error-code--2303"></a>错误代码：2303

- **消息**：`Hadoop job failed with transient exit code '%exitCode;'. See '%logPath;/stderr' for more details. Alternatively, open the Ambari UI on the HDI cluster and find the logs for the job '%jobId;'. Try again or contact HDInsight team for further support.`

- **原因：** 作业已提交到 HDI 群集，但在群集上失败。

- **建议**：遵循活动运行输出中的 Yarn 日志链接操作，并查看 HDI 输出中的错误。 请重试，如果需要，请联系 HDInsight 团队以获得支持。


### <a name="error-code--2304"></a>错误代码：2304

- **消息**：`MSI authentication is not supported on storages for HDI activities.`

- **原因：** 为 HDI 链接服务或 HDI 活动中使用的存储链接服务配置了不受支持的 MSI 身份验证。

- **建议**：请为 HDI 链接服务或 HDI 活动中使用的存储帐户提供完整的连接字符串。


### <a name="error-code--2305"></a>错误代码：2305

- **消息**：`Failed to initialize the HDInsight client for the cluster '%cluster;'. Error: '%message;'`

- **原因：** HDI 群集的连接信息不正确、提供的用户无权执行所需的操作，或者 HDInsight 服务在响应来自 ADF 的请求时出现问题。

- **建议**：请确保用户信息正确。 此外，验证是否可以在安装 IR（如果使用自承载 IR）的 VM 上的浏览器中打开 HDI 群集的 Ambari UI，或者可以从任何计算机（如果使用 Azure IR）打开此 UI。


### <a name="error-code--2306"></a>错误代码：2306

- **消息**：`An invalid json is provided for script action '%scriptActionName;'. Error: '%message;'`

- **原因：** 为脚本操作提供的 JSON 无效。


- **建议**：错误消息应有助于识别问题。 请修复 JSON 配置，然后重试。 查看 https://docs.azure.cn/data-factory/compute-linked-services#azure-hdinsight-on-demand-linked-service 获取更多信息。
                


### <a name="error-code--2310"></a>错误代码：2310

- **消息**：`Failed to submit Spark job. Error: '%message;'`

- **原因：** ADF 已尝试使用 Livy API (livy/batch) 在 Spark 群集上创建批，但收到了错误。

- **建议**：请按照错误消息解决问题。 如果信息不足以解决问题，请联系 HDI 团队，并向他们提供可在 ADF“监视”页的活动运行输出中找到的 Batch ID 和作业 ID。


### <a name="error-code--2312"></a>错误代码：2312

- **消息**：`Spark job failed, batch id:%batchId;. Please follow the links in the activity run Output from ADF Monitoring page to troubleshoot the run on HDInsight Spark cluster. Please contact HDInsight support team for further assistance.`

- **原因：** 作业在 HDInsight Spark 群集上失败。

- **建议**：请遵循 ADF“监视”页上的活动运行输出中的链接，排查 HDInsight Spark 群集上的运行问题。 请联系 HDInsight 支持团队以获得更多帮助。


### <a name="error-code--2313"></a>错误代码：2313

- **消息**：`The batch with ID '%batchId;' was not found on Spark cluster. Open the Spark History UI and try to find it there. Contact HDInsight support for further assistance.`

- **原因：** 已在 HDInsight Spark 群集上删除该批。

- **建议**：排查 HDInsight Spark 群集上的批问题。 联系 HDInsight 支持部门以获得更多帮助。 


### <a name="error-code--2328"></a>错误代码：2328

- **消息**：`Failed to create the on demand HDI cluster. Cluster or linked service name: '%clusterName;', error: '%message;'`

- **原因**：`The error message should show the details of what went wrong.`

- **建议**：错误消息应有助于排查问题。


### <a name="error-code--2329"></a>错误代码：2329

- **消息**：`Failed to delete the on demand HDI cluster. Cluster or linked service name: '%clusterName;', error: '%message;'`

- **原因：** 错误消息应会显示问题的详细信息。

- **建议**：错误消息应有助于排查问题。


### <a name="error-code--2331"></a>错误代码：2331

- **消息**：`The file path should not be null or empty.`

- **原因：** 提供的文件路径为空。

- **建议**：请提供存在的文件路径。


### <a name="error-code--2340"></a>错误代码：2340

- **消息**：`HDInsightOnDemand linked service does not support execution via SelfHosted IR. Your IR name is '%IRName;'. Please select an Azure IR instead.`

- **原因：** HDInsightOnDemand 链接服务不支持通过 SelfHosted IR 执行。

- **建议**：请选择一个 Azure IR，然后重试。


### <a name="error-code--2341"></a>错误代码：2341

- **消息**：`HDInsight cluster URL '%clusterUrl;' is incorrect, it must be in URI format and the scheme must be 'https'.`

- **原因：** 提供的 URL 未采用正确的格式。

- **建议**：请修复群集 URL，然后重试。


### <a name="error-code--2342"></a>错误代码：2342

- **消息**：`Failed to connect to HDInsight cluster: '%errorMessage;'.`

- **原因：** 为群集提供了错误的凭据、存在网络配置或连接问题，或者 IR 在连接到群集时出现问题。

- **建议**：  
      1. 通过在浏览器中打开 HDInsight 群集的 Ambari UI，验证凭据是否正确。
      2. 如果群集位于 VNet 中，而使用的是自承载 IR，则 HDI URL 应是 VNet 中的专用 URL，这意味着，它应在群集名称后面包含“-int”。 例如，“https://mycluster.azurehdinsight.cn/ ”应更改为“https://mycluster-int.azurehdinsight.cn/ ”。
      2. 如果群集位于 VNet 中，而使用的是自承载 IR，并且已使用专用 URL，但连接仍然失败，则表示安装 IR 的 VM 在连接到 HDI 时出现了问题。 连接到安装 IR 的 VM，并在浏览器中打开 Ambari UI。 对群集使用专用 URL。 应该能够在浏览器中建立此连接。 如果不能，请联系 HDInsight 支持团队以获得更多帮助。
      3. 如果未使用自承载 IR，则 HDI 群集应可公开访问。 在浏览器中打开 Ambari UI，并确保它已打开。 如果群集或其上的服务出现任何问题，请联系 HDInsight 支持团队获得帮助。
      一般情况下，ADF 链接服务中使用的 HDI 群集 URL 必须可供 ADF IR（自承载 IR 或 Azure IR）访问，这样才能通过连接测试，并使运行正常工作。 可以通过在 VM 或任意公共计算机上的浏览器中打开该 URL，轻松地对此进行验证。
    


### <a name="error-code--2343"></a>错误代码：2343

- **消息**：`User name and password cannot be null or empty to connect to the HDInsight cluster.`

- **原因：** 用户名或密码为空。

- **建议**：提供正确的凭据以连接到 HDI，然后重试。


### <a name="error-code--2345"></a>错误代码：2345

- **消息**：`Failed to read the content of the hive script. Error: '%message;'`

- **原因：** 脚本文件不存在，或 ADF 无法连接到脚本所在的位置。

- **建议**：请验证脚本是否存在，以及关联的链接服务是否具有适当的连接凭据。


### <a name="error-code--2346"></a>错误代码：2346

- **消息**：`Failed to create ODBC connection to the HDI cluster with error message '%message;'.`

- **原因：** ADF 已尝试与 HDI 群集建立 ODBC 连接，但失败并出现错误。

- **建议**：错误消息和错误代码应有助于排查 ODBC 连接错误。 如果这些信息不足以解决问题，请联系 Azure HDInsight 团队获得支持。


### <a name="error-code--2347"></a>错误代码：2347

- **消息**：`Hive execution through ODBC failed with error message '%message;'.`

- **原因：** ADF 已通过 ODBC 连接将 Hive 脚本提交到 HDI 群集，但脚本在 HDI 上失败。

- **建议**：Hive 脚本执行在 HDI 群集上失败，错误消息和错误代码应有助于进行故障排除。 如果这些信息不足以解决问题，请联系 Azure HDInsight 团队获得支持。


### <a name="error-code--2348"></a>错误代码：2348

- **消息**：`The main storage has not been initialized. Please check the properties of the storage linked service in the HDI linked service.`

- **原因：** 未正确设置存储链接服务属性。

- **建议**：HDI 活动的主存储链接服务中仅支持完整的连接字符串。 请确保未使用 MSI 身份验证或应用程序。


### <a name="error-code--2350"></a>错误代码：2350

- **消息**：`Failed to prepare the files for the run '%jobId;'. HDI cluster: '%cluster;', Error: '%errorMessage;'`

- **原因：** 提供的凭据（用于连接到文件所在的存储）不正确，或存储中不存在文件。

- **建议**：此错误是在 ADF 针对 HDI 活动执行准备步骤时发生的。 ADF 在将作业提交到 HDI 之前，会尝试将文件复制到主存储。 请确保文件在提供的位置存在，并且存储连接正确。 ADF HDI 活动不支持 HDI 活动相关存储帐户中的 MSI 身份验证，因此请确保这些链接服务具有完整的密钥或使用 Azure Key Vault。


### <a name="error-code--2351"></a>错误代码：2351

- **消息**：`Could not open the file '%filePath;' in container/fileSystem '%container;'.`

- **原因：** 文件在指定的路径不存在。

- **建议**：请检查该文件是否确实存在，以及包含指向此文件的连接信息的链接服务是否具有正确的凭据。


### <a name="error-code--2352"></a>错误代码：2352

- **消息**：`The file storage has not been initialized. Please check the properties of the file storage linked service in the HDI activity.`

- **原因：** 未正确设置文件存储链接服务属性。

- **建议**：请确保已正确配置文件存储链接服务的属性。


### <a name="error-code--2353"></a>错误代码：2353

- **消息**：`The script storage has not been initialized. Please check the properties of the script storage linked service in the HDI activity.`

- **原因：** 未正确设置脚本存储链接服务属性。

- **建议**：请确保已正确配置脚本存储链接服务的属性。


### <a name="error-code--2354"></a>错误代码：2354

- **消息**：`The storage linked service type '%linkedServiceType;' is not supported for '%executorType;' activities for property '%linkedServicePropertyName;'.`

- **原因：** 活动不支持存储链接服务类型。

- **建议**：请确保所选的链接服务是活动支持的类型之一。 HDI 活动支持 AzureBlobStorage 和 AzureBlobFSStorage 链接服务。


### <a name="error-code--2355"></a>错误代码：2355

- **消息**：`The '%value' provided for commandEnvironment is incorrect. The expected value should be an array of strings where each string has the format CmdEnvVarName=CmdEnvVarValue.`

- **原因：** 为 commandEnvironment 提供的值不正确。

- **建议**：  
      请确保提供的值类似于：\"commandEnvironment\": [ \"variableName=variableValue\" ]，并且每个变量仅在列表中出现一次。
    


### <a name="error-code--2356"></a>错误代码：2356

- **消息**：`The commandEnvironment already contains a variable named '%variableName;'.`

- **原因：** 变量在 commandEnvironment 中提供了两次。

- **建议**：  
      请确保提供的值类似于：\"commandEnvironment\": [ \"variableName=variableValue\" ]，并且每个变量仅在列表中出现一次。
    


### <a name="error-code--2358"></a>错误代码：2358

- **消息**：`The value '%value;' for the required property 'TimeToLive' in the on demand HDInsight linked service '%linkedServiceName;' has invalid format. It should be a timespan between '00:05:00' and '24:00:00'.`

- **原因：** 为必需属性“TimeToLive”提供的值的格式无效。 

- **建议**：请更新值，使其处于建议的范围内，然后重试。


### <a name="error-code--2359"></a>错误代码：2359

- **消息**：`The value '%value;' for the property 'roles' is invalid. Expected types are 'zookeeper', 'headnode', and 'workernode'.`

- **原因：** 为属性“role”提供的值无效。

- **建议**：请将值更新为建议的值之一，然后重试。


### <a name="error-code--2360"></a>错误代码：2360

- **消息**：`The connection string in HCatalogLinkedService is invalid. Encountered an error while trying to parse: '%message;'.`

- **原因：** 为 HCatalogLinkedService 提供的连接字符串无效。

- **建议**：请将值更新为正确的 Azure SQL 连接字符串，然后重试。


### <a name="error-code--2361"></a>错误代码：2361

- **消息**：`Failed to create on demand HDI cluster. Cluster name is '%clusterName;'.`

- **原因：** 群集创建失败，并且 ADF 未收到 HDInsight 服务返回的错误。

- **建议**：打开 Azure 门户，尝试查找具有所提供名称的 HDI 资源，然后检查预配状态。 联系 HDInsight 支持团队以获得更多帮助。


### <a name="error-code--2362"></a>错误代码：2362

- **消息**：`Only Azure Blob storage accounts are supported as additional storages for HDInsight on demand linked service.`

- **原因：** 提供的附加存储不是 Azure Blob 存储。

- **建议**：提供 Azure Blob 存储帐户作为 HDInsight 按需链接服务的附加存储。



## <a name="web-activity"></a>Web 活动

### <a name="error-code--2128"></a>错误代码：2128

- **消息**：`No response from the endpoint. Possible causes: network connectivity, DNS failure, server certificate validation or timeout.`

- **原因：** 网络连接、DNS 失败、服务器证书验证或超时。

- **建议**：验证尝试访问的终结点是否响应请求。 可以使用 Fiddler/Postman 等工具。


### <a name="error-code--2108"></a>错误代码：2108

- **消息**：`Error calling the endpoint '%url;'. Response status code: '%code;'`

- **原因：** 某个根本问题（例如网络连接、DNS 失败、服务器证书验证或超时）导致请求失败。

- **建议**：使用 Fiddler/Postman 验证请求。
<br>


#### <a name="more-details"></a>更多详细信息
若要使用 Fiddler 创建受监视 Web 应用程序的 HTTP 会话：

1. 下载、安装并打开 [Fiddler](https://www.telerik.com/download/fiddler)。

1. 如果 Web 应用程序使用 HTTPS，请转到“工具” > “Fiddler 选项” > “HTTPS”。    选择“捕获 HTTPS 连接”和“解密 HTTPS 流量”。  

   ![Fiddler 选项](media/data-factory-troubleshoot-guide/fiddler-options.png)

1. 如果应用程序使用 TLS/SSL 证书，请将 Fiddler 证书添加到设备。 转到“工具” > “Fiddler 选项” > “HTTPS” > “操作” > “将根证书导出到桌面”。     

1. 转到“文件” > “捕获流量”来关闭捕获。   或者按 **F12**。

1. 清除浏览器缓存以删除所有已缓存的项；必须重新下载这些项。

1. 创建请求：

   1. 选择“编辑器”选项卡。 

   1. 设置 HTTP 方法和 URL。
   
   1. 根据需要添加标头和请求正文。

   1. 选择“执行”  。

1. 再次打开流量捕获，并在页面上完成相关的事务。

1. 转到“文件” > “保存” > “所有会话”。   

有关详细信息，请参阅 [Fiddler入门](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureFiddler)。

## <a name="next-steps"></a>后续步骤

尝试通过以下资源获得故障排除方面的更多帮助：

*  [MSDN 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home)



            
