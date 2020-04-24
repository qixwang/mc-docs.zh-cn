---
title: 使用策略为托管应用程序部署关联
description: 了解如何使用 Azure Policy 服务为托管应用程序部署关联。
author: rockboyfor
ms.topic: conceptual
origin.date: 09/06/2019
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: 99ddc49ac325e8c99d684a35624ce40e0e3d68a0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "76170625"
---
# <a name="deploy-associations-for-a-managed-application-using-azure-policy"></a>使用 Azure Policy 为托管应用程序部署关联

Azure 策略可用于部署关联，将资源关联到托管应用程序。 本文介绍了一种用于部署关联的内置策略，并介绍了如何使用该策略。

## <a name="built-in-policy-to-deploy-associations"></a>用于部署关联的内置策略

“为托管应用程序部署关联”是一种内置策略，此策略可用于部署关联，将资源关联到托管应用程序。 此策略接受三个参数：

- 托管应用程序 ID - 此 ID 是需将资源关联到其中的托管应用程序的资源 ID。
- 要关联的资源类型 - 这些资源类型是要关联到托管应用程序的资源类型的列表。 可以使用同一策略将多个资源类型关联到一个托管应用程序。
- 关联名称前缀 - 此字符串是要添加到正在创建的关联资源的名称中的前缀。 默认值为“DeployedByPolicy”。

DeployIfNotExists 评估 它在资源提供程序处理了所选资源类型的创建或更新资源请求之后运行，该计算返回了成功状态代码。 目前可以使用任何模板部署方法部署此模板。
有关关联的详细信息，请参阅 [Azure 自定义提供程序资源载入](../custom-providers/concepts-resource-onboarding.md)

## <a name="how-to-use-the-deploy-associations-built-in-policy"></a>如何使用部署关联内置策略 

### <a name="prerequisites"></a>必备条件
如果托管应用程序需要订阅的权限来执行操作，则在不授权的情况下，关联资源的策略部署将不起作用。

### <a name="policy-assignment"></a>策略分配
若要使用内置策略，请创建一个策略分配，并分配“为托管应用程序部署关联”策略。 成功分配策略后，策略会标识不合规的资源并为这些资源部署关联。

![分配内置策略](media/concepts-built-in-policy/assign-builtin-policy-managedapp.png)

<!--Not Available on ## Getting help-->
<!--Not Available on [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-custom-providers)-->
<!--Not Available on ## Next steps-->
<!--Not Available on - [Concepts: Azure Custom Providers resource onboarding](../custom-providers/concepts-resource-onboarding.md)-->
<!--Not Available on - [Tutorial: Resource onboarding with custom providers](../custom-providers/tutorial-resource-onboarding.md)-->
<!--Not Available on - [Tutorial: Create custom actions and resources in Azure](../custom-providers/tutorial-get-started-with-custom-providers.md)-->
<!--Not Available on - [Quickstart: Create a custom resource provider and deploy custom resources](../custom-providers/create-custom-provider.md)-->
<!--Not Available on - [How to: Adding custom actions to an Azure REST API](../custom-providers/custom-providers-action-endpoint-how-to.md)-->
<!--Not Available on - [How to: Adding custom resources to an Azure REST API](../custom-providers/custom-providers-resources-endpoint-how-to.md)-->

<!-- Update_Description: new article about concepts built in policy -->
<!--NEW.date: 01/20/2020-->