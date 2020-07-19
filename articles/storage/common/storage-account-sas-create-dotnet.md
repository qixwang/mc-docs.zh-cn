---
title: 使用 .NET 创建帐户 SAS
titleSuffix: Azure Storage
description: 了解如何使用 .NET 客户端库创建帐户共享访问签名 (SAS)。
services: storage
author: WenJason
ms.service: storage
ms.topic: how-to
origin.date: 08/06/2019
ms.date: 07/20/2020
ms.author: tamram
ms.reviewer: dineshm
ms.subservice: common
ms.openlocfilehash: 5bc8753bf49cfc029ce036b38f0b51a4b2b81a87
ms.sourcegitcommit: 31da682a32dbb41c2da3afb80d39c69b9f9c1bc6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2020
ms.locfileid: "86414701"
---
# <a name="create-an-account-sas-with-net"></a>使用 .NET 创建帐户 SAS

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

本文介绍如何使用存储帐户密钥通过[用于 .NET 的 Azure 存储客户端库](https://docs.azure.cn/zh-cn/dotnet/api/overview/storage?view=azure-dotnet)创建帐户 SAS。

## <a name="create-an-account-sas"></a>创建帐户 SAS

若要为容器创建帐户 SAS，请调用 [CloudStorageAccount.GetSharedAccessSignature](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.getsharedaccesssignature) 方法。

以下代码示例创建一个帐户 SAS，该 SAS 对 Blob 和文件服务是有效的，并授予客户端读取、写入和列表权限，使其能够访问服务级别 API。 帐户 SAS 将协议限制为 HTTPS，因此请求必须使用 HTTPS 发出。 请务必将尖括号中的占位符值替换为你自己的值：

```csharp
static string GetAccountSASToken()
{
    // To create the account SAS, you need to use Shared Key credentials. Modify for your account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=<storage-account>;AccountKey=<account-key>;EndpointSuffix=core.chinacloudapi.cn";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create a new access policy for the account.
    SharedAccessAccountPolicy policy = new SharedAccessAccountPolicy()
        {
            Permissions = SharedAccessAccountPermissions.Read | SharedAccessAccountPermissions.Write | SharedAccessAccountPermissions.List,
            Services = SharedAccessAccountServices.Blob | SharedAccessAccountServices.File,
            ResourceTypes = SharedAccessAccountResourceTypes.Service,
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Protocols = SharedAccessProtocol.HttpsOnly
        };

    // Return the SAS token.
    return storageAccount.GetSharedAccessSignature(policy);
}
```

## <a name="use-an-account-sas-from-a-client"></a>通过客户端使用帐户 SAS

若要使用帐户 SAS 访问 Blob 服务的服务级别 API，请使用存储帐户的 SAS 和 Blob 存储终结点构造 Blob 服务客户端对象。 请务必将尖括号中的占位符值替换为你自己的值：

```csharp
static void UseAccountSAS(string sasToken)
{
    // Create new storage credentials using the SAS token.
    StorageCredentials accountSAS = new StorageCredentials(sasToken);
    // Use these credentials and the account name to create a Blob service client.
    CloudStorageAccount accountWithSAS = new CloudStorageAccount(accountSAS, "<storage-account>", endpointSuffix: core.chinacloudapi.cn, useHttps: true);
    CloudBlobClient blobClientWithSAS = accountWithSAS.CreateCloudBlobClient();

    // Now set the service properties for the Blob client created with the SAS.
    blobClientWithSAS.SetServiceProperties(new ServiceProperties()
    {
        HourMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        MinuteMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        Logging = new LoggingProperties()
        {
            LoggingOperations = LoggingOperations.All,
            RetentionDays = 14,
            Version = "1.0"
        }
    });

    // The permissions granted by the account SAS also permit you to retrieve service properties.
    ServiceProperties serviceProperties = blobClientWithSAS.GetServiceProperties();
    Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
    Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
    Console.WriteLine(serviceProperties.HourMetrics.Version);
}
```

## <a name="next-steps"></a>后续步骤

- [使用共享访问签名 (SAS) 授予对 Azure 存储资源的有限访问权限](storage-sas-overview.md)
- [创建帐户 SAS](https://docs.microsoft.com/rest/api/storageservices/create-account-sas)
