---
title: Azure Service Fabric 探测
description: 如何使用应用程序和服务清单文件在 Azure Service Fabric 中对运行情况探测进行建模。
ms.topic: conceptual
origin.date: 3/12/2020
ms.date: 05/06/2020
ms.author: v-yeche
ms.openlocfilehash: 6570a3ad8ebd605d120f2cf9db394b9eb7961a58
ms.sourcegitcommit: 81241aa44adbcac0764e2b5eb865b96ae56da6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/09/2020
ms.locfileid: "83002161"
---
# <a name="liveness-probe"></a>运行情况探测
从 7.1 版开始，Service Fabric 支持针对[容器化][containers-introduction-link]应用程序的运行情况探测机制。 运行情况探测可以用来通知容器化应用程序的运行情况，在应用程序未及时响应时会导致重启。
本文概述了如何通过清单文件定义运行情况探测。

继续阅读本文之前，建议先熟悉 [Service Fabric 应用程序模型][application-model-link]和 [Service Fabric 托管模型][hosting-model-link]。

> [!NOTE]
> 只有 NAT 网络模式下的容器支持运行情况探测。

## <a name="semantics"></a>语义
只能为每个容器指定 1 个运行情况探测，可以通过以下字段控制其行为：

* `initialDelaySeconds`：容器启动后开始执行探测的初始延迟秒数。 支持的值为整型。默认值为 0。 最小值为 0。

* `timeoutSeconds`：一个时间段（秒），在此时间之后，如果探测未成功完成，则会将其视为失败。 支持的值为整型。默认值为 1。 最小值为 1。

* `periodSeconds`：指定探测频率（以秒为单位）。 支持的值为整型。默认值为 10。 最小值为 1。

* `failureThreshold`：达到 FailureThreshold 后，容器会重启。 支持的值为整型。默认值为 3。 最小值为 1。

* `successThreshold`：失败后，探测必须成功执行 SuccessThreshold 次，才会将其视为成功。 支持的值为整型。默认值为 1。 最小值为 1。

某个时刻将有最多 1 个针对容器的探测。 如果探测在 timeoutSeconds  内未完成，则会一直等待并计数，直至达到 failureThreshold  。 

此外，ServiceFabric 还会引发有关 DeployedServicePackage 的以下探测[运行状况报告][health-introduction-link]：

* `Ok`：如果探测成功了 successThreshold  次，我们会将运行状况报告为“正常”。

* `Error`：如果探测 failureCount == failureThreshold  ，我们会在重启容器前报告“错误”。

* `Warning`： 
    1. 如果探测失败并且 failureCount < failureThreshold  ，我们会报告“警告”。 此运行状况报告会一直进行下去，直至 failureCount 达到 failureThreshold  或 **successThreshold**。
    2. 如果在失败后成功，我们仍然会报告“警告”，但也会更新后续的成功状态。

## <a name="specifying-liveness-probe"></a>指定运行情况探测

可以在 ApplicationManifest.xml 中的 ServiceManifestImport 下指定探测：

探测可以是下列项之一：

1. HTTP
2. TCP
3. Exec 

## <a name="http-probe"></a>HTTP 探测

对于 HTTP 探测，Service Fabric 会向指定的端口和路径发送 HTTP 请求。 返回代码大于或等于 200 且小于 400 表示成功。

下面是一个演示如何指定 HttpGet 探测的示例：

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <CodePackagePolicy CodePackageRef="Code">
        <Probes>
          <Probe Type="Liveness" FailureThreshold="5" SuccessThreshold="2" InitialDelaySeconds="10" PeriodSeconds="30" TimeoutSeconds="20">
            <HttpGet Path="/" Port="8081" Scheme="http">
              <HttpHeader Name="Foo" Value="Val"/>
              <HttpHeader Name="Bar" Value="val1"/>
            </HttpGet>
          </Probe>
        </Probes>
      </CodePackagePolicy>
    </Policies>
  </ServiceManifestImport>
```

HttpGet 探测有其他可设置的属性：

* `path`：要在 HTTP 请求中访问的路径。

* `port`：探测时要访问的端口。 范围为 1 到 65535。 必需。

* `scheme`：用于连接到代码包的方案。 如果设置为 HTTPS，则会跳过证书验证。 默认值为 HTTP

* `httpHeader`：要在请求中设置的标头。 可以指定多个标头。

* `host`：要连接到的主机 IP。

## <a name="tcp-probe"></a>TCP 探测

对于 TCP 探测，Service Fabric 会尝试使用指定的端口在容器上打开一个套接字。 如果它可以建立连接，则探测会被视为成功。 下面的示例展示了如何指定使用 TCP 套接字的探测：

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <CodePackagePolicy CodePackageRef="Code">
        <Probes>
          <Probe Type="Liveness" FailureThreshold="5" SuccessThreshold="2" InitialDelaySeconds="10" PeriodSeconds="30" TimeoutSeconds="20">
            <TcpSocket Port="8081"/>
          </Probe>
        </Probes>
      </CodePackagePolicy>
    </Policies>
  </ServiceManifestImport>
```

## <a name="exec-probe"></a>Exec 探测

此探测将向容器发出一个 exec 命令，并等待命令完成。

> [!NOTE]
> Exec 命令接受以逗号分隔的字符串。 示例中的以下命令适用于 Linux 容器。
> 如果尝试使用 Windows 容器，请使用 <Command>cmd</Command>

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <CodePackagePolicy CodePackageRef="Code">
        <Probes>
          <Probe Type="Liveness" FailureThreshold="5" SuccessThreshold="2" InitialDelaySeconds="10" PeriodSeconds="30" TimeoutSeconds="20">
            <Exec>
              <Command>ping,-c,2,localhost</Command>
            </Exec>
          </Probe>        
       </Probes>
      </CodePackagePolicy>
    </Policies>
  </ServiceManifestImport>
```

## <a name="next-steps"></a>后续步骤
请参阅以下文章，了解相关信息。
* [Service Fabric 和容器][containers-introduction-link]。

<!-- Links -->

[containers-introduction-link]: service-fabric-containers-overview.md
[health-introduction-link]: service-fabric-health-introduction.md
[application-model-link]: service-fabric-application-model.md
[hosting-model-link]: service-fabric-hosting-model.md

<!-- Update_Description: new article about probes codepackage -->
<!--NEW.date: 05/06/2020-->