---
title: 使用 .NET 定义储访问策略 - Azure 存储
description: 了解如何使用 .NET 客户端库定义存储访问策略。
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 08/06/2019
ms.date: 09/09/2019
ms.author: v-jay
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 8a551da0d02d37af931444cc54ef4ca9820a63b6
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "70209432"
---
# <a name="define-a-stored-access-policy-with-net"></a>使用 .NET 定义存储访问策略

存储访问策略对服务器端的服务级别共享访问签名 (SAS) 提供另一级别的控制。 定义存储访问策略可以将共享访问签名分组到一起，并为通过策略绑定的共享访问签名提供其他限制。 可以使用存储访问策略更改 SAS 的开始时间、到期时间或权限，或者在颁发 SAS 后将其吊销。
  
 以下存储资源支持存储访问策略：  
  
- Blob 容器  
- 文件共享  
- 队列  
- 表  
  
> [!NOTE]
> 容器上的存储访问策略可以与共享访问签名相关联，后者授予对容器本身的权限，或对它包含的 Blob 的权限。 类似地，文件共享上的存储访问策略可以与共享访问签名相关联，后者授予对共享本身的权限，或对它包含的文件的权限。  
>
> 仅服务 SAS 支持存储访问策略。 帐户 SAS 不支持存储访问策略。  

## <a name="create-a-stored-access-policy"></a>创建存储访问策略

下面的代码在容器上创建存储访问策略。 可以使用访问策略指定对容器或其 Blob 上的服务 SAS 的约束。

```csharp
private static async Task CreateStoredAccessPolicyAsync(CloudBlobContainer container, string policyName)
{
    // Create a new stored access policy and define its constraints.
    // The access policy provides create, write, read, list, and delete permissions.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        // When the start time for the SAS is omitted, the start time is assumed to be the time when Azure Storage receives the request.
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
            SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create | SharedAccessBlobPermissions.Delete
    };

    // Get the container's existing permissions.
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();

    // Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    await container.SetPermissionsAsync(permissions);
}
```

## <a name="see-also"></a>另请参阅

- [使用共享访问签名 (SAS) 授予对 Azure 存储资源的有限访问权限](storage-sas-overview.md)
- [定义存储访问策略](https://docs.microsoft.com/rest/api/storageservices/define-stored-access-policy)

