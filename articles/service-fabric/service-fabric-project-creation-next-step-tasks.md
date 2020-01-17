---
title: 创建 Service Fabric 项目的后续步骤
description: 了解刚刚在 Visual Studio 中创建的应用程序项目。  了解如何使用教程生成服务，并详细了解如何开发适用于 Service Fabric 的服务。
ms.topic: conceptual
origin.date: 12/07/2017
ms.date: 01/13/2020
ms.author: v-yeche
ms.openlocfilehash: dfcc5e59c9b04d2ed85dda110c09ec79997e3d20
ms.sourcegitcommit: 713136bd0b1df6d9da98eb1da7b9c3cee7fd0cee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75742482"
---
# <a name="your-service-fabric-application-and-next-steps"></a>Service Fabric 应用程序和后续步骤
已创建 Azure Service Fabric 应用程序。 本指南介绍一些可以尝试阅读的教程、项目构成、你可能感兴趣的其他信息，以及可能的后续步骤。

## <a name="get-started-with-tutorials-walk-throughs-and-samples"></a>从教程、演练和示例入门
已准备就绪？  

演练 .NET 应用程序教程。 了解如何使用 ASP.NET Core 前端和有状态后端[生成应用程序](service-fabric-tutorial-create-dotnet-app.md)、[将应用程序部署](service-fabric-tutorial-deploy-app-to-party-cluster.md)到群集以及[配置 CI/CD](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)。

<!-- Not Available on [set up monitoring and diagnostics](service-fabric-tutorial-monitoring-aspnet.md) -->

或者，尝试阅读以下演练之一来了解：
- [在 Windows 上创建第一个 C# Reliable Services 服务](service-fabric-reliable-services-quick-start.md) 
- [在 Windows 上创建第一个 C# Reliable Actors 服务](service-fabric-reliable-actors-get-started.md) 
- [在 Windows 上创建第一个来宾可执行文件服务](quickstart-guest-app.md) 
- [Windows 容器应用程序](service-fabric-get-started-containers.md) 

我们还建议尝试演练[示例应用程序](https://aka.ms/servicefabricsamples)。

## <a name="have-questions-or-feedback--need-to-report-an-issue"></a>有问题或反馈？  需要报告问题？
请阅读[常见问题](service-fabric-common-questions.md)，找到有关 Service Fabric 的功能及其用法的答案。

[支持选项](service-fabric-support.md)列出了 StackOverflow 和 MSDN 上可以解答问题的论坛，以及用于报告问题、获得支持和提交产品反馈的选项。

## <a name="the-application-project"></a>应用程序项目
每个新应用程序都包含一个应用程序项目。 根据选择的服务类型，可能有一个或两个附加的项目。

应用程序项目包括：

* 对构成应用程序的服务的引用集。
* 三个发布配置文件（1-Node Local、5-Node Local 和 Cloud），可用于维护首选项以适应不同的环境 - 例如，与群集终结点相关的首选项，以及是否默认执行升级部署。
* 三个应用程序参数文件（同上），可用于维护环境特定的应用程序配置，例如要为服务创建的分区数量。 了解如何[为多个环境配置应用程序](service-fabric-manage-multiple-environment-app-configuration.md)。
* 可使用部署脚本从命令行部署应用程序，或者通过自动持续集成和部署管道来部署应用程序。 详细了解如何[使用 PowerShell 部署应用程序](service-fabric-deploy-remove-applications.md)。
* 用于描述应用程序的应用程序清单。 可以在 ApplicationPackageRoot 文件夹下查找清单。 详细了解[应用程序和服务清单](service-fabric-application-model.md)。

## <a name="learn-more-about-the-programming-models"></a>详细了解编程模型
Service Fabric 提供了多种方法来编写和管理服务。  下面是有关[无状态和有状态 Reliable Services](service-fabric-reliable-services-introduction.md)、[Reliable Actors](service-fabric-reliable-actors-introduction.md)、[容器](service-fabric-containers-overview.md)、[来宾可执行文件](service-fabric-guest-executables-introduction.md)及[无状态和有状态 ASP.NET Core 服务](service-fabric-reliable-services-communication-aspnetcore.md)的概述和概念信息。

## <a name="learn-about-service-communication"></a>了解服务通信
Service Fabric 应用程序由不同的服务组成，其中每个服务执行专门的任务。 这些服务可以相互通信，群集外部可能有些客户端应用程序会与这些服务进行连接和通信。 了解如何在 Service Fabric 中[设置与服务进行的通信以及服务之间的通信](service-fabric-connect-and-communicate-with-services.md)。 

## <a name="learn-about-configuring-application-security"></a>了解如何配置应用程序安全性
可以保护群集中以不同用户帐户运行的应用程序。 使用用户帐户进行部署时，Service Fabric 还有助于保护应用程序所使用的资源，例如文件、目录和证书。 这样，即使是在共享托管环境中，运行应用程序会更加安全。  了解如何[为应用程序配置安全策略](service-fabric-application-runas-security.md)。

应用程序可能包含敏感信息，例如存储连接字符串、密码或其他不应以明文形式处理的值。 了解如何[管理应用程序中的机密](service-fabric-application-secret-management.md)。

## <a name="learn-about-the-application-lifecycle"></a>了解应用程序生命周期
与其他平台一样，Service Fabric 应用程序通常经历以下几个阶段：设计、开发、测试、部署、升级、维护和删除。 [此文](service-fabric-application-lifecycle.md)提供了有关 API 的概述，以及不同角色在 Service Fabric 应用程序生命周期的各个阶段如何使用它们。

## <a name="next-steps"></a>后续步骤
- [在 Azure 中创建 Windows 群集](service-fabric-tutorial-create-vnet-and-windows-cluster.md)。
- 使用 [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) 可视化群集，包括已部署的应用程序和物理布局。
- [对服务进行版本控制和升级](service-fabric-application-upgrade-tutorial.md)

<!--Update_Description: update meta properties  -->