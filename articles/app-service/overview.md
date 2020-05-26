---
title: 概述
description: 了解 Azure 应用服务如何帮助用户开发和托管 Web 应用程序
ms.assetid: 94af2caf-a2ec-4415-a097-f60694b860b3
ms.topic: overview
origin.date: 01/04/2017
ms.date: 05/22/2020
ms.author: v-tawe
ms.custom: mvc, seodec18
ms.openlocfilehash: a886291ed9343e66ae2bcf4711932baddcb5209d
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801323"
---
# <a name="app-service-overview"></a>应用服务概述

Azure 应用服务是一项基于 HTTP 的服务，用于托管 Web 应用程序、REST API 和移动后端  。 可以使用 .NET、NET Core、Java、Ruby、Node.js、PHP 或 Python 等偏好的语言进行开发。

应用服务不仅可将 Microsoft Azure 的强大功能（例如安全性、负载均衡、自动缩放和自动管理）添加到应用程序。 还可以利用其 DevOps 功能，例如包管理、过渡环境、自定义域和 SSL 证书。

<!-- continuous deployment from Azure DevOps, GitHub, Docker Hub, and other sources, -->

使用应用服务时，需要支付 Azure 计算资源的使用费。 使用的计算资源量由运行应用的应用服务计划确定  。 有关详细信息，请参阅 [Azure 应用服务计划概述](overview-hosting-plans.md)。

## <a name="why-use-app-service"></a>为何使用应用服务？

下面是应用服务的一些主要功能：

* **多个语言和框架** - 应用服务针对 ASP.NET、ASP.NET Core、Java、Ruby、Node.js、PHP 或 Python 提供一流支持。 我们还能以后台服务的形式运行 [PowerShell 和其他脚本或可执行文件](webjobs-create.md)。
* **托管生产环境** - 应用服务会自动[修补并维护 OS 和语言框架](overview-patch-os-runtime.md)。 将时间花在编写优秀应用上，让 Azure 来考虑平台问题。
* **具有高可用性的全局缩放** - 以手动或自动方式进行[增大](manage-scale-up.md)或[扩大](../monitoring-and-diagnostics/insights-how-to-scale.md)。 在 Azure.cn 的全国数据中心基础结构中的任意位置托管应用，并且应用服务 [SLA](https://www.azure.cn/support/sla/app-service/) 承诺高可用性。
* **与 SaaS 平台和本地数据建立连接** - 从适用于企业系统（例如 SAP）的 50 多个[连接器](../connectors/apis-list.md)、SaaS 服务（例如 Salesforce）以及 Internet 服务（例如 Facebook）中进行选择。 使用[混合连接](app-service-hybrid-connections.md)和 [Azure 虚拟网络](web-sites-integrate-with-vnet.md)访问本地数据。
* **安全性和合规性** - 应用服务符合 [ISO、SOC 和 PCI](https://www.microsoft.com/trustcenter)的要求。 使用 [Azure Active Directory](configure-authentication-provider-aad.md) 或 [Microsoft](configure-authentication-provider-microsoft.md) 对用户进行身份验证。 创建 [IP 地址限制](app-service-ip-restrictions.md)和[管理服务标识](overview-managed-identity.md)。
* **应用程序模板** - 从 [Azure 市场](https://market.azure.cn/)的大量应用程序模板列表中进行选择，例如 WordPress、Joomla 和 Drupal。
* **Visual Studio 集成** — Visual Studio 中的专用工具可简化创建、部署和调试工作。
* **API 和移动功能** - 应用服务针对 RESTful API 方案提供统包式 CORS 支持，通过启用身份验证、脱机数据同步、推送通知等功能简化移动应用方案。
* **无服务器代码** - 按需运行代码片段或脚本，无需显式预配或管理基础结构，并且只需为代码实际使用的计算时间付费（请参阅 [Azure Functions](/azure-functions/)）。

除了应用服务，Azure 还提供可用来托管网站和 Web 应用程序的其他服务。 大多数情况下，应用服务是最佳选择。  对于微服务体系结构，请考虑使用 [Service Fabric](/service-fabric)。 如果需要更好地控制运行代码的 VM，请考虑使用 [Azure 虚拟机](/virtual-machines/)。

## <a name="next-steps"></a>后续步骤

创建第一个 Web 应用。

> [!div class="nextstepaction"]
> [ASP.NET Core](app-service-web-get-started-dotnet.md)

> [!div class="nextstepaction"]
> [ASP.NET](app-service-web-get-started-dotnet-framework.md)

> [!div class="nextstepaction"]
> [PHP](app-service-web-get-started-php.md)


> [!div class="nextstepaction"]
> [Node.js](app-service-web-get-started-nodejs.md)

> [!div class="nextstepaction"]
> [Java](app-service-web-get-started-java.md)



> [!div class="nextstepaction"]
> [HTML](app-service-web-get-started-html.md)

