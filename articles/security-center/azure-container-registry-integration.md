---
title: Azure 安全中心和 Azure 容器注册表
description: 了解 Azure 安全中心与 Azure 容器注册表的集成
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2020
ms.author: v-tawe
origin.date: 11/19/2019
ms.openlocfilehash: 276efed3cfbe4978af9d83f440890d735c9dd4ea
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83423161"
---
# <a name="azure-container-registry-integration-with-security-center"></a>Azure 容器注册表与安全中心的集成

Azure 容器注册表 (ACR) 是一种托管的专用 Docker 注册表服务，它在中心注册表中存储和管理用于 Azure 部署的容器映像。 它基于开源 Docker 注册表 2.0。

如果你使用的是 Azure 安全中心标准层，则可以添加容器注册表捆绑包。 此可选功能使你可以更深入地了解基于 ARM 的注册表中的映像的漏洞。 在订阅级别启用或禁用涵盖订阅中所有注册表的捆绑包。 此功能按映像收费，如[定价页](security-center-pricing.md)所示。 启用容器注册表捆绑包可确保安全中心准备好扫描推送到注册表的映像。 

每当将映像推送到注册表时，安全中心都会自动扫描该映像。 若要触发映像扫描，请将该映像推送到存储库。

扫描完成后（通常在大约 10 分钟后），可在安全中心建议中找到如下结果：

[![有关在 Azure 容器注册表 (ACR) 托管映像中发现的漏洞的 Azure 安全中心建议示例](media/azure-container-registry-integration/container-security-acr-page.png)](media/azure-container-registry-integration/container-security-acr-page.png#lightbox)

## <a name="benefits-of-integration"></a>集成的好处

安全中心在订阅中识别基于 ARM 的 ACR 注册表并无缝提供以下内容：

* **Azure 原生漏洞扫描**，适用于所有推送的 Linux 映像。 安全中心使用行业领先的漏洞扫描供应商 Qualys 提供的扫描程序来扫描映像。 默认情况下，此原生解决方案已无缝集成。

* **安全建议**，适用于具有已知漏洞的 Linux 映像。 安全中心提供每个报告的漏洞的详细信息和严重性分类。 此外，它还指导如何修正在推送到注册表的每个映像上发现的特定漏洞。

![Azure 安全中心和 Azure 容器注册表 (ACR) 综合概述](./media/azure-container-registry-integration/aks-acr-integration-detailed.png)

## <a name="next-steps"></a>后续步骤

若要详细了解安全中心的容器安全功能，请参阅：

* [Azure 安全中心和容器安全性](container-security.md)

* [与 Azure Kubernetes 服务集成](azure-kubernetes-service-integration.md)

* [虚拟机保护](security-center-virtual-machine-protection.md) - 介绍安全中心的建议
