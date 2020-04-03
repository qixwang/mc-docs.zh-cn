---
title: 语音识别服务静态数据的加密
titleSuffix: Azure Cognitive Services
description: 语音识别服务静态数据的加密。
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 03/13/2020
ms.date: 03/16/2020
ms.author: v-tawe
ms.openlocfilehash: 4282653c00b894bd0e300340f5e15a111955c70f
ms.sourcegitcommit: b2f2bb08ab1b5ccb3c596d84b3b6ddca5bba3903
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80151761"
---
# <a name="speech-service-encryption-of-data-at-rest"></a>语音识别服务静态数据的加密

语音识别服务在将数据保存到云时会自动加密数据。 语音识别服务加密可以保护数据，并帮助组织履行在安全性与合规性方面做出的承诺。

## <a name="about-cognitive-services-encryption"></a>关于认知服务加密

数据是使用符合 [FIPS 140-2](https://wikipedia.org/wiki/FIPS_140-2) 的 [256 位 AES](https://wikipedia.org/wiki/Advanced_Encryption_Standard) 加密法加密和解密的。 加密和解密都是透明的，这意味着将替你管理加密和访问。 你的数据默认情况下就是安全的，你无需修改代码或应用程序，即可利用加密。

## <a name="about-encryption-key-management"></a>关于加密密钥管理

使用自定义语音识别和自定义语音时，语音识别服务可能会将以下数据存储在云中：  

* 语音识别跟踪数据 - 仅当你为自定义终结点启用了跟踪时才存储
* 已上传的训练和测试数据

默认情况下，你的数据存储在 Microsoft 的存储中，你的订阅使用 Microsoft 托管的加密密钥。 你还可以选择准备你自己的存储帐户。 对应用商店的访问是由托管标识管理的，语音识别服务无法直接访问你自己的数据，例如语音识别跟踪数据、自定义训练数据和自定义模型。

有关托管标识的详细信息，请参阅[什么是托管标识](https://docs.azure.cn/active-directory/managed-identities-azure-resources/overview)。

<!-- 
## Bring your own storage (BYOS) for customization and logging

To request access to bring your own storage, fill out and submit the [Speech service - bring your own storage (BYOS) request form](https://aka.ms/cogsvc-cmk). Once approved, you'll need to create your own storage account to store the data required for customization and logging. When adding a storage account, the Speech service resource will enable a system assigned managed identity. After the system assigned managed identity is enabled, this resource will be registered with Azure Active Directory (AAD). After being registered, the managed identity will be given access to the storage account. You can learn more about Managed Identities here. For more information about Managed Identity, see [What are managed identities](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

> [!IMPORTANT]
> If you disable system assigned managed identities, access to the storage account will be removed. This will cause the parts of the Speech service that require access to the storage account to stop working.  

## Regional availability

BYOS is currently available in these regions:

* US South Central
* West US 2
* East US

-->

## <a name="next-steps"></a>后续步骤

<!-- * [Speech service - bring your own storage (BYOS) request form](https://aka.ms/cogsvc-cmk) -->

* [什么是托管标识](https://docs.azure.cn/active-directory/managed-identities-azure-resources/overview)。
