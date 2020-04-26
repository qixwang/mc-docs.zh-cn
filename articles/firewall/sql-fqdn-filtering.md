---
title: 配置使用 SQL FQDN 的 Azure 防火墙应用程序规则
description: 本文介绍如何在 Azure 防火墙应用程序规则中配置 SQL FQDN。
services: firewall
author: rockboyfor
ms.service: firewall
ms.topic: article
origin.date: 07/19/2019
ms.date: 04/06/2020
ms.author: v-yeche
ms.openlocfilehash: 72a04f5b14e42137086c72835875a0a180fa74e5
ms.sourcegitcommit: 564739de7e63e19a172122856ebf1f2f7fb4bd2e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2020
ms.locfileid: "82093253"
---
# <a name="configure-azure-firewall-application-rules-with-sql-fqdns"></a>配置使用 SQL FQDN 的 Azure 防火墙应用程序规则

> [!IMPORTANT]
> 使用 SQL FQDN 的 Azure 防火墙应用程序规则目前以公共预览版提供。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[适用于 Azure 预览版的补充使用条款](https://www.azure.cn/support/legal/subscription-agreement/)。

现在，你可以配置使用 SQL FQDN 的 Azure 防火墙应用程序规则。 这样，就可以限制为在你的虚拟网络中只能访问指定的 SQL 服务器实例。

使用 SQL FQDN 可以筛选流量：

- 从 VNet 发往 Azure SQL 数据库或 Azure SQL 数据仓库的流量。 例如：仅允许访问 *sql-server1.database.chinacloudapi.cn*。
- 从本地发往 VNet 中运行的 Azure SQL 托管实例或 SQL IaaS 的流量。
- 从辐射到辐射基础结构发往 VNet 中运行的 Azure SQL 托管实例或 SQL IaaS 的流量。

在公共预览期，仅支持以[代理模式](/sql-database/sql-database-connectivity-architecture#connection-policy)（端口 1433）使用 SQL FQDN 筛选。 如果以默认的重定向模式使用 SQL，可以使用 SQL 服务标记作为[网络规则](overview.md#network-traffic-filtering-rules)的一部分来筛选访问流量。
如果将非默认端口用于 SQL IaaS 流量，可以在防火墙应用程序规则中配置这些端口。

目前，可以通过 Azure 门户、Azure CLI、REST 和模板在所有区域中使用包含 SQL FQDN 的应用程序规则。

## <a name="configure-using-azure-cli"></a>使用 Azure CLI 进行配置

1. [使用 Azure CLI 部署 Azure 防火墙](deploy-cli.md)。
2. 如果筛选发往 Azure SQL 数据库、SQL 数据仓库或 SQL 托管实例的流量，请确保将 SQL 连接模式设置为“代理”。  若要了解如何切换 SQL 连接模式，请参阅 [Azure SQL 连接体系结构](/sql-database/sql-database-connectivity-architecture)。 

    <!--Pending on [Azure SQL Connectivity Settings](/sql-database/sql-database-connectivity-settings#change-connection-policy-via-azure-cli)-->
    
    > [!NOTE]
    > 与重定向模式相比，SQL 代理模式可能会增大延迟。   若要继续使用重定向模式（在 Azure 中建立客户端连接的默认模式），可以在防火墙[网络规则](tutorial-firewall-deploy-portal.md#configure-a-network-rule)中使用 SQL [服务标记](service-tags.md)筛选访问流量。

3. 配置使用 SQL FQDN 的应用程序规则以允许访问 SQL 服务器：

    ```azurecli
    az extension add -n azure-firewall

    az network firewall application-rule create \
    -g FWRG \
    -f azfirewall \
    -c FWAppRules \
    -n srule \
    --protocols mssql=1433 \
    --source-addresses 10.0.0.0/24 \
    --target-fqdns sql-serv1.database.chinacloudapi.cn
    ```

## <a name="configure-using-the-azure-portal"></a>使用 Azure 门户进行配置
1. [使用 Azure CLI 部署 Azure 防火墙](deploy-cli.md)。
2. 如果筛选发往 Azure SQL 数据库、SQL 数据仓库或 SQL 托管实例的流量，请确保将 SQL 连接模式设置为“代理”。  若要了解如何切换 SQL 连接模式，请参阅 [Azure SQL 连接体系结构](../sql-database/sql-database-connectivity-architecture.md)。 

    <!--Pending on [Azure SQL Connectivity Settings](/sql-database/sql-database-connectivity-settings#change-azure-sql-database-connection-policy)-->

    > [!NOTE]
    > 与重定向模式相比，SQL 代理模式可能会增大延迟。   若要继续使用重定向模式（在 Azure 中建立客户端连接的默认模式），可以在防火墙[网络规则](tutorial-firewall-deploy-portal.md#configure-a-network-rule)中使用 SQL [服务标记](service-tags.md)筛选访问流量。
3. 在应用程序规则中添加相应的协议、端口和 SQL FQDN，然后选择“保存”。 
    
    ![使用 SQL FQDN 的应用程序规则](media/sql-fqdn-filtering/application-rule-sql.png)
4. 从通过防火墙筛选流量的 VNet 中的虚拟机访问 SQL。 
5. 验证 [Azure 防火墙日志](log-analytics-samples.md)是否显示允许流量。

## <a name="next-steps"></a>后续步骤

若要了解 SQL 代理和重定向模式，请参阅 [Azure SQL 数据库连接体系结构](../sql-database/sql-database-connectivity-architecture.md)。

<!-- Update_Description: new article about sql fqdn filtering -->
<!--ms.date: 07/22/2019-->