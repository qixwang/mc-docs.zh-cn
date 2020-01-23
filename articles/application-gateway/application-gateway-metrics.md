---
title: 应用程序网关的 Azure Monitor 指标
description: 了解如何使用指标来监视应用程序网关的性能
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 01/15/2020
ms.author: v-junlch
ms.openlocfilehash: bd298d19de46857c33e0f7fa72edc479d25836a7
ms.sourcegitcommit: 48d51745ca18de7fa05b77501b4a9bf16cea2068
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76116924"
---
# <a name="metrics-for-application-gateway"></a>应用程序网关的指标

应用程序网关会将称为“指标”的数据点发布到 [Azure Monitor](/azure-monitor/overview)，使用户能够监视应用程序网关和后端实例的性能。 这些指标是一组有序时序数据中的数值，用于描述应用程序网关在特定时间的某种状况。 如果请求通过应用程序网关传送，则应用程序网关将会测量其指标并每隔 60 秒发送一次指标。 如果没有任何请求通过应用程序网关传送，或者指标没有数据，则不会报告指标。 有关详细信息，请参阅 [Azure Monitor 指标](/azure-monitor/platform/data-platform-metrics)。

## <a name="metrics-supported-by-application-gateway-v2-sku"></a>应用程序网关 V2 SKU 支持的指标

### <a name="timing-metrics"></a>计时指标

可以使用与请求和响应计时相关的以下指标。 分析特定侦听器的这些指标即可确定应用程序速度变慢是否是由于 WAN、应用程序网关、应用程序网关与后端应用程序之间的网络或者后端应用程序性能的问题。

> [!NOTE]
>
> 如果应用程序网关中有多个侦听器，则请在比较不同的延迟指标时始终按“侦听器”  维度进行筛选，以便获得有意义的推理结果。

- **客户端 RTT**

  客户端与应用程序网关之间的平均往返时间。 此指标指示建立连接和返回确认所用的时间。 

- **应用程序网关总时间**

  处理请求和发送响应平均花费的时间。 此时间按特定的平均时间间隔（从应用程序网关接收第一个 HTTP 请求字节到完成响应发送操作所需的时间）来计算。 必须注意，这通常包括应用程序网关处理时间、请求和响应数据包遍历网络的时间，以及后端服务器做出响应所花费的时间。
  
按侦听器筛选后，如果“客户端 RTT”远大于“应用程序网关总时间”，则可以推断客户端观察到的延迟是由于客户端和应用程序网关之间的网络连接性不好。   如果这两个延迟相当，则高延迟可能是由于以下任一原因导致的：应用程序网关、应用程序网关与后端应用程序之间的网络或者后端应用程序性能。

- **后端第一个字节响应时间**

  从开始与后端服务器建立连接，到收到响应标头的第一个字节的间隔时间，大致相当于后端服务器的处理时间

- **后端最后一个字节响应时间**

  从开始与后端服务器建立连接，到收到响应正文的最后一个字节的间隔时间
  
如果“应用程序网关总时间”远长于特定侦听器的“后端最后一个字节的响应时间”，则可以推断出延迟高可能是由于应用程序网关问题。   另一方面，如果这两个指标相当，则可能是应用程序网关和后端应用程序之间的网络或后端应用程序的性能出了问题。

- **后端连接时间**

  与后端应用程序建立连接所花费的时间。 在使用 SSL 时，它包括握手所用的时间。 请注意，此指标不同于其他延迟指标，因为这仅度量连接时间，因此不应与其他延迟直接进行大小比较。 不过，将“后端连接时间”的模式与其他延迟的模式进行比较即可了解是否可以通过应用程序网关和后端应用程序之间的网络差异推断出其他延迟会增加。  
  

### <a name="application-gateway-metrics"></a>应用程序网关指标

应用程序网关支持以下指标：

- **接收的字节数**

   应用程序网关从客户端收到的字节数

- **发送的字节数**

   应用程序网关向客户端发送的字节数

- **客户端 TLS 协议**

   与应用程序网关建立了连接的客户端发起的 TLS 和非 TLS 请求计数。 若要查看 TLS 协议分布，请按“TLS 协议”维度进行筛选。

- **当前容量单位数**

   消耗的容量单位数。 容量单位测量在固定价格的基础上按消耗量计收的费用。 容量单位有三个决定因素 - 计算单位、持久连接和吞吐量。 每个容量单位最多包括：1 个计算单位，或 2500 个持久连接，或 2.22-Mbps 吞吐量。

- **当前计算单位数**

   消耗的处理器容量计数。 影响计算单位的因素包括每秒 TLS 连接数、URL 重写计算和 WAF 规则处理。 

- **当前连接数**

   使用应用程序网关建立的当前连接计数

- **失败的请求数**

   应用程序网关已提供服务的失败请求计数。 可以进一步筛选请求计数，以显示每个/特定后端池 http 设置组合的计数。


- **响应状态**

   应用程序网关返回的 HTTP 响应状态。 可以进一步对响应状态代码分布进行归类来显示 2xx、3xx、4xx 和 5xx 类别的响应。

- **吞吐量**

   应用程序网关每秒提供的字节数

- **请求总数**

   应用程序网关已提供服务的成功请求计数。 可以进一步筛选请求计数，以显示每个/特定后端池 http 设置组合的计数。

- **Web 应用程序防火墙匹配的规则**

- **Web 应用程序防火墙触发的规则**

### <a name="backend-metrics"></a>后端指标

应用程序网关支持以下指标：

- **后端响应状态**

  后端返回的 HTTP 响应状态代码计数。 这不包括应用程序网关生成的任何响应代码。 可以进一步对响应状态代码分布进行归类来显示 2xx、3xx、4xx 和 5xx 类别的响应。

- **正常的主机计数**

  由运行状况探测判定为正常的后端数。 可以按每个后端池进行筛选来显示特定后端池中正常的/不正常的主机数。

- **不正常的主机计数**

  由运行状况探测判定为不正常的后端数。 可按每个后端池进行筛选，以显示特定后端池中不正常的主机。

## <a name="metrics-supported-by-application-gateway-v1-sku"></a>应用程序网关 V1 SKU 支持的指标

### <a name="application-gateway-metrics"></a>应用程序网关指标

应用程序网关支持以下指标：

- **CPU 使用率**

  显示分配给应用程序网关的 CPU 的使用率。  正常情况下，CPU 使用率不应经常超过 90%，因为这可能导致托管在应用程序网关后面的网站中出现延迟，并破坏客户端体验。 可以通过修改应用程序网关的配置（具体方法是：增加实例计数和/或移到更大的 SKU 大小）来间接控制或改进 CPU 使用率。

- **当前连接数**

  使用应用程序网关建立的当前连接计数

- **失败的请求数**

  应用程序网关已提供服务的失败请求计数。 可以进一步筛选请求计数，以显示每个/特定后端池 http 设置组合的计数。

- **响应状态**

  应用程序网关返回的 HTTP 响应状态。 可以进一步对响应状态代码分布进行归类来显示 2xx、3xx、4xx 和 5xx 类别的响应。

- **吞吐量**

  应用程序网关每秒提供的字节数

- **请求总数**

  应用程序网关已提供服务的成功请求计数。 可以进一步筛选请求计数，以显示每个/特定后端池 http 设置组合的计数。

- **Web 应用程序防火墙匹配的规则**

- **Web 应用程序防火墙触发的规则**

### <a name="backend-metrics"></a>后端指标

应用程序网关支持以下指标：

- **正常的主机计数**

  由运行状况探测判定为正常的后端数。 可以按每个后端池进行筛选来显示特定后端池中正常的/不正常的主机数。

- **不正常的主机计数**

  由运行状况探测判定为不正常的后端数。 可按每个后端池进行筛选，以显示特定后端池中不正常的主机。

## <a name="metrics-visualization"></a>指标可视化

浏览到应用程序网关，并在“监视”下选择“指标”   。 若要查看可用值，请选择“指标”下拉列表  。

在下图中可以看到过去 30 分钟显示的三个指标的示例：

[![](./media/application-gateway-diagnostics/figure5.png "Metric view")](./media/application-gateway-diagnostics/figure5-lb.png#lightbox)

若要查看当前的指标列表，请参阅 [Azure Monitor 支持的指标](../azure-monitor/platform/metrics-supported.md)。


## <a name="next-steps"></a>后续步骤

* 使用 [Azure Monitor 日志](../azure-monitor/insights/azure-networking-analytics.md)可视化计数器和事件日志。
* [Visualize your Azure Activity Log with Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx)（使用 Power BI 直观显示 Azure 活动日志）博客文章。
* [View and analyze Azure activity logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/)（在 Power BI 和其他组件中查看和分析 Azure 活动日志）博客文章。

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png

<!-- Update_Description: wording update -->