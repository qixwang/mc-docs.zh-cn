---
title: 关于 Azure Key Vault 机密 - Azure Key Vault
description: Azure Key Vault REST 接口概述以及机密的开发人员详细信息。
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: overview
origin.date: 09/04/2019
ms.date: 04/20/2020
ms.author: v-tawe
ms.openlocfilehash: 86396c08ac89dcc733ea184fbb78bcb0c3c8f983
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82588845"
---
# <a name="about-azure-key-vault-secrets"></a>关于 Azure Key Vault 机密

凭借 Azure Key Vault，Microsoft Azure 应用程序和用户能够存储和使用多种类型的机密数据：

- 机密：提供机密（例如密码和数据库连接字符串）的安全存储。

- Azure 存储：可以管理 Azure 存储帐户的密钥。 在内部，Key Vault 可以使用 Azure 存储帐户列出（同步）密钥，并定期重新生成（轮换）密钥。 

有关 Key Vault 的更多常规信息，请参阅[什么是 Azure Key Vault？](/key-vault/key-vault-overview)

## <a name="azure-key-vault"></a>Azure Key Vault

以下部分提供在实现 Key Vault 服务中可以用到的常规信息。 

### <a name="objects-identifiers-and-versioning"></a>对象、标识符和版本控制

对于存储在 Key Vault 中的对象，在创建了某一对象的新实例后，这些对象就会受到版本控制。 每个版本都分配有唯一标识符和 URL。 首次创建一个对象时，该对象被赋予了一个唯一的版本标识符，并标记为当前版本的对象。 创建与对象同名的新实例会向新对象赋予一个唯一的版本标识符，并使其成为当前版本。  

Key Vault 中的对象可以使用当前标识符或特定于版本的标识符进行寻址。 例如，给定一个名称为 `MasterKey` 的密钥，使用当前标识符执行操作会导致系统使用最新的可用版本。 使用特定于版本的标识符执行操作会导致系统使用该特定版本的对象。  

Key Vault 中的对象通过 URL 唯一标识。 不管地理位置如何，系统中都不存在两个具有相同 URL 的对象。 对象的完整 URL 称为对象标识符。 URL 由标识 Key Vault 的前缀、对象类型、用户提供的对象名称和对象版本组成。 对象名称不区分大小写且不可变。 不包括对象版本的标识符称为基本标识符。  

有关详细信息，请参阅[身份验证、请求和响应](../general/authentication-requests-and-responses.md)

对象标识符具有以下常规格式：  

`https://{keyvault-name}.vault.azure.cn/{object-type}/{object-name}/{object-version}`  

其中：  

|||  
|-|-|  
|`keyvault-name`|Azure Key Vault 服务中的密钥保管库名称。<br /><br /> Key Vault 名称由用户选择，并且全局唯一。<br /><br /> Key Vault 的名称必须是 3-24 个字符，且仅包含 0-9、a-z、A-Z 和 -。|  
|`object-type`|对象的类型，要么为“密钥”，要么为“机密”。|  
|`object-name`|`object-name` 是用户提供名称，在 Key Vault 中必须保持唯一。 该名称必须是 1-127 个字符，且仅包含 0-9、a-z、A-Z 和 - 的字符串。|  
|`object-version`|`object-version` 是系统生成的 32 个字符的字符串标识符，可以选择用来对某个对象的唯一版本进行寻址。|  

## <a name="key-vault-secrets"></a>Key Vault 机密 

### <a name="working-with-secrets"></a>使用机密

从开发人员的角度来看，Key Vault API 接受机密值并将其作为字符串返回。 在内部，Key Vault 存储机密并将其作为八位字节序列（8 位字节）管理，每个字节的最大大小为 25k 字节。 Key Vault 服务不提供机密的语义。 它只是接受数据，然后加密和存储该数据，最后返回机密标识符（“id”）。 该标识符可用于稍后检索机密。  

对于高度敏感的数据，客户端应考虑对数据进行额外的保护。 例如，在 Key Vault 中存储数据之前，使用单独的保护密钥加密数据。  

Key Vault 还支持机密的 contentType 字段。 客户端可以指定机密的内容类型，以帮助在检索时解释机密数据。 此字段的最大长度为 255 个字符。 没有预定义的值。 建议用于解释机密数据的提示。 例如，实现可以将密码和证书都存储为机密，然后使用此字段进行区分。 没有预定义的值。  

### <a name="secret-attributes"></a>机密属性

除机密数据外，还可以指定以下属性：  

- *exp*：IntDate，可选，默认值为 **forever**。 exp（过期时间）属性标识在不应检索机密数据当时或之后的过期时间，[特定情况](#date-time-controlled-operations)除外  。 此字段仅供参考，因为它通知密钥保管库服务用户可能无法使用特定机密  。 其值必须是包含 IntDate 值的数字。   
- *nbf*：IntDate，可选，默认值为 **now**。 nbf（非过去）属性标识在不应检索机密数据之前的时间，[特定情况](#date-time-controlled-operations)除外  。 此字段仅供参考  。 其值必须是包含 IntDate 值的数字。 
- enabled：布尔型，可选，默认值为 true   。 此属性指定是否可以检索机密数据。 enabled 属性与 nbf 和 exp 结合使用，如果在 nbf 和 exp 之间出现操作，只有在 enabled 设置为 true 时，才允许该操作      。 nbf 和 exp 时段外的操作会自动禁止，[特定情况](#date-time-controlled-operations)除外   。  

在包含机密属性的任何响应中还包括以下其他只读属性：  

- *created*：IntDate，可选。 created 属性指示创建此版本的机密的时间。 如果机密在添加此属性之前创建，此值为 NULL。 其值必须是包含 IntDate 值的数字。  
- *updated*：IntDate，可选。 updated 属性指示更新此版本的机密的时间。 如果机密上次更新的时间早于添加此属性的时间，此值为 NULL。 其值必须是包含 IntDate 值的数字。

#### <a name="date-time-controlled-operations"></a>日期时间控制的操作

机密的获取操作在 nbf / exp 时段外适合尚未生效的机密和过期的机密    。 对于尚未生效的机密，调用机密的“获取”操作可用于测试目的  。 检索（获取）过期的密钥可以用于恢复操作  。

### <a name="secret-access-control"></a>机密访问控制

Key Vault 中托管的机密的访问控制是在包含这些机密的 Key Vault 级别提供的。 在同一 Key Vault 中，机密的访问控制策略不同于密钥的访问控制策略。 用户可以创建一个或多个保管库来保存机密，并且需要维护方案相应的机密分段和管理。   

在保管库上的机密访问控制条目中可以按主体使用以下权限，这些权限对机密对象上允许的操作采取严密的镜像操作：  

- 针对机密管理操作的权限
  - *get*：读取机密  
  - *list*：列出 Key Vault 中存储的机密或机密版本  
  - *set*：创建机密  
  - *delete*：删除机密  
  - *recover*：恢复已删除的机密
  - *backup*：备份密钥保管库中的机密
  - *restore*：将备份机密还原到密钥保管库

- 针对特权操作的权限
  - *purge*：清除（永久删除）已删除的机密

有关使用机密的详细信息，请参阅 [Key Vault REST API 中的机密操作参考](https://docs.microsoft.com/rest/api/keyvault)。 有关建立权限的信息，请参阅[保管库 - 创建或更新](https://docs.microsoft.com/rest/api/keyvault/vaults/createorupdate)和[保管库 - 更新访问策略](https://docs.microsoft.com/rest/api/keyvault/vaults/updateaccesspolicy)。 

### <a name="secret-tags"></a>机密标记  
可以用标记的形式指定其他特定于应用程序的元数据。 Key Vault 支持多达 15 种标记，每种标记可以有 256 个字符的名称和 256 个字符的值。  

>[!Note]
>如果调用方具有“列表”  或“获取”  权限，则调用方可以读取标记。

## <a name="azure-storage-account-key-management"></a>Azure 存储帐户密钥管理

Key Vault 可以管理 Azure 存储帐户密钥：

- 在内部，Key Vault 可以使用 Azure 存储帐户列出（同步）密钥。 
- Key Vault 定期重新生成（轮换）密钥。
- 响应调用方时永远不会返回密钥值。
- Key Vault 管理存储帐户和经典存储帐户的密钥。

有关详细信息，请参阅 [Azure Key Vault 存储帐户密钥](../secrets/overview-storage-keys.md)

### <a name="storage-account-access-control"></a>存储帐户访问控制

授权用户或应用程序主体对托管的存储帐户执行操作时，可以使用以下权限：  

- 针对托管存储帐户和 SaS 定义操作的权限
  - *get*：获取有关存储帐户的信息 
  - *list*：列出 Key Vault 托管的存储帐户
  - *update*：更新存储帐户
  - *delete*：删除存储帐户  
  - *recover*：恢复删除的存储帐户
  - *backup*：备份存储帐户
  - *restore*：将备份存储帐户还原到 Key Vault
  - *set*：创建或更新存储帐户
  - *regeneratekey*：为存储帐户重写指定的密钥值
  - *getsas*：获取有关存储帐户的 SAS 定义的信息
  - *listsas*：列出存储帐户的存储 SAS 定义
  - *deletesas*：从存储帐户中删除 SAS 定义
  - *setsas*：创建或更新存储帐户的新 SAS 定义/属性

- 针对特权操作的权限
  - *purge*：清除（永久删除）托管存储帐户

有关详细信息，请参阅 [Key Vault REST API 中的存储帐户操作参考](https://docs.microsoft.com/rest/api/keyvault)。 有关建立权限的信息，请参阅[保管库 - 创建或更新](https://docs.microsoft.com/rest/api/keyvault/vaults/createorupdate)和[保管库 - 更新访问策略](https://docs.microsoft.com/rest/api/keyvault/vaults/updateaccesspolicy)。

## <a name="see-also"></a>另请参阅

- [身份验证、请求和响应](../general/authentication-requests-and-responses.md)
- [Key Vault 开发人员指南](../general/developers-guide.md)
