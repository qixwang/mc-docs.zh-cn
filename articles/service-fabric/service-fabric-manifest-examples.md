---
title: Azure Service Fabric 应用程序清单示例
description: 了解如何为 Service Fabric 应用程序配置应用程序和服务清单设置。
author: rockboyfor
ms.topic: conceptual
origin.date: 06/11/2018
ms.date: 01/13/2020
ms.author: v-yeche
ms.openlocfilehash: 46e410544668151bb60b0c4ce17747671560aed2
ms.sourcegitcommit: 713136bd0b1df6d9da98eb1da7b9c3cee7fd0cee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75742493"
---
# <a name="service-fabric-application-and-service-manifest-examples"></a>Service Fabric 应用程序和服务清单示例
此部分包含应用程序和服务清单的示例。 这些示例不是要演示重要的方案，而是演示可以使用的不同设置以及如何使用它们。 

下面是所示功能的索引及其所在的示例清单。

|功能|清单|
|---|---|
|[资源调控](service-fabric-resource-governance.md)|[Reliable Services 应用程序清单](service-fabric-manifest-example-reliable-services-app.md#application-manifest)、[容器应用程序清单](service-fabric-manifest-example-container-app.md#application-manifest)|
|[使用本地管理员帐户运行服务](service-fabric-application-runas-security.md)|[Reliable Services 应用程序清单](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[将默认策略应用到所有服务代码包](service-fabric-application-runas-security.md#apply-a-default-policy-to-all-service-code-packages)|[Reliable Services 应用程序清单](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[创建用户和组主体](service-fabric-application-runas-security.md)|[Reliable Services 应用程序清单](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|在服务实例之间共享数据包|[Reliable Services 应用程序清单](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[重写服务终结点](service-fabric-service-manifest-resources.md#overriding-endpoints-in-servicemanifestxml)|[Reliable Services 应用程序清单](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[在服务启动时运行脚本](service-fabric-run-script-at-service-startup.md)|[VotingWeb 服务清单](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[定义 HTTPS 终结点](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#define-an-https-endpoint-in-the-service-manifest)|[VotingWeb 服务清单](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[声明配置包](service-fabric-application-and-service-manifests.md)|[VotingData 服务清单](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[声明数据包](service-fabric-application-and-service-manifests.md)|[VotingData 服务清单](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[重写环境变量](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[容器应用程序清单](service-fabric-manifest-example-container-app.md#application-manifest)|
|[配置容器端口到主机的映射](service-fabric-get-started-containers.md#configure-container-port-to-host-port-mapping-and-container-to-container-discovery)| [容器应用程序清单](service-fabric-manifest-example-container-app.md#application-manifest)|
|[配置容器注册表身份验证](service-fabric-get-started-containers.md#configure-container-repository-authentication)|[容器应用程序清单](service-fabric-manifest-example-container-app.md#application-manifest)|
|[设置隔离模式](service-fabric-get-started-containers.md#configure-isolation-mode)|[容器应用程序清单](service-fabric-manifest-example-container-app.md#application-manifest)|
|[指定特定于 OS 内部版本的容器映像](service-fabric-get-started-containers.md#specify-os-build-specific-container-images)|[容器应用程序清单](service-fabric-manifest-example-container-app.md#application-manifest)|
|[设置环境变量](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[容器 FrontEndService 服务清单](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)、[容器 BackEndService 服务清单](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|
|[配置终结点](service-fabric-get-started-containers.md#configure-communication)|[容器 FrontEndService 服务清单](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)、[容器 BackEndService 服务清单](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)、[VotingData 服务清单](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|将命令传递给容器|[容器 FrontEndService 服务清单](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[将证书导入到容器中](service-fabric-securing-containers.md)|[容器 FrontEndService 服务清单](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[配置卷驱动程序](service-fabric-containers-volume-logging-drivers.md)|[容器 BackEndService 服务清单](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|

<!-- Update_Description: wording update -->
