---
title: 在 Azure 容器实例中运行人脸容器
titleSuffix: Azure Cognitive Services
description: 将人脸容器部署到 Azure 容器实例，并在 Web 浏览器中对其进行测试。
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 08/04/2020
ms.author: v-johya
ms.openlocfilehash: 6d3b33b7fdd0827b161d3bf962dbdde8a4886f9a
ms.sourcegitcommit: caa18677adb51b5321ad32ae62afcf92ac00b40b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2020
ms.locfileid: "88023696"
---
# <a name="deploy-the-face-container-to-azure-container-instances"></a>将人脸容器部署到 Azure 容器实例

> [!IMPORTANT]
> 已达到人脸容器用户的限制。 目前不接受人脸容器的新应用程序。

了解如何将认知服务[人脸](../face-how-to-install-containers.md)容器部署到 Azure [容器实例](/container-instances/)。 此过程演示如何创建 Azure 人脸资源。 然后讨论如何拉取关联的容器映像。 最后，我们重点介绍了从浏览器中执行这两个业务流程的能力。 使用容器可以将开发人员的注意力从管理基础结构转移到应用程序开发上。

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

[!INCLUDE [Create a Cognitive Services Face resource](../includes/create-face-resource.md)]

[!INCLUDE [Create an Face container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../../containers/includes/containers-next-steps.md)]

