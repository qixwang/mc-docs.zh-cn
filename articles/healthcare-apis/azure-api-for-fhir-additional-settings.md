---
title: Azure API for FHIR 的其他设置
description: 可为 Azure API for FHIR 指定的其他设置的概述
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.reviewer: mihansen
ms.author: cavoeg
author: CaitlinV39
ms.date: 11/22/2019
ms.openlocfilehash: cbe20a3221a30aa59d18dc401569166bc8e33ce6
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80343870"
---
# <a name="additional-settings-for-azure-api-for-fhir"></a>Azure API for FHIR 的其他设置

在本操作指南中，我们将了解可在 Azure API for FHIR 中指定的其他设置。 一些附加页面提供了更深入的详细信息。

## <a name="configure-database-settings"></a>配置数据库设置

Azure API for FHIR 使用数据库存储其数据。 底层数据库的性能取决于预配服务期间选择的请求单位 (RU) 数，以及预配服务后数据库设置中的 RU 数。

必须预配吞吐量才能确保始终为数据库提供足够的系统资源。 应用程序所需的 RU 数取决于执行的操作。 操作既包括简单的读写，也包括更复杂的查询。

有关如何更改默认设置的详细信息，请参阅[配置数据库设置](configure-database.md)。

## <a name="find-identity-object-ids"></a>查找标识对象 ID
完全托管式的 Azure API for FHIR 服务配置为仅允许一系列预定义的标识对象 ID 进行访问。 应用程序或用户在尝试访问 FHIR API 时必须提供持有者令牌。 此持有者令牌将包含特定的声明（字段）。 若要授予对 FHIR API 的访问权限，该令牌必须包含正确的颁发者 (`iss`)、受众 (`aud`)，以及允许的对象 ID 列表中的某个对象 ID (`oid`)。 标识对象 ID 是 Azure Active Directory 中的用户或服务主体的对象 ID。

创建新的 Azure API for FHIR 实例时，可以配置允许的对象 ID 列表。 若要配置此列表，请参阅有关[查找标识对象 ID](find-identity-object-ids.md) 的操作指南。

## <a name="enable-diagnostic-logging"></a>启用诊断日志记录
在设置过程中，你可能想要启用诊断日志记录，以便能够出于合规目的监视服务并获取准确的报告。 有关如何设置诊断日志记录的详细信息，请参阅有关如何设置诊断日志记录的[操作指南](enable-diagnostic-logging.md)以及一些示例查询。 

## <a name="use-custom-headers-to-add-data-to-audit-logs"></a>使用自定义标头将数据添加到审核日志
在 Azure API for FHIR 中，你可能想要在日志中包含来自调用方系统的其他信息。 为此，可以使用自定义标头。

可以使用自定义标头来捕获多种类型的信息。 例如：

* 标识或授权信息
* 调用方来源
* 发起方组织
* 客户端系统详细信息（电子健康记录、患者门户）

若要将此数据添加到审核日志，请参阅[使用自定义 HTTP 标头将数据添加到审核日志](use-custom-headers.md)操作指南。

## <a name="next-steps"></a>后续步骤

在本操作指南中，你为 Azure API for FHIR 指定了其他设置。

接下来，请查看有关创建可读取 FHIR 数据的 Web 应用程序的教程系列。

>[!div class="nextstepaction"]
>[部署 JavaScript 应用程序](tutorial-web-app-fhir-server.md)