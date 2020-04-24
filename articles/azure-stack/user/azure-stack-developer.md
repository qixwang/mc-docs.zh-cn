---
title: 开发适用于 Azure Stack Hub 的应用
description: 使用 Azure 服务在 Azure Stack Hub 上构建应用原型的开发注意事项。
author: WenJason
ms.topic: article
origin.date: 01/24/2020
ms.date: 02/24/2020
ms.author: v-jay
ms.reviewer: unknown
ms.lastreviewed: 05/21/2019
ms.openlocfilehash: 28ee903d93240cc4b17680e6be25b806b12b90f0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77540803"
---
# <a name="develop-for-azure-stack-hub"></a>为 Azure Stack Hub 进行开发

即使没有 Azure Stack Hub 环境的访问权限，也可以立即开始开发应用。 Azure Stack Hub 提供在数据中心运行的 Azure 服务，这意味着你可以使用相同的 Azure 工具和流程在 Azure Stack Hub 上进行开发。

## <a name="development-considerations"></a>开发注意事项

通过一些准备工作并使用以下主题中的指导，可以使用 Azure 来模拟 Azure Stack Hub 环境。

* 在 Azure 中，可以创建可部署到 Azure Stack Hub 的 Azure 资源管理器模板。 请参阅[模板注意事项](azure-stack-develop-templates.md)，获取有关开发模板的指导，以确保可移植性。
* Azure 与 Azure Stack Hub 之间的服务可用性和服务版本控制存在差异。 可以使用 [Azure Stack Hub 策略模块](azure-stack-policy-module.md)来限制 Azure Stack Hub 可用的 Azure 服务可用性和资源类型。 约束服务可确保应用依赖可用于 Azure Stack Hub 的服务。
* [Azure Stack Hub 快速启动模板](https://github.com/Azure/AzureStack-QuickStart-Templates)是常见的方案示例，演示了如何开发可部署到 Azure 和 Azure Stack Hub 的模板。

## <a name="next-steps"></a>后续步骤

有关 Azure Stack 开发的详细信息，请参阅以下文章：

* [Azure 资源管理器模板最佳做法](azure-stack-develop-templates.md)
* [GitHub 上的 Azure Stack Hub 快速入门模板](https://github.com/Azure/AzureStack-QuickStart-Templates)
