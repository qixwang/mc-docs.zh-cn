---
title: 在 Azure 容器实例中运行计算机视觉容器
titleSuffix: Azure Cognitive Services
description: 将计算机视觉容器部署到 Azure 容器实例，并在 Web 浏览器中对其进行测试。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 06/08/2020
ms.author: v-tawe
origin.date: 04/01/2020
ms.openlocfilehash: bd1a5e1c803d5235acaa9b7db39de0e212d0ba81
ms.sourcegitcommit: 8dae792aefbe44e8388f961b813e3da6564423ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84655024"
---
# <a name="deploy-the-computer-vision-container-to-azure-container-instances"></a>将计算机视觉容器部署到 Azure 容器实例

了解如何将认知服务[计算机视觉](computer-vision-how-to-install-containers.md)容器部署到 Azure [容器实例](https://docs.microsoft.com/azure/container-instances/)。 此过程演示如何创建计算机视觉资源。 然后讨论如何拉取关联的容器映像。 最后，我们重点介绍了从浏览器中执行这两个业务流程的能力。 使用容器可以将开发人员的注意力从管理基础结构转移到应用程序开发上。

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>请求访问专用容器注册表

[!INCLUDE [Request access](../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Create a Cognitive Services Computer Vision resource](includes/create-computer-vision-resource.md)]

[!INCLUDE [Create the Computer Vision on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

<!-- containers-next-steps available -->