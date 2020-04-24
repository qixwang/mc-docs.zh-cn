---
title: 连接体系结构 - Azure Database for MariaDB
description: 介绍 Azure Database for MariaDB 服务器的连接体系结构。
author: WenJason
ms.author: v-jay
ms.service: mariadb
ms.topic: conceptual
origin.date: 12/02/2019
ms.date: 01/13/2020
ms.openlocfilehash: 2542b169ca6174c55bd89f241322c19f0930cc6e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75776759"
---
# <a name="connectivity-architecture-in-azure-database-for-mariadb"></a>Azure Database for MariaDB 中的连接体系结构
本文介绍 Azure Database for MariaDB 的连接体系结构，以及如何在 Azure 内部和外部将流量从客户端定向到 Azure Database for MariaDB 实例。

## <a name="connectivity-architecture"></a>连接体系结构

通过网关建立与 Azure Database for MariaDB 的连接，该网关负责将传入连接路由到群集中服务器的物理位置。 下图演示了流量流。

![连接体系结构概述](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

客户端在连接到数据库时，会获得一个用于连接到网关的连接字符串。 该网关有一个公共 IP 地址，用于侦听端口 3306。 在数据库群集中，流量将转发到相应的 Azure Database for MariaDB。 因此，为了通过某种方式（例如，通过公司网络）连接到服务器，必须打开客户端防火墙，使出站流量能够访问我们的网关。 下面是一个按区域分类的可供我们的网关使用的 IP 地址的完整列表。

## <a name="azure-database-for-mariadb-gateway-ip-addresses"></a>Azure Database for MariaDB 网关 IP 地址

下表列出了所有数据区域的 Azure Database for MariaDB 网关的主要 IP 和次要 IP。 主 IP 地址是网关的当前 IP 地址，第二个 IP 地址是主 IP 地址故障时使用的故障转移 IP 地址。 如前所述，客户应该允许到这两个 IP 地址的出站流量。 第二个 IP 地址不侦听任何服务，除非 Azure Database for MariaDB 激活该地址，使之接受连接。

| **区域名称** | **网关 IP 地址** |
|:----------------|:-------------|
| 中国东部 2 | 40.73.82.1  |
| 中国北部 2 | 40.73.50.0     |
||||

## <a name="next-steps"></a>后续步骤

* [使用 Azure 门户创建和管理 Azure Database for MariaDB 防火墙规则](./howto-manage-firewall-portal.md)
* [使用 Azure CLI 创建和管理 Azure Database for MariaDB 防火墙规则](./howto-manage-firewall-cli.md)
