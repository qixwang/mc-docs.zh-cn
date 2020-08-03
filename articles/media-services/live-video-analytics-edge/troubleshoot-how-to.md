---
title: 对 IoT Edge 上的实时视频分析进行故障排除 - Azure
description: 本文介绍对 IoT Edge 上的实时视频分析进行故障排除的步骤。
ms.topic: how-to
origin.date: 05/24/2020
ms.date: 07/27/2020
ms.openlocfilehash: 01ab9e2d725c2a2f30f91a86d1dfa904ffa58ff6
ms.sourcegitcommit: 091c672fa448b556f4c2c3979e006102d423e9d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2020
ms.locfileid: "87162745"
---
# <a name="troubleshoot-live-video-analytics-on-iot-edge"></a>对 IoT Edge 上的实时视频分析进行故障排除

本文介绍对 IoT Edge 上的实时视频分析进行故障排除的步骤。

## <a name="troubleshoot-deployment-issues"></a>排查部署问题

### <a name="diagnostics"></a>诊断

在部署实时视频分析的过程中，你将设置 Azure 资源，如 IoT 中心和 IoT Edge 设备。 作为诊断问题的第一步，请务必确保按照以下说明正确设置边缘：

1. [运行“check”命令](/iot-edge/troubleshoot#run-the-check-command)
1. [检查 IoT Edge 版本](/iot-edge/troubleshoot#check-your-iot-edge-version)
1. [检查 IoT Edge 安全管理器的状态及其日志](/iot-edge/troubleshoot#check-the-status-of-the-iot-edge-security-manager-and-its-logs)
1. [查看通过 IoT Edge 中心的消息](/iot-edge/troubleshoot#view-the-messages-going-through-the-iot-edge-hub)
1. [重启容器](/iot-edge/troubleshoot#restart-containers)
1. [检查防火墙和端口配置规则](/iot-edge/troubleshoot#check-your-firewall-and-port-configuration-rules)

### <a name="pre-deployment-issues"></a>部署前的问题

如果边缘基础结构正常，则可以查找部署清单文件的问题。 若要在边缘设备上与任何其他 IoT 模块一起部署 IoT Edge 模块上的实时视频分析，请使用包含边缘中心、边缘代理和其他模块及其属性的部署清单。 如果 JSON 的格式不正确，可能会出现如下错误： 

```
az iot edge set-modules --hub-name <iot-hub-name> --device-id lva-sample-device --content <path-to-deployment_manifest.json>
```

未能分析参数“content”的文件“<deployment manifest.json>”中的 json，出现异常：“额外数据：第 101 行第 1 列 (char 5325)”

如果遇到此错误，则建议检查 JSON 文件中是否缺少括号或存在文件结构的其他问题。 可以使用客户端（如 [Notepad++ 与 JSON 查看器插件](https://riptutorial.com/notepadplusplus/example/18201/json-viewer)）或联机工具（如 https://jsonformatter.curiousconcept.com/ ）来验证文件结构。

### <a name="deployment--diagnose-with-media-graph-direct-methods"></a>部署 - 用媒体图直接方法诊断 

在边缘设备上正确部署 IoT Edge 模块上的实时视频分析后，可以通过调用[直接方法](direct-methods.md)来创建和运行媒体图。 可以通过直接方法使用门户来运行媒体图诊断：

1. 通过门户，转到连接到边缘设备的 IoT 中心。
    1. 进入 IoT 中心边栏选项卡后，查找“自动设备管理”->“IoT Edge”。
    1. 单击“IoT Edge”应显示边缘设备列表。 选择想要诊断的设备。
         
        ![边缘设备](./media/troubleshoot-how-to/lva-sample-device.png)
    1. 检查响应代码是否为 200-OK。 [IoT Edge 运行时](/iot-edge/iot-edge-runtime)有各种其他响应代码，例如：
        1. 400 - 部署配置格式不正确或无效。
        1. 417 - 没有为设备设置部署配置。
        1. 412 - 部署配置中的架构版本无效。
        1. 406 - IoT Edge 设备脱机或不发送状态报告。
        1. 500 - IoT Edge 运行时中出现了一个错误。
    1. 单击该设备还应显示包含已部署的预期 IoT Edge 模块及其状态的列表
    1. 如果“在部署中指定”和“由设备报告”列指示“是”，则可以针对 IoT Edge 模块上的实时视频分析调用直接方法。 单击该模块后，会转到一个屏幕，可以在其中查看所需的属性和报告的属性，并可以调用直接方法。 
        1. 检查报告的属性和所需属性有助于了解模块属性是否已与部署同步。 如果没有同步，则可以重启边缘 
        1. 使用[直接方法](direct-methods.md)指南来调用一些方法，尤其是一些简单的方法，如 GraphTopologyList。 本指南还指定了所需的请求和响应有效负载以及错误代码。 简单的直接方法成功后，可以确保实时视频分析边缘模块功能正常。
        
        ![直接方法](./media/troubleshoot-how-to/direct-method.png) 
1. 如果收到状态 501 代码，请检查直接方法名称是否正确。 如果方法名称和请求有效负载准确，则应返回结果，并显示成功代码 =200。 如果请求有效负载不准确，将显示状态 =400 以及指示错误代码和消息的响应有效负载，这些错误代码和消息应该有助于诊断直接方法调用的问题。 

### <a name="post-deployment--diagnose-logs-for-issues-during-run"></a>后期部署 - 在运行期间诊断日志中的问题 

边缘模块的容器日志应具有诊断<!--<todo:add link to diagnostics doc>--> 信息，这些信息应该有助于在模块运行时期间调试问题。 可以[检查容器日志中是否有问题](/iot-edge/troubleshoot#check-container-logs-for-issues)并进行自我诊断，但如果执行了上述所有检查但仍遇到问题，请[使用“support bundle”命令](/iot-edge/troubleshoot#gather-debug-information-with-support-bundle-command)从 IoT Edge 设备中收集日志，Azure 团队可以对其进行进一步分析。 可以与我们[联系](https://portal.azure.cn/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)以获取支持，并提交收集的日志。

## <a name="common-error-resolutions"></a>常见错误解决方法

实时视频分析作为 IoT Edge 模块部署到边缘设备上，并且与 IoT Edge 代理和中心模块协作。 在部署实时视频分析时会遇到的一些常见错误是由底层 IoT 基础结构的问题导致的。 IoT Edge 代理和中心的一些常见错误包括：

1. [IoT Edge 代理在大约一分钟后停止](/iot-edge/troubleshoot-common-errors#iot-edge-agent-stops-after-about-a-minute)。
1. [IoT Edge 代理无法访问某个模块的映像 (403)](/iot-edge/troubleshoot-common-errors#iot-edge-agent-cant-access-a-modules-image-403)。
1. [Edge 代理模块报告“配置文件为空”，且设备上不会启动任何模块](/iot-edge/troubleshoot-common-errors#edge-agent-module-reports-empty-config-file-and-no-modules-start-on-the-device)。
1. [IoT Edge 中心未能启动](/iot-edge/troubleshoot-common-errors#iot-edge-hub-fails-to-start)。
1. [由于主机名无效，IoT Edge 安全守护程序失败](/iot-edge/troubleshoot-common-errors#iot-edge-security-daemon-fails-with-an-invalid-hostname)。
1. [实时视频分析或任何其他自定义 IoT Edge 模块无法将消息发送到边缘中心，出现 404 错误](/iot-edge/troubleshoot-common-errors#iot-edge-module-fails-to-send-a-message-to-edgehub-with-404-error)。
1. [IoT Edge 模块部署成功后，会从设备中消失](/iot-edge/troubleshoot-common-errors#iot-edge-module-deploys-successfully-then-disappears-from-device)。

### <a name="edge-set-up-script-issues"></a>边缘设置脚本问题

作为我们的文档的一部分，我们提供了[设置脚本](https://github.com/Azure/live-video-analytics/tree/master/edge/setup)来部署边缘和云资源，以便开始使用实时视频分析边缘。 在本部分中，我们捕获了你可能会遇到的有关脚本的错误，并介绍如何调试它们。

脚本运行部分创建几个资源，但失败，并出现以下消息：

```
registering device...

Unable to load extension 'eventgrid: unrecognized kwargs: ['min_profile']'. Use --debug for more information.
The command failed with an unexpected error. Here is the traceback:

No module named 'azure.mgmt.iothub.iot_hub_client'
Traceback (most recent call last):
File "/opt/az/lib/python3.6/site-packages/knack/cli.py", line 215, in invoke
  cmd_result = self.invocation.execute(args)
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 631, in execute
  raise ex
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 695, in _run_jobs_serially
  results.append(self._run_job(expanded_arg, cmd_copy))
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 688, in _run_job
  six.reraise(*sys.exc_info())
File "/opt/az/lib/python3.6/site-packages/six.py", line 693, in reraise
  raise value
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 665, in _run_job
  result = cmd_copy(params)
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/commands/__init__.py", line 324, in __call__
  return self.handler(*args, **kwargs)
File "/opt/az/lib/python3.6/site-packages/azure/cli/core/__init__.py", line 574, in default_command_handler
  return op(**command_args)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/operations/hub.py", line 75, in iot_device_list
  result = iot_query(cmd, query, hub_name, top, resource_group_name, login=login)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/operations/hub.py", line 45, in iot_query
  target = get_iot_hub_connection_string(cmd, hub_name, resource_group_name, login=login)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/common/_azure.py", line 112, in get_iot_hub_connection_string
  client = iot_hub_service_factory(cmd.cli_ctx)
File "/home/.azure/cliextensions/azure-cli-iot-ext/azext_iot/_factory.py", line 28, in iot_hub_service_factory
  from azure.mgmt.iothub.iot_hub_client import IotHubClient
ModuleNotFoundError: No module named 'azure.mgmt.iothub.iot_hub_client'
```
    
解决此问题：

1. 运行以下命令：

    ```
    az --version
    ```
1. 确保安装了以下扩展。 在编写本指南时，扩展的版本如下所示：

    |||
    |---|---|
    |azure-cli   |      2.5.1*|
    |command-modules-nspkg         |   2.0.3|
    |core  |    2.5.1*|
    |nspkg    | 3.0.4|
    |telemetry| 1.0.4|
    |扩展：    ||
    |storage-preview          |     0.2.10|
    |azure-cli-iot-ext          |    0.8.9|
    |eventgrid| 0.4.9|
    |azure-iot                       | 0.9.2|
1. 如果任何扩展早于以上版本号，请使用此命令将扩展更新到最新版本：

    ```
    az extension update --name <Extension name>
    ```

    例如： `az extension update --name azure-iot`

### <a name="sample-app-issues"></a>示例应用问题

在发布过程中，我们提供了一些 .NET 示例代码来引导我们的开发人员社区。 在本部分中，我们捕获了在运行示例代码时可能会遇到的错误，并介绍如何调试此类错误。

1. 在直接方法调用中，Program.cs 失败并出现以下错误：

    ```
    Unhandled exception. Microsoft.Azure.Devices.Common.Exceptions.UnauthorizedException: {"Message":"{\"errorCode\":401002,\"trackingId\":\"b1da85801b2e4faf951a2291a2c467c3-G:32-TimeStamp:04/06/2020 17:15:11\",\"message\":\"Unauthorized\",\"timestampUtc\":\"2020-04-06T17:15:11.6990676Z\"}","ExceptionMessage":""}
    
        at Microsoft.Azure.Devices.HttpClientHelper.ExecuteAsync(HttpClient httpClient, HttpMethod httpMethod, Uri requestUri, Func`3 modifyRequestMessageAsync, Func`2 isMappedToException, Func`3 processResponseMessageAsync, IDictionary`2 errorMappingOverrides, CancellationToken cancellationToken)
    
        at Microsoft.Azure.Devices.HttpClientHelper.ExecuteAsync(HttpMethod httpMethod, Uri requestUri, Func`3 modifyRequestMessageAsync, Func`3 processResponseMessageAsync, IDictionary`2 errorMappingOverrides, CancellationToken cancellationToken)
        
        at Microsoft.Azure.Devices.HttpClientHelper.PostAsync[T,T2](Uri requestUri, T entity, TimeSpan operationTimeout, IDictionary`2 errorMappingOverrides, IDictionary`2 customHeaders, CancellationToken cancellationToken)…
    ```

    1. 确保在 VS Code 环境中安装了 [Azure IoT 工具](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)，并连接到 IoT 中心设置。 （Ctrl+shift+P，然后选择“选择 IoT 中心方法”以连接到订阅和 IoT 中心）
1. 检查是否可以通过 VS Code 调用边缘模块上的直接方法（例如，使用以下有效负载 { "@apiVersion":"1.0"} 调用 GraphToplogyList），应返回以下响应。 

    ```
    {
      "status": 200,
      "payload": {
        "values": [
          {…
    …}
          ]
        }
    }
    ```

    ![Visual Studio Code](./media/troubleshoot-how-to/visual-studio-code1.png)
1. 如果上述操作失败，请尝试以下操作：
    1. 转到边缘设备上的命令提示符，然后键入。
    
    ```
    sudo systemctl restart iotedge
    ```

    这会重启边缘设备和所有模块。 请等待几分钟，然后运行以下内容以确认模块正在运行，然后再次尝试使用 DirectMethod。

    ```
    sudo iotedge list
    ```
    1. 如果上述操作也失败，请尝试重新启动 VM 或计算机。
    1. 如果所有操作均失败，请运行以下内容，以获取 ZIP 文件，其中所有[相关日志](/iot-edge/troubleshoot#gather-debug-information-with-support-bundle-command)会附加到该[支持票证](https://portal.azure.cn/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)。

    ```
    sudo iotedge support-bundle --since 2h
    ```
1. 如果收到错误响应 400 代码，请确保按照[直接方法](direct-methods.md)指南，为方法调用有效负载设置了正确的格式。
1. 如果收到状态 200 代码，则表示中心运行正常，模块部署正确且正常响应。 下一步是检查应用配置是否准确。 应用配置包含 appsettings.json 文件中的以下字段。 请仔细检查 deviceId 和 moduleId 是否准确。 检查此项的一种简单方法是通过 VSCode 中的 Azure IoT 中心扩展部分。 appsettings.json 文件中的值和 IoT 中心部分应该匹配。
    
    ```
    {
        "IoThubConnectionString" : 
        "deviceId" : 
        "moduleId" : 
    }
    ```

    ![IoT 中心](./media/troubleshoot-how-to/iot-hub.png)

1. 最后，请确保在 appsettings.json 中提供 IoT 中心连接字符串，而不是 IoT 中心设备连接字符串，因为它们的[格式](https://devblogs.microsoft.com/iotdev/understand-different-connection-strings-in-azure-iot-hub/)不同。

### <a name="live-video-analytics-working-with-external-modules"></a>用于外部模块的实时视频分析

通过 HTTP 扩展处理器的实时视频分析可以扩展媒体图，以使用 REST 通过 HTTP 发送和接收来自其他 IoT Edge 模块的数据。  作为[特定示例](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/httpExtension)，媒体图可以将视频帧作为图像发送到外部推理模块（如 Yolo V3），并接收基于 JSON 的分析结果。 在这种拓扑中，事件的最终目标主要是 IoT 中心。 如果在中心上看不到推理事件，请检查以下各项：

1. 检查媒体图要发布到的中心是否与要检查的中心相同。 有时，创建多个部署时，最终将会获得多个中心，并可能会错误地检查错误的事件中心。
1. 通过 VSCode 检查是否已部署并正在运行外部模块。 在此处的示例图中，rtspsim 和 cv 是在 lvaEdge 模块外部运行的 IoT Edge 模块。

    ![IoT 中心](./media/troubleshoot-how-to/iot-hub.png)
1. 检查是否要将事件发送到正确的 URL 终结点。 外部 AI 容器公开一个 URL 和一个其接收使用的端口，并返回 POST 请求中的数据。 此 URL 被指定为 Http 扩展处理器的 endpoint: url 属性。 如[拓扑 url](https://github.com/Azure/live-video-analytics/blob/master/MediaGraph/topologies/httpExtension/topology.json) 中所示，设置为推理 url 参数。 确保参数(http://yolov3/score) 的默认值或传入的值是准确的，可以使用 curl 来测试它是否正常工作。  
    1. 例如，在本地计算机上运行的 yolo v3 容器，容器的 IP 地址是 172.17.0.3（使用 docker 检查以查找 IP 地址）。

    ```
    curl -X POST http://172.17.0.3/score -H "Content-Type: image/jpeg" --data-binary @<fullpath to jpg>
    ```

    返回的结果：

    ```
    {"inferences": [{"type": "entity", "entity": {"tag": {"value": "car", "confidence": 0.8668569922447205}, "box": {"l": 0.3853073438008626, "t": 0.6063712999658677, "w": 0.04174524943033854, "h": 0.02989496027381675}}}]}
    ```

1. 如果正在使用 Http 扩展处理器运行图的一个或多个实例，则在每个 Http 扩展处理器之前都应有一个帧速率筛选器来管理视频源的每秒帧数 (fps)。 在某些情况下，边缘计算机的 CPU/内存使用率很高，可能会丢失某些推理事件。 若要解决此问题，请在帧速率筛选器上将 maximumFps 属性设置为低值。 可以在图的每个实例上将其设置为 0.5 ("maximumFps":0.5 )，然后重新运行以检查中心上的推理事件。
    1. 另外，还可以使用更高的 CPU 和内存来获取功能更强大的边缘计算机。
    
### <a name="multiple-direct-methods-in-parallel--timeout-failure"></a>并行的多个直接方法 - 超时失败 

IoT Edge 上的实时视频分析提供了一种基于直接方法的编程模型，该模型支持设置多个拓扑和多个图形实例。 在拓扑和图形设置的过程中，你将在边缘模块上调用多个直接方法调用。 如果并行调用了多个方法调用，特别是启动和停止图形的方法调用，则可能会遇到一些超时故障，如下面所示。 

程序集初始化方法 Microsoft.Media.LiveVideoAnalytics.Test.Feature.Edge.AssemblyInitializer.InitializeAssemblyAsync 引发了异常。 Microsoft.Azure.Devices.Common.Exceptions.IotHubException：Microsoft.Azure.Devices.Common.Exceptions.IotHubException：<br/> `{"Message":"{\"errorCode\":504101,\"trackingId\":\"55b1d7845498428593c2738d94442607-G:32-TimeStamp:05/15/2020 20:43:10-G:10-TimeStamp:05/15/2020 20:43:10\",\"message\":\"Timed out waiting for the response from device.\",\"info\":{},\"timestampUtc\":\"2020-05-15T20:43:10.3899553Z\"}","ExceptionMessage":""}. Aborting test execution. `

我们建议不要以并行方式调用直接方法，而是按顺序调用，即仅在上一个直接方法调用完成后才调用下一个直接方法。 

## <a name="next-steps"></a>后续步骤

[教程：将基于事件的视频录制到云中并从云中播放](event-based-video-recording-tutorial.md)
