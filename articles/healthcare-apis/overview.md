---
title: 什么是 Azure API for FHIR？ - Azure API for FHIR
description: 使用 Azure API for FHIR 可通过 FHIR API 快速交换数据。 使用托管的云服务引入、管理和保存受保护健康信息 (PHI)。
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: overview
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: efef29ce38d8da1c4e421d0b91dc34c3100502c2
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80343711"
---
# <a name="what-is-azure-api-for-fhirreg"></a>什么是 Azure API for FHIR&reg;？

使用 Azure API for FHIR，可以通过快速医疗保健互操作性资源 (FHIR®) API（受云中托管的平台即服务 (PaaS) 产品/服务支持）快速交换数据。 处理健康数据的任何人都可以通过 Azure API for FHIR 更轻松地在云中引入、管理和保存受保护健康信息 ([PHI](https://www.hhs.gov/answers/hipaa/what-is-phi/index.html))： 

- 托管的 FHIR 服务，可在数分钟内预配到云中 
- Azure 中基于 FHIR® 的企业级终结点，用于访问数据以及以 FHIR® 格式存储数据
- 高性能，低延迟
- 在合规的云环境中安全管理受保护健康数据 (PHI)
- SMART on FHIR 可用于移动和 Web 实施方案
- 使用基于角色的访问控制 (RBAC) 大规模控制自己的数据
- 针对每个数据存储中的访问、创建、修改和读取操作提供审核日志跟踪

使用 Azure API for FHIR 可以在几分钟内创建并部署一个 FHIR 服务来利用云的弹性缩放。  只需为所需的吞吐量和存储付费。 无论要管理的数据集的大小如何，为 Azure API for FHIR 提供支持的 Azure 服务都可以实现很高的性能。

使用 FHIR API 与合规的数据存储可以安全进行连接，并与任何使用 FHIR API 的系统交互。  Microsoft 负责承担 PaaS 产品/服务的操作、维护、更新与合规性要求方面的工作，因此你可以将自己的运营和开发资源解放出来。 

以下视频概要地介绍了 Azure API for FHIR：

>[!VIDEO https://www.youtube.com/embed/5vS7Iq9vpXE]

## <a name="leveraging-the-power-of-your-data-with-fhir"></a>通过 FHIR 利用数据的威力

医疗保健行业正在根据新设立的 [FHIR&reg;](https://hl7.org/fhir)（快速医疗保健互操作性资源）标准快速转换健康数据。 通过 FHIR 可以实现支持标准化语义和数据交换的可靠且可扩展数据模型，从而使得使用 FHIR 的所有系统可以协同工作。  将数据转换为 FHIR 使你可以快速连接电子健康记录系统或研究数据库等现有数据源。 使用 FHIR 还可以在现代的移动和 Web 开发实施方案中快速交换数据。 最重要的是，FHIR 可以使用分析和机器学习工具来简化数据引入及加速开发。  

### <a name="securely-manage-health-data-in-the-cloud"></a>在云中安全管理健康数据

使用 Azure API for FHIR 可以通过基于 HL7 FHIR 规范的一致性 RESTful FHIR API 交换数据。 Azure API for FHIR 受 Azure 中的托管 PaaS 产品/服务支持，还提供一个可缩放的安全环境，用于管理和存储本机 FHIR 格式的受保护健康信息 (PHI) 数据。  

### <a name="free-up-your-resources-to-innovate"></a>解放资源以进行创新

你可以投入资源来构建和运行自己的 FHIR 服务，但如果你使用 Azure API for FHIR，Microsoft 将会承担操作、维护、更新与合规性要求方面的工作，使你可以将自己的运营和开发资源解放出来。

### <a name="enable-interoperability-with-fhir"></a>启用与 FHIR 的互操作性

使用 Azure API for FHIR 可以连接到利用 FHIR API 执行读取、写入、搜索和其他功能的任何系统。  Azure API for FHIR 可以充当一个强大的工具，能够与来自电子健康记录、医生和患者仪表板、远程监视程序的临床数据配合使用，或者与系统外部具有 FHIR API 的数据库配合使用，来整合、标准化和应用机器学习。

### <a name="control-data-access-at-scale"></a>大规模控制数据访问

你可以控制自己的数据。 可以使用基于角色的访问控制 (RBAC) 来管理数据的存储和访问方式。  Azure API for FHIR 可以提供更高的安全性并减少管理工作量，而你可以根据针对环境创建的角色定义来确定谁有权访问你创建的数据集。  

### <a name="audit-logs-and-tracking"></a>审核日志和跟踪 

使用内置的审核日志快速跟踪数据的去向。 跟踪每个数据存储中的访问、创建、修改和读取操作。

### <a name="secure-your-data"></a>保护数据

使用无可比拟的安全智能保护 PHI。  数据会隔离到每个 API 实例特有的一个数据库中，并通过多区域故障转移进行保护。 Azure API for FHIR 针对数据实施分层的深度防御和高级威胁防护。  

## <a name="applications-for-a-fhir-service"></a>FHIR 服务的应用

FHIR 服务器是实现健康数据互操作性的重要工具。  Azure API for FHIR 设计为一个 API，并且用作可以快速创建、部署和开始使用的服务。  随着 FHIR 标准在医疗保健行业的普及，其用例将不断增多，下面是一些初始客户应用场景，其中 Azure API for FHIR 非常有用： 

- 创业/IOT 和应用开发：  开发以患者或提供商为中心的应用（移动或 Web 应用）的客户可以利用 Azure API for FHIR 作为完全托管的后端服务。 Azure API for FHIR 提供宝贵的资源，让客户在针对健康数据的安全云环境中管理和交换数据，利用 SMART on FHIR 实施准则，并使其技术可供所有提供商系统使用（例如，大多数 EHR 都支持 FHIR 读取 API）。   
- 医疗保健生态系统：  尽管 EHR 在许多临床设置中都以主要“事实来源”的形式存在，但提供商有时也会使用多个并非互相连接的数据库或多个以不同格式存储数据的数据库。  将 Azure API for FHIR 用作位于这些系统顶层的服务能够以 FHIR 格式将数据标准化。  这有助于跨多个系统以一致的数据格式实现数据交换。 

- 研究：  医疗保健研究人员会发现 FHIR 标准通常很有用并且 Azure API for FHIR 非常有用，因为它可以根据一个通用的 FHIR 数据模型将数据规范化，并减少机器学习和数据共享的工作量。
通过 Azure API for FHIR 交换数据能够提供审核日志和访问控制，从而帮助控制数据流以及谁有权访问哪些数据类型。 

## <a name="fhir-from-microsoft"></a>Microsoft 提供的 FHIR

可在两种配置中使用 Microsoft 提供的 FHIR 功能：

* Azure API for FHIR – Azure 中的一个 PaaS 产品/服务，可在 Azure 门户中轻松预配，由 Microsoft 管理。
* 适用于 Azure 的 FHIR 服务器 – 可部署到 Azure 订阅中的一个开源项目，已在 GitHub 上提供，网址为 https://github.com/Microsoft/fhir-server 。

对于需要扩展或自定义 FHIR 服务器或者需要访问底层服务（例如数据库）的用例，开发人员应选择开源的适用于 Azure 的 FHIR 服务器，而不要寻求使用 FHIR API。   若要实施已准备好用于生产的统包式 FHIR API 和后端服务（其中只应通过 FHIR API 访问保存的数据），开发人员应选择 Azure API for FHIR

## <a name="next-steps"></a>后续步骤

若要开始使用 Azure API for FHIR，请根据以下 5 分钟快速入门部署 Azure API for FHIR。

>[!div class="nextstepaction"]
>[部署 Azure API for FHIR](fhir-paas-portal-quickstart.md)

FHIR 是 HL7 的注册商标，在 HL7 的许可下使用。
