---
title: 在 Azure 数据工厂中排查自承载集成运行时问题
description: 了解如何在 Azure 数据工厂中排查自承载集成运行时问题。
services: data-factory
author: WenJason
ms.service: data-factory
ms.topic: troubleshooting
origin.date: 06/24/2020
ms.date: 07/27/2020
ms.author: v-jay
ms.openlocfilehash: fb1bacd992a01ec23e9ffa3b1c440929e65e40f5
ms.sourcegitcommit: 0eaa82cf74477d26d06bdd8fb6e715e6ed1339c4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "86974333"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>排查自承载集成运行时问题

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

本文探讨 Azure 数据工厂中的自承载集成运行时的常用故障排除方法。

## <a name="common-errors-and-resolutions"></a>常见错误和解决方案

### <a name="error-message"></a>错误消息： 

`Self-hosted integration runtime can't connect to cloud service`

![自承载 IR 连接问题](media/self-hosted-integration-runtime-troubleshoot-guide/unable-to-connect-to-cloud-service.png)

#### <a name="cause"></a>原因 

自承载集成运行时无法连接到数据工厂服务（后端）。 此问题通常是防火墙中的网络设置导致的。

#### <a name="resolution"></a>解决方法

1. 检查集成运行时服务是否正在运行。
    
   ![自承载 IR 服务运行状态](media/self-hosted-integration-runtime-troubleshoot-guide/integration-runtime-service-running-status.png)
    
1. 如果服务正在运行，请转到步骤 3。

1. 如果自承载集成运行时上没有配置代理（这是默认设置），请在安装了自承载集成运行时的计算机上运行以下 PowerShell 命令：

    ```powershell
    (New-Object System.Net.WebClient).DownloadString("https://chinae2.frontend.datamovement.azure.cn/")
    ```
        
   > [!NOTE]     
   > 服务 URL 可能会有所不同，具体取决于数据工厂位置。 可以在“ADF UI” > “连接” > “集成运行时” > “编辑自承载 IR” > “节点” > “查看服务 URL”下找到服务 URL。
            
    下面是预期的响应：
            
    ![PowerShell 命令响应](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)
            
1. 如果未收到预期的响应，请根据情况使用以下方法之一：
            
    * 如果收到“无法解析远程名称”消息，则表明存在域名系统 (DNS) 问题。 请与网络团队联系以解决此问题。
    * 如果收到“ssl/tls 证书不受信任”消息，请检查 https://chinae2.frontend.datamovement.azure.cn/ 的证书在计算机上是否受信任，然后使用证书管理器安装公共证书。 此操作应该会缓解此问题。
    * 转到“Windows” > “事件查看器(日志)” > “应用程序和服务日志” > “Integration Runtime”，检查是否存在由 DNS、防火墙规则或公司网络设置导致的故障。 （如果发现此类故障，请强行关闭连接。）由于每个公司都有自定义的网络设置，因此请与网络小组联系以排查这些问题。

1. 如果在自承载集成运行时上配置了“代理”，请验证代理服务器是否可以访问服务终结点。 有关示例命令，请参阅 [PowerShell, web requests, and proxies](https://stackoverflow.com/questions/571429/powershell-web-requests-and-proxies)（PowerShell、Web 请求和代理）。    
                
    ```powershell
    $user = $env:username
    $webproxy = (get-itemproperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet
    Settings').ProxyServer
    $pwd = Read-Host "Password?" -assecurestring
    $proxy = new-object System.Net.WebProxy
    $proxy.Address = $webproxy
    $account = new-object System.Net.NetworkCredential($user,[Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($pwd)), "")
    $proxy.credentials = $account
    $url = "https://chinae2.frontend.datamovement.azure.cn/"
    $wc = new-object system.net.WebClient
    $wc.proxy = $proxy
    $webpage = $wc.DownloadData($url)
    $string = [System.Text.Encoding]::ASCII.GetString($webpage)
    $string
    ```

下面是预期的响应：
            
![PowerShell 命令响应 2](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)

> [!NOTE] 
> 代理注意事项：
> * 检查是否需要将代理服务器放在“安全接收方”列表中。 如果需要，请确保[这些域](/data-factory/data-movement-security-considerations#firewall-requirements-for-on-premisesprivate-network)在“安全接收方”列表中。
> * 检查 TLS/SSL 证书“chinae2.frontend.datamovement.azure.cn/”在代理服务器上是否受信任。
> * 如果在代理上使用 Active Directory 身份验证，请将服务帐户更改为可作为“Integration Runtime 服务”访问该代理的用户帐户。

### <a name="error-message"></a>错误消息： 
`Self-hosted integration runtime node/ logical SHIR is in Inactive/ "Running (Limited)" state`

#### <a name="cause"></a>原因 

自承载集成运行时节点的状态可能为“非活动”，如以下屏幕截图所示：

![非活动的自承载 IR 节点](media/self-hosted-integration-runtime-troubleshoot-guide/inactive-self-hosted-ir-node.png)

当节点无法相互通信时，会出现此行为。

#### <a name="resolution"></a>解决方法

1. 登录到节点托管的 VM。 在“应用程序和服务日志” > “Integration Runtime”下，打开事件查看器并筛选所有错误日志。

1. 检查错误日志是否包含以下错误： 
    
    ```
    System.ServiceModel.EndpointNotFoundException: Could not connect to net.tcp://xxxxxxx.bwld.com:8060/ExternalService.svc/WorkerManager. The connection attempt lasted for a time span of 00:00:00.9940994. TCP error code 10061: No connection could be made because the target machine actively refused it 10.2.4.10:8060. 
    System.Net.Sockets.SocketException: No connection could be made because the target machine actively refused it. 
    10.2.4.10:8060
    at System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)
    at System.Net.Sockets.Socket.Connect(EndPoint remoteEP)
    at System.ServiceModel.Channels.SocketConnectionInitiator.Connect(Uri uri, TimeSpan timeout)
    ```
       
1. 如果看到此错误，请在命令行上运行以下命令： 

   ```
   telnet 10.2.4.10 8060
   ```
   
1. 如果收到以下错误，请与 IT 部门联系以获得解决此问题的帮助。 成功进行 telnet 之后，如果集成运行时节点状态仍然存在问题，请联系 Azure 支持。
        
   ![命令行错误](media/self-hosted-integration-runtime-troubleshoot-guide/command-line-error.png)
        
1. 检查错误日志是否包含以下错误：

    ```
    Error log: Cannot connect to worker manager: net.tcp://xxxxxx:8060/ExternalService.svc/ No DNS entries exist for host azranlcir01r1. No such host is known Exception detail: System.ServiceModel.EndpointNotFoundException: No DNS entries exist for host xxxxx. ---> System.Net.Sockets.SocketException: No such host is known at System.Net.Dns.GetAddrInfo(String name) at System.Net.Dns.InternalGetHostByName(String hostName, Boolean includeIPv6) at System.Net.Dns.GetHostEntry(String hostNameOrAddress) at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri) --- End of inner exception stack trace --- Server stack trace: at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri)
    ```
    
1. 若要解决此问题，请尝试下面的一种或两种方法：
    - 将所有节点置于同一域中。
    - 在所有托管 VM 的主机文件中添加 IP 到主机映射。


## <a name="troubleshoot-connectivity-issue"></a>排查连接问题

### <a name="troubleshoot-connectivity-issue-between-self-hosted-ir-and-data-factory-or-self-hosted-ir-and-data-sourcesink"></a>排查自承载 IR 与数据工厂或自承载 IR 与数据源/接收器之间的连接问题

若要排查网络连接问题，应知道如何[收集网络跟踪](#how-to-collect-netmon-trace)，了解如何使用它，并在通过自承载 IR 在实际案例中应用 Netmon 工具之前[分析 netmon 跟踪](#how-to-analyze-netmon-trace)。

有时，在排查自承载 IR 与数据工厂之间的连接问题（例如下面的问题）时，会出现以下情况： 

![HTTP 请求失败](media/self-hosted-integration-runtime-troubleshoot-guide/http-request-error.png)

或者，在排查自承载 IR 与数据源/接收器之间的连接问题时，会遇到以下错误：

**错误消息：** 
`Copy failed with error:Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server: 'IP address'`

**错误消息：** 
`One or more errors occurred. An error occurred while sending the request. The underlying connection was closed: An unexpected error occurred on a receive. Unable to read data from the transport connection: An existing connection was forcibly closed by the remote host. An existing connection was forcibly closed by the remote host Activity ID.`

**解决方法：** 遇到以上问题时，请参考以下说明来进一步进行排查：

获取 netmon 跟踪并进一步进行分析。
- 首先，你可以设置筛选器来查看从服务器到客户端的任何重置。 在下面的示例中，你可以看到服务器端是数据工厂服务器。

    ![数据工厂服务器](media/self-hosted-integration-runtime-troubleshoot-guide/data-factory-server.png)

- 当你获得重置包时，可以顺着 TCP 来查找对话。

    ![查找对话](media/self-hosted-integration-runtime-troubleshoot-guide/find-conversation.png)

- 然后，你可以通过删除筛选器来获取客户端与数据工厂服务器之间的对话。

    ![获取对话](media/self-hosted-integration-runtime-troubleshoot-guide/get-conversation.png)
- 根据收集的 netmon 跟踪，我们可以判断 TTL (TimeToLive) 总计为 64。 根据[此文](https://packetpushers.net/ip-time-to-live-and-hop-limit-basics/)中提到的**默认 TTL 和跃点限制值**（摘录如下），我们可以确定是 Linux 系统重置了包并导致连接断开。

    默认 TTL 和跃点限制值在不同的操作系统中有所不同，下面是一些操作系统的默认值：
    - Linux 内核 2.4 (circa 2001)：对于 TCP、UDP 和 ICMP，该值为 255
    - Linux 内核 4.10 (2015)：对于 TCP、UDP 和 ICMP，该值为 64
    - Windows XP (2001)：对于 TCP、UDP 和 ICMP，该值为 128
    - Windows 10 (2015)：对于 TCP、UDP 和 ICMP，该值为 128
    - Windows Server 2008：对于 TCP、UDP 和 ICMP，该值为 128
    - Windows Server 2019 (2018)：对于 TCP、UDP 和 ICMP，该值为 128
    - macOS (2001)：对于 TCP、UDP 和 ICMP，该值为 64

    ![TTL 61](media/self-hosted-integration-runtime-troubleshoot-guide/ttl-61.png)
    
    但在上面的示例中，它显示为 61 而不是 64，因为当网络包到达目标时，它需要通过不同的跃点，例如路由器/网络设备。 将在最终 TTL 中扣减路由器/网络设备的数目。
    在本例中，我们可以看到，可以从 Linux 系统以 TTL 64 发送重置。

- 我们需要检查从自承载 IR 算起的第四个跃点，以确认重置设备可能来自何处。
 
    *来自 Linux 系统 A 的网络包的 TTL 64 -> B 的 TTL (64 - 1 = 63) -> C 的 TTL (63 - 1 = 62) -> 自承载 IR 的 TTL (62 - 1 = 61)*

- 在理想情况下，TTL 将为 128，这意味着 Windows 系统正在运行我们的数据工厂。 如以下示例中所示，128 – 107 = 21 个跃点，这意味着在 TCP 3 握手期间，包从数据工厂发送到自承载 IR 共经过了 21 个跃点。
 
    ![TTL 107](media/self-hosted-integration-runtime-troubleshoot-guide/ttl-107.png)

    因此，你需要与网络团队合作，以检查从自承载 IR 算起的第四个跃点是什么。 如果它是作为 Linux 系统存在的防火墙，请检查日志来确认设备为何在 TCP 3 握手后重置了包。 但是，如果不确定要在何处进行调查，请尝试一起获取自承载 IR 和防火墙在有问题的时间内的 netmon 跟踪，以找出可能重置此包并导致连接断开的设备。 在这种情况下，你还需要与网络团队合作才能继续进行调查。

### <a name="how-to-collect-netmon-trace"></a>如何收集 netmon 跟踪

1. 从[此网站](https://www.microsoft.com/en-sg/download/details.aspx?id=4865)下载 Netmon 工具，并将其安装在服务器计算机（有问题的任何服务器）和客户端（如自承载 IR）上。

2. 创建一个文件夹，例如在以下路径中创建：D:\netmon。 请确保它有足够的空间来保存日志。

3. 捕获 IP 和端口信息。 
    1. 启动 CMD 提示符。
    2. 选择“以管理员身份运行”并运行以下命令：
       
        ```
        Ipconfig /all >D:\netmon\IP.txt
        netstat -abno > D:\netmon\ServerNetstat.txt
        ```

4. 捕获 Netmon 跟踪（网络包）。
    1. 启动 CMD 提示符。
    2. 选择“以管理员身份运行”并运行以下命令：
        
        ```
        cd C:\Program Files\Microsoft Network Monitor 3
        ```
    3. 可以使用三个不同的命令来捕获网络包：
        - 选项 A：轮循式 File 命令（只捕获一个文件，将覆盖旧日志）。

            ```
            nmcap /network * /capture /file D:\netmon\ServerConnection.cap:200M
            ```         
        - 选项 B：链式 File 命令（如果达到 200 MB 的限制，将创建新文件）。
        
            ```
            nmcap /network * /capture /file D:\netmon\ServerConnection.chn:200M
            ```          
        - 选项 C：计划的 File 命令。

            ```
            nmcap /network * /capture /StartWhen /Time 10:30:00 AM 10/28/2011 /StopWhen /Time 11:30:00 AM 10/28/2011 /file D:\netmon\ServerConnection.chn:200M
            ```  

5. 按 **Ctrl+C** 可停止捕获 Netmon 跟踪。
 
> [!NOTE]
> 如果只能在客户端计算机上收集 netmon 跟踪，请获取有助于分析跟踪的服务器 IP 地址。

### <a name="how-to-analyze-netmon-trace"></a>如何分析 netmon 跟踪

如果收集到以上 netmon 跟踪，则在尝试 telnet **8.8.8.8 888** 时会看到以下跟踪：

![netmon 跟踪 1](media/self-hosted-integration-runtime-troubleshoot-guide/netmon-trace-1.png)

![netmon 跟踪 2](media/self-hosted-integration-runtime-troubleshoot-guide/netmon-trace-2.png)
 

这意味着你无法基于端口 **888** 与服务器端 **8.8.8.8** 建立 TCP 连接，因此你会看到两个额外的 **SynReTransmit** 包。 由于源 **SELF-HOST2** 在发送第一个包时无法建立到 **8.8.8.8** 的连接，因此它将继续建立连接。

> [!TIP]
> - 你可以单击“加载筛选器” -> “标准筛选器” -> “地址” -> “IPv4 地址”。   
> - 输入 **IPv4.Address == 8.8.8.8** 作为筛选器，然后单击“应用”。 之后，你将只能看到从本地计算机到目标 **8.8.8.8** 的通信。

![筛选器地址 1](media/self-hosted-integration-runtime-troubleshoot-guide/filter-addresses-1.png)
        
![筛选器地址 2](media/self-hosted-integration-runtime-troubleshoot-guide/filter-addresses-2.png)

下面的示例演示了一个优秀方案。 

- 如果 Telnet **8.8.8.8 53** 正常运行且没有任何问题，则可以看到发生了 TCP 3 握手，然后会话完成了 TCP 4 握手。

    ![优秀方案示例 1](media/self-hosted-integration-runtime-troubleshoot-guide/good-scenario-1.png)
     
    ![优秀方案示例 2](media/self-hosted-integration-runtime-troubleshoot-guide/good-scenario-2.png)

- 基于上面的 TCP 3 握手，你可以看到以下工作流：

    ![TCP 3 握手工作流](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-3-handshake-workflow.png)
 
- 完成会话所需的 TCP 4 握手及其工作流将如下所示：

    ![TCP 4 握手](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-4-handshake.png)

    ![TCP 4 握手工作流](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-4-handshake-workflow.png) 


## <a name="next-steps"></a>后续步骤

有关故障排除的更多帮助，请尝试以下资源：

*  [MSDN 论坛](https://social.msdn.microsoft.com/Forums/zh-CN/home)
*  [数据工厂 Stack Overflow 论坛](https://stackoverflow.com/questions/tagged/azure-data-factory)
