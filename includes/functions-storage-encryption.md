---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/01/2020
ms.author: v-junlch
ms.openlocfilehash: d96e51ecba15d1739b8a915ed433b214ab150d02
ms.sourcegitcommit: 1008ad28745709e8d666f07a90e02a79dbbe2be5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85945240"
---
Azure 存储可对存储帐户中的所有数据进行静态加密。 有关详细信息，请参阅[静态数据的 Azure 存储加密](../articles/storage/common/storage-service-encryption.md)。

默认情况下，数据使用 Microsoft 管理的密钥进行加密。 为了进一步控制加密密钥，可以提供客户管理的密钥，用于对 blob 和文件数据进行加密。 这些密钥必须存在于 Azure Key Vault 中，以便 Functions 能够访问存储帐户。 若要了解详细信息，请参阅[使用客户管理的密钥进行静态加密](../articles/azure-functions/configure-encrypt-at-rest-using-cmk.md)。  

