---
title: Apache Kafka REST 代理 - Azure HDInsight
description: 了解如何在 Azure HDInsight 上使用 Kafka REST 代理执行 Apache Kafka 操作。
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
origin.date: 12/17/2019
ms.date: 01/13/2020
ms.openlocfilehash: 09bbc8af262e4712664f54f336ef04c32f8669de
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75631404"
---
# <a name="interact-with-apache-kafka-clusters-in-azure-hdinsight-using-a-rest-proxy"></a>使用 REST 代理与 Azure HDInsight 中的 Apache Kafka 群集交互

使用 Kafka REST 代理可以通过基于 HTTP 的 REST API 来与 Kafka 群集交互。 这意味着，Kafka 客户端可以在虚拟网络的外部。 此外，客户端可以发出简单的 HTTP 调用来与 Kafka 群集相互发送和接收消息，而无需依赖 Kafka 库。  

## <a name="background"></a>背景

### <a name="architecture"></a>体系结构

如果不使用 REST 代理，则 Kafka 客户端需要与 Kafka 群集位于同一 VNet 中，或者位于对等互连的 VNet 中。 使用 REST 代理可以连接到位于任何位置的数据生成者或使用者。 部署 REST 代理会为群集创建新的公共终结点，可以在门户设置中找到此终结点。

有关 API 支持的操作的完整规范，请参阅 [Apache Kafka REST 代理 API](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy)。

### <a name="security"></a>安全性

使用 Azure Active Directory 安全组来管理对 Kafka REST 代理的访问。 有关详细信息，请参阅[使用 Azure Active Directory 组管理应用和资源访问](/active-directory/fundamentals/active-directory-manage-groups)。

创建启用 REST 代理的 Kafka 群集时，需要提供有权访问 REST 终结点的 AAD 安全组。 组所有者应将需要访问 REST 代理的 Kafka 客户端（应用程序）注册到此组。 组所有者可以通过门户或 Powershell 执行此操作。

在向 REST 代理终结点发出请求之前，客户端应用程序应获取 OAuth 令牌来验证适当安全组的成员身份。 有关 OAuth 令牌工作原理的详细信息，请参阅[使用 OAuth 2.0 代码授权流来授权访问 Azure Active Directory Web 应用程序](../../active-directory/develop/v1-protocols-oauth-code.md)。 有关在 Python 中提取 OAuth 令牌的示例，请参阅[客户端应用程序示例](#client-application-sample)

客户端应用程序获取 OAuth 令牌后，必须在向 REST 代理发出的 HTTP 请求中传递该令牌。

## <a name="prerequisites"></a>先决条件

1. 将一个应用程序注册到 Azure AD。 编写的用来与 Kafka REST 代理交互的客户端应用程序将使用此应用程序的 ID 和机密对 Azure 进行身份验证。
1. 创建一个 Azure AD 安全组，并将已注册到 Azure AD 的应用程序添加到该安全组。 此安全组将用于控制允许哪些应用程序与 REST 代理交互。 有关创建 Azure AD 组的详细信息，请参阅[使用 Azure Active Directory 创建基本组并添加成员](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)。

## <a name="create-a-kafka-cluster-with-rest-proxy-enabled"></a>创建启用 REST 代理的 Kafka 群集

1. 在运行 Kafka 群集创建工作流期间，在“安全性 + 网络”选项卡中，选中“启用 Kafka REST 代理”选项。

     ![启用 Kafka REST 代理并选择安全组](./media/apache-kafka-rest-proxy/apache-kafka-rest-proxy-enable.png)

1. 单击“选择安全组”。  从安全组列表中，选择你要允许其访问 REST 代理的安全组。 可以使用搜索框查找适当的安全组。 单击底部的“选择”按钮  。

     ![启用 Kafka REST 代理并选择安全组](./media/apache-kafka-rest-proxy/apache-kafka-rest-proxy-select-security-group.png)

1. 根据[使用 Azure 门户在 Azure HDInsight 中创建 Apache Kafka 群集](/hdinsight/kafka/apache-kafka-get-started)中所述，完成创建群集的剩余步骤。

1. 创建群集后，转到群集属性并记下 Kafka REST 代理 URL。

     ![查看 REST 代理 URL](./media/apache-kafka-rest-proxy/apache-kafka-rest-proxy-view-proxy-url.png)

## <a name="client-application-sample"></a>客户端应用程序示例

可使用以下 Python 代码来与 Kafka 群集上的 REST 代理交互。 此代码将执行以下操作：

1. 从 Azure AD 提取 OAuth 令牌
1. 创建指定的主题
1. 向该主题发送消息
1. 使用来自该主题的消息

有关在 Python 中获取 OAuth 令牌的详细信息，请参阅 [Python AuthenticationContext 类](https://docs.microsoft.com/python/api/adal/adal.authentication_context.authenticationcontext?view=azure-python)。 未创建的或已通过 Kafka REST 代理删除的主题可能会经过一段延迟时间才反映此状态。 此延迟是缓存刷新造成的。

```python
#Required python packages
#pip3 install adal
#pip install msrestazure

import adal
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD
import requests

#--------------------------Configure these properties-------------------------------#
# Tenant ID for your Azure Subscription
tenant_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
# Your Client Application Id
client_id = 'XYZABCDE-1234-1234-1234-ABCDEFGHIJKL'
# Your Client Credentials
client_secret = 'password'
# kafka rest proxy -endpoint
kafkarest_endpoint = "https://<clustername>-kafkarest.azurehdinsight.net"
#--------------------------Configure these properties-------------------------------#

#getting token
login_endpoint = AZURE_PUBLIC_CLOUD.endpoints.active_directory
resource = "https://hib.azurehdinsight.net"
context = adal.AuthenticationContext(login_endpoint + '/' + tenant_id)

token = context.acquire_token_with_client_credentials(
    resource,
    client_id,
    client_secret)

accessToken = 'Bearer ' + token['accessToken']

print(accessToken)

# relative url
getstatus = "/v1/metadata/topics"
request_url = kafkarest_endpoint + getstatus

# sending get request and saving the response as response object
response = requests.get(request_url, headers={'Authorization': accessToken})
print(response.content)
```

若要使用代码示例，请执行以下步骤：

1. 在装有 Python 的计算机上保存示例代码。
1. 执行 `pip3 install adal` 和 `pip install msrestazure` 安装所需的 Python 依赖项。
1. 修改代码并更新环境的以下属性：
    1.  *租户 ID* – 订阅所在的 Azure 租户。
    1.  *客户端 ID* – 在安全组中注册的应用程序的 ID。
    1.  *客户端机密* – 在安全组中注册的应用程序的机密。
    1.  *Kafkarest_endpoint* – 根据[部署部分](#create-a-kafka-cluster-with-rest-proxy-enabled)所述，从群集概述中的“属性”选项卡获取此值。 此属性应采用以下格式 – `https://<clustername>-kafkarest.azurehdinsight.cn`
3. 在命令行中，通过执行 `python <filename.py>` 来执行 Python 文件

## <a name="next-steps"></a>后续步骤

* [Kafka REST 代理 API 参考文档](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy/)
* [教程：使用 Apache Kafka 生成者和使用者 API](apache-kafka-producer-consumer-api.md)