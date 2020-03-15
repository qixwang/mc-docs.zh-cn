---
title: Azure Functions 的存储注意事项
description: 了解 Azure Functions 的存储要求以及如何加密存储的数据。
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: c01112fe5280b2b5f07ed7c77a391c8ddf5c5454
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79291722"
---
# <a name="storage-considerations-for-azure-functions"></a>Azure Functions 的存储注意事项

创建函数应用实例时，Azure Functions 需要一个 Azure 存储帐户。 函数应用可使用以下存储服务：


|存储服务  | Functions 的用途  |
|---------|---------|
| [Azure Blob 存储](../storage/blobs/storage-blobs-introduction.md)     | 维护绑定状态和函数密钥。  <br/>另由 [Durable Functions 中的任务中心](durable/durable-functions-task-hubs.md)使用。 |
| [Azure 文件](../storage/files/storage-files-introduction.md)  | 用于存储和运行[消耗计划](functions-scale.md#consumption-plan)中的函数应用代码的文件共享。 |
| [Azure 队列存储](../storage/queues/storage-queues-introduction.md)     | 由 [Durable Functions 中的任务中心](durable/durable-functions-task-hubs.md)使用。   |
| [Azure 表存储](../storage/tables/table-storage-overview.md)  |  由 [Durable Functions 中的任务中心](durable/durable-functions-task-hubs.md)使用。       |

> [!IMPORTANT]
> 使用消耗托管计划时，函数代码和绑定配置文件存储在主存储帐户的 Azure 文件存储中。 删除主存储帐户时，此内容将随之删除且无法恢复。

## <a name="storage-account-requirements"></a>存储帐户要求

创建函数应用时，必须创建或链接到支持 Blob、队列和表存储的常规用途的 Azure 存储帐户。 这是因为，Functions 依赖于 Azure 存储执行管理触发器和记录函数执行等操作。 某些存储帐户不支持队列和表。 这些帐户包括仅限 Blob 的存储帐户、Azure 高级存储，以及启用了 ZRS 复制的常规用途存储帐户。 创建函数应用时，将从“存储帐户”边栏选项卡中筛选出这些不受支持的帐户。

若要了解有关存储帐户类型的详细信息，请参阅 [Azure 存储服务简介](../storage/common/storage-introduction.md#azure-storage-services)。 

尽管可将现有的存储帐户用于函数应用，但必须确保该帐户满足这些要求。 可以保证在函数应用创建流中创建的存储帐户满足这些存储帐户要求。  

## <a name="storage-account-guidance"></a>有关存储帐户的指导

每个函数应用都需要存储帐户才能运行。 如果该帐户已被删除，则函数应用不会运行。 若要解决存储相关的问题，请参阅[如何排查存储相关的问题](functions-recover-storage-account.md)。 以下附加注意事项适用于函数应用使用的存储帐户。

### <a name="storage-account-connection-setting"></a>存储帐户连接设置

存储帐户连接在 [AzureWebJobsStorage 应用程序设置](./functions-app-settings.md#azurewebjobsstorage)中维护。 

重新生成存储密钥时，必须更新存储帐户连接字符串。 [在此处阅读有关存储密钥管理的详细信息](/storage/common/storage-create-storage-account)。

### <a name="shared-storage-accounts"></a>共享存储帐户

多个函数应用可以共享同一存储帐户，不会导致任何问题。 例如，在 Visual Studio 中，可以使用 Azure 存储仿真器开发多个应用。 在这种情况下，仿真器的作用类似于单个存储帐户。 还可以使用函数应用所用的同一存储帐户来存储应用程序数据。 但在生产环境中，这种做法不一定恰当。

### <a name="optimize-storage-performance"></a>优化存储性能

[!INCLUDE [functions-shared-storage](../../includes/functions-shared-storage.md)]

## <a name="storage-data-encryption"></a>存储数据加密

Azure 存储对静态存储帐户中的所有数据进行加密。 有关详细信息，请参阅[静态数据的 Azure 存储加密](../storage/common/storage-service-encryption.md)。

默认情况下，使用 Microsoft 管理的密钥对数据进行加密。 为了更进一步控制加密密钥，可以提供客户管理的密钥，以用于对 Blob 和文件数据进行加密。 要使 Functions 能够访问存储帐户，这些密钥必须在 Azure Key Vault 中存在。 有关详细信息，请参阅[通过 Azure 门户使用 Azure Key Vault 配置客户管理的密钥](../storage/common/storage-encryption-keys-portal.md)。  

## <a name="next-steps"></a>后续步骤

详细了解 Azure Functions 托管选项。

> [!div class="nextstepaction"]
> [Azure Functions 的缩放和托管](functions-scale.md)

<!-- Update_Description: wording update -->

