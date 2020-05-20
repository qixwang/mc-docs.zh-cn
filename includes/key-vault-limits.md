---
author: rothja
ms.service: cost-management-billing
ms.topic: include
origin.date: 11/09/2018
ms.date: 12/09/2019
ms.author: v-tawe
ms.openlocfilehash: 3827d77260a639fe3955bae3e1a0b314645c0926
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74840131"
---
#### <a name="key-transactions-maximum-transactions-allowed-in-10-seconds-per-vault-per-regionsup1sup"></a>密钥事务数（每个区域的每个保管库在 10 秒内允许的事务数上限<sup>1</sup>）：

|密钥类型|软件密钥<br>CREATE 密钥|Software-key<br>所有其他事务|
|:---|---:|---:|---:|---:|
|RSA 2,048 位|10|2,000|
|RSA 3,072 位|10|500|
|RSA 4,096 位|10|250|
|ECC P-256|10|2,000|
|ECC P-384|10|2,000|
|ECC P-521|10|2,000|
|ECC SECP256K1|10|2,000|

<!-- HSM not support, use Software for example -->

> [!NOTE]
> 在上表中，我们看到，对于 RSA 2,048 位软件密钥，每 10 秒允许 2,000 个 GET 事务。
>
> 限制阈值是加权的，并且是针对其总和施加的。 例如，如上表所示，对 RSA 软件密钥执行 GET 操作时，使用 4,096 位密钥的开销是使用 2,048 位密钥的开销的 8 倍。 这是因为 2,000/250 = 8。
>
> 在给定的 10 秒间隔内，Azure Key Vault 客户端在遇到 `429` 限制 HTTP 状态代码之前，只能执行以下操作*之一*：
> - 2,000 个 RSA 2,048 位软件密钥 GET 事务
> - 250 个 RSA 4,096 位软件密钥 GET 事务
> - 249 个 RSA 4,096 位软件密钥 GET 事务和 8 个 RSA 2,048 位软件密钥 GET 事务

#### <a name="secrets-managed-storage-account-keys-and-vault-transactions"></a>机密、托管存储帐户密钥，以及保管库事务：
| 事务类型 | 每个区域的每个保管库在 10 秒内允许的事务数上限<sup>1</sup> |
| --- | --- |
| 所有事务 |2,000 |

有关超出这些限制时如何处理限制的信息，请参阅 [Azure Key Vault 限制指南](../articles/key-vault/key-vault-ovw-throttling.md)。

<!-- HSM not support, use Software for example -->

<sup>1</sup> 所有事务类型的订阅范围限制是每个密钥保管库限制的 5 倍。 例如，每个订阅的“软件 - 其他”事务限制为每个订阅 10 秒内 10,000 个事务。
