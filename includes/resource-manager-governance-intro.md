---
title: include 文件
description: include 文件
services: azure-resource-manager
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: include
origin.date: 02/19/2018
ms.date: 04/15/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: fe3dbc849f32f1d4003333257bdf06aa2d5a232c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63825124"
---
将资源部署到 Azure 时，可以灵活选择想要部署的资源类型、资源的位置以及对它们的设置方式。 但是，除了你想要在组织中允许的选项，这种灵活性可能还会开放更多其他选项。 在考虑将资源部署到 Azure 时，你可能想知道以下问题：

* 如何满足特定国家/地区针对数据所有权制定的法规要求？
* 如何控制成本？
* 如何确保用户不会无意中更改关键系统？
* 如何跟踪资源成本并准确地进行计费？

本文会为你解答这些问题。 具体而言，你需要：

> [!div class="checklist"]
> * 将用户分配到角色并分配角色对应的作用域，这样用户就能具备执行预期操作所需的权限，同时并不会涉及其他操作。
> * 应用策略来对订阅中的资源进行约定。
> * 锁定系统中的关键资源。
> * 标记资源，以便按它们对组织的价值进行跟踪。

本文重点介绍实现管理需要完成的任务。
<!-- Not Avaiable on  [Governance in Azure](../articles/security/governance-in-azure.md) -->
<!--ms.date: 03/26/2018-->