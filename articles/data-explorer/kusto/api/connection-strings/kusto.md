---
title: Kusto 连接字符串 - Azure 数据资源管理器 | Microsoft Docs
description: 本文介绍了 Azure 数据资源管理器中的 Kusto 连接字符串。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 03/24/2020
ms.date: 07/01/2020
ms.openlocfilehash: 66b2c79e9a9b7f58a8e158f60ba565a38f6815dd
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226369"
---
# <a name="kusto-connection-strings"></a>Kusto 连接字符串

Kusto 连接字符串可以为 Kusto 客户端应用程序提供与 Kusto 服务终结点建立连接所需的信息。 Kusto 连接字符串借鉴了 ADO.NET 连接字符串。 也就是说，连接字符串是一个以分号分隔的名称/值参数对列表，可以用单个 URI 作为前缀。

**示例：**

```text
https://help.kusto.chinacloudapi.cn/Samples; Fed=true; Accept=true
```

URI 提供要与之通信的服务终结点：

* (`https://help.kusto.chinacloudapi.cn`) - `Data Source` 属性的值。
* `Samples`（默认数据库）- `Initial Catalog` 属性的值。

使用名称/值语法提供两个附加属性： 

* `Fed` 属性（也称为 `AAD Federated Security`）设置为 `true`。
* `Accept` 属性设置为 `true`。

> [!NOTE]
>
> * 属性名称不区分大小写，并且忽略名称/值对之间的空格。
> * 属性值**区分**大小写。 包含分号 (`;`)、单引号 (`'`) 或双引号 (`"`) 的属性值必须用双引号引起来。

多个 Kusto 客户端工具支持基于连接字符串的 URI 前缀进行扩展，因为它们允许使用简写格式 `@` _ClusterName_ `/` _InitialCatalog_。
例如，这些工具将连接字符串 `@help/Samples` 转换为 `https://help.kusto.chinacloudapi.cn/Samples; Fed=true`，后者指示三个属性（`Data Source`、`Initial Catalog` 和 `AAD Federated Security`）。

可以采用编程方式通过 C# `Kusto.Data.KustoConnectionStringBuilder` 类分析和操作 Kusto 连接字符串。 此类验证所有连接字符串，并在验证失败时生成运行时异常。
此功能存在于所有形式的 Kusto SDK 中。

## <a name="connection-string-properties"></a>连接字符串属性

下表列出了可以在 Kusto 连接字符串中指定的所有属性。
它列出了编程名称（属性在 `Kusto.Data.KustoConnectionStringBuilder` 对象中的名称）以及作为别名的其他属性名称。

### <a name="general-properties"></a>常规属性

| 属性名称              | 别名                      | 编程名称  | 说明                                                                                                                          |
|----------------------------|----------------------------------------|--------------------|---------------------------------------------------|
| 跟踪的客户端版本 |                                        | TraceClientVersion | 跟踪客户端版本时，请使用此值   |
| 数据源                | 地址、网络地址、服务器 | 数据源         | 指定 Kusto 服务终结点的 URI。 例如，`https://mycluster.kusto.chinacloudapi.cn` 或 `net.tcp://localhost`               |
| 初始目录            | 数据库                               | InitialCatalog     | 将在默认情况下使用的数据库的名称。 例如 MyDatabase|
| 查询一致性          | QueryConsistency                       | QueryConsistency   | 设置为 `strongconsistency` 或 `weakconsistency`，用于决定查询在运行之前是否应与元数据同步 |

### <a name="user-authentication-properties"></a>用户身份验证属性

| 属性名称          | 别名                          | 编程名称 | 说明                       |
|------------------------|--------------------------------------------|-------------------|-----------------------------------|
| AAD 联合安全性 | 联合安全性、联合、AADFed | FederatedSecurity | 一个布尔值，指示客户端执行 Azure Active Directory (AAD) 联合身份验证  |
| 强制执行 MFA            | MFA、EnforceMFA                             | EnforceMfa        | 一个布尔值，指示客户端获取多重身份验证令牌       |
| 用户 ID                | UID、用户                                  | UserID            | 一个字符串值，指示客户端使用指定的用户名执行用户身份验证           |
| 跟踪的用户名  |                                            | TraceUserName     | 一个字符串值，用于向服务报告在内部跟踪请求时使用的用户名         |
| 用户令牌             | UsrToken、UserToken                        | UserToken         | 一个字符串值，指示客户端使用指定的持有者令牌执行用户身份验证。<br/>替代 ApplicationClientId、ApplicationKey 和 ApplicationToken。 （如果指定了此项，则会跳过实际的客户端身份验证流，转而使用提供的令牌。）       |
| 命名空间              | NS                                         | 命名空间         | （供将来使用）  |



### <a name="application-authentication-properties"></a>应用程序身份验证属性

|属性名称                                     |别名                         |编程名称                             |说明      |
|--------------------------------------------------|------------------------------------------|----------------------------------------------|-----------------|
|AAD 联合安全性                            |联合安全性、联合、AADFed|FederatedSecurity                             |一个布尔值，指示客户端执行 Azure Active Directory (AAD) 联合身份验证|
|应用程序证书指纹                |AppCert                                   |ApplicationCertificateThumbprint              |一个字符串值，该值提供使用应用程序客户端证书身份验证流时要使用的客户端证书的指纹|
|应用程序客户端 ID                             |AppClientId                               |ApplicationClientId                           |一个字符串值，该值提供进行身份验证时要使用的应用程序客户端 ID|
|应用程序密钥                                   |AppKey                                    |ApplicationKey                                |一个字符串值，该值提供使用应用程序机密流进行身份验证时要使用的应用程序密钥|
|跟踪的应用程序名称                      |TraceAppName                              |ApplicationNameForTracing                     |一个字符串值，用于向服务报告在内部跟踪请求时使用的应用程序名称|
|应用程序令牌                                 |AppToken                                  |ApplicationToken                              |一个字符串值，指示客户端使用指定的持有者令牌执行应用程序身份验证|
|颁发机构 ID                                      |TenantId                                  |颁发机构                                     |一个字符串值，该值提供在其中注册应用程序的租户的名称或 ID|
|                                                  |                                          |EmbeddedManagedIdentity                       |一个字符串值，指示客户端使用哪个应用程序标识进行托管标识身份验证；使用 `system` 来指示系统分配的标识。 此属性不能通过连接字符串进行设置，只能以编程方式设置。|ManagedServiceIdentity                        |TODO|
|应用程序证书使用者可分辨名称|应用程序证书使用者           |ApplicationCertificateSubjectDistinguishedName||
|应用程序证书颁发者可分辨名称 |应用程序证书颁发者            |ApplicationCertificateIssuerDistinguishedName ||
|应用程序证书发送公共证书   |应用程序证书 SendX5c、SendX5c  |ApplicationCertificateSendPublicCertificate   ||



### <a name="client-communication-properties"></a>客户端通信属性

|属性名称                      |别名|编程名称  |说明                                                   |
|-----------------------------------|-----------------|-------------------|--------------------------------------------------------------|
|Accept      ||Accept      |一个布尔值，它请求失败时返回的详细错误对象。|
|流式处理   ||流式处理   |一个布尔值，它请求客户端在将数据提供给调用方之前不累积数据。|
|未压缩||未压缩|一个布尔值，它请求客户端不要求进行传输级压缩。|

## <a name="authentication-properties-details"></a>身份验证属性（详细信息）

连接字符串的重要任务之一是告诉客户端如何向服务进行身份验证。
客户端针对 HTTP/HTTPS 终结点进行身份验证时通常使用以下算法：

1. 如果 AadFederatedSecurity 为 true：
    1. 如果指定了 UserToken，则使用指定的令牌进行 AAD 联合身份验证
    1. 否则，如果指定了 ApplicationToken，则使用指定的令牌执行联合身份验证
    1. 否则，如果指定了 ApplicationClientId 和 ApplicationKey，则使用指定的应用程序客户端 ID 和密钥执行联合身份验证
    1. 否则，如果指定了 ApplicationClientId 和 ApplicationCertificateThumbprint，则使用指定的应用程序客户端 ID 和证书执行联合身份验证
    1. 否则，将使用当前登录用户的标识执行联合身份验证（如果这是会话中的第一次身份验证，系统会提示用户）

1. 否则，不进行身份验证。


### <a name="aad-federated-application-authentication-with-application-certificate"></a>使用应用程序证书的 AAD 联合应用程序身份验证

1. 基于应用程序证书的身份验证仅支持 Web 应用程序（不支持原生客户端应用程序）。
1. 应将 Web 应用程序配置为接受给定的证书。 [如何基于 AAD 应用程序的证书进行身份验证](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)
1. 应将 Web 应用程序配置为相关 Kusto 群集中的经授权主体。
1. 应安装具有给定指纹的证书（安装位置为本地计算机存储或当前用户存储）。
1. 证书的公钥应至少包含 2048 位。

## <a name="aad-based-authentication-examples"></a>基于 AAD 的身份验证示例

**使用当前登录用户标识的 AAD 联合身份验证（系统会根据需要提示用户）**

```csharp
var serviceUri = "Service URI, typically of the form https://cluster.region.kusto.chinacloudapi.cn";
var authority = "contoso.com"; // Or the AAD tenant GUID: "..."

// Recommended syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
  .WithAadUserPromptAuthentication(authority);

// Legacy syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
{
    FederatedSecurity = true,
    InitialCatalog = "NetDefaultDB",
    Authority = authority,
};

// Equivalent Kusto connection string: $"Data Source={serviceUri};Database=NetDefaultDB;Fed=True;Authority Id={authority}"
```

**使用用户 ID 提示的 AAD 联合身份验证（系统会根据需要提示用户）**

```csharp
var serviceUri = "Service URI, typically of the form https://cluster.region.kusto.chinacloudapi.cn";
var authority = "contoso.com"; // Or the AAD tenant GUID: "..."
var userUPN = "johndoe@contoso.com";

// Recommended syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
  .WithAadUserPromptAuthentication(authority);
kustoConnectionStringBuilder.UserID = userUPN;

// Legacy syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
{
    FederatedSecurity = true,
    InitialCatalog = "NetDefaultDB",
    UserID = userUPN,
    Authority = authority,
};

// Equivalent Kusto connection string: $"Data Source={serviceUri};Database=NetDefaultDB;Fed=True;User ID={userUPN};Authority Id={authority}"
```

**使用 ApplicationClientId 和 ApplicationKey 的 AAD 联合应用程序身份验证**

```csharp
var serviceUri = "Service URI, typically of the form https://cluster.region.kusto.chinacloudapi.cn";
var authority = "contoso.com"; // Or the AAD tenant GUID: "..."
var applicationClientId = <ApplicationClientId>;
var applicationKey = <ApplicationKey>;

// Recommended syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
    .WithAadApplicationKeyAuthentication(applicationClientId, applicationKey, authority);

// Legacy syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
{
    FederatedSecurity = true,
    InitialCatalog = "NetDefaultDB",
    ApplicationClientId = applicationClientId,
    ApplicationKey = applicationKey,
    Authority = authority,
};

// Equivalent Kusto connection string: $"Data Source={serviceUri};Database=NetDefaultDB;Fed=True;AppClientId={applicationClientId};AppKey={applicationKey};Authority Id={authority}"
```

**使用用户/应用程序令牌的 AAD 联合身份验证**

```csharp
var serviceUri = "Service URI, typically of the form https://cluster.region.kusto.chinacloudapi.cn";
var authority = "contoso.com"; // Or the AAD tenant GUID: "..."
var access_token = "<access token obtained from AAD>"

// Recommended syntax - AAD User token
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
    .WithAadUserTokenAuthentication(access_token, authority);

// Legacy syntax - AAD User token
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
{
    FederatedSecurity = true,
    UserToken = access_token,
    Authority = authority,
};

// Equivalent Kusto connection string: "Data Source={serviceUri};Database=NetDefaultDB;Fed=True;UserToken={access_token};Authority Id={authority}"

// Recommended syntax - AAD Application token
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
    .WithAadApplicationTokenAuthentication(access_token, authority);

// Legacy syntax - AAD Application token
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
{
    FederatedSecurity = true,
    ApplicationToken = access_token,
    Authority = authority,
};

// Equivalent Kusto connection string: $"Data Source={serviceUri};Database=NetDefaultDB;Fed=True;AppToken={applicationToken};Authority Id={authority}"
```

**使用令牌提供程序回调（将在每次需要令牌时调用）**

```csharp
var serviceUri = "Service URI, typically of the form https://cluster.region.kusto.chinacloudapi.cn";
Func<string> tokenProviderCallback; // User-defined method to retrieve the access token

// Recommended syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
    .WithAadTokenProviderAuthentication(tokenProviderCallback);

// Legacy syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
{
    FederatedSecurity = true,
    TokenProviderCallback = () => Task.FromResult(tokenProviderCallback()),
};
```

**使用托管标识**

```csharp
var serviceUri = "Service URI, typically of the form https://cluster.region.kusto.chinacloudapi.cn";
var managedIdentity = "<managed identity>"; // For system-assigned identity use "system"

// Recommended syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
    .WithAadManagedIdentity(managedIdentity);

// Legacy syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
{
    FederatedSecurity = true,
    EmbeddedManagedIdentity = managedIdentity,
};
```

**使用 X.509 证书**

```csharp
var serviceUri = "Service URI, typically of the form https://cluster.region.kusto.chinacloudapi.cn";
var authority = "contoso.com"; // Or the AAD tenant GUID: "..."
string applicationClientId = "<applicationClientId>";
X509Certificate2 applicationCertificate = "<certificate blob>";
bool sendX5c = <desired value>; // Set too 'True' to use Trusted Issuer feature of AAD

// Recommended syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
    .WithAadApplicationCertificateAuthentication(applicationClientId, applicationCertificate, authority, sendX5c);

// Legacy syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
{
    FederatedSecurity = true,
    ApplicationClientId = applicationClientId,
    ApplicationCertificateBlob = applicationCertificate,
    ApplicationCertificateSendX5c = sendX5c,
    Authority = authority,
};
```

**使用 X.509 证书的指纹（客户端会尝试从本地存储加载证书）**

```csharp
var serviceUri = "Service URI, typically of the form https://cluster.region.kusto.chinacloudapi.cn";
var authority = "contoso.com"; // Or the AAD tenant GUID: "..."
string applicationClientId = "<applicationClientId>";
string applicationCertificateThumbprint = "<ApplicationCertificateThumbprint>";

// Recommended syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
    .WithAadApplicationThumbprintAuthentication(applicationClientId, applicationCertificateThumbprint, authority);

// Legacy syntax
var kustoConnectionStringBuilder = new KustoConnectionStringBuilder(serviceUri)
{
    FederatedSecurity = true,
    ApplicationClientId = applicationClientId,
    ApplicationCertificateThumbprint = applicationCertificateThumbprint,
    Authority = authority,
};

// Equivalent Kusto connection string: $"Data Source={serviceUri};Database=NetDefaultDB;Fed=True;AppClientId={applicationClientId};AppCert={applicationCertificateThumbprint};Authority Id={authority}"
```
