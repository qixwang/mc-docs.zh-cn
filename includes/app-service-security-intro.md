---
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 04/15/2020
ms.date: 06/22/2020
ms.author: v-tawe
ms.openlocfilehash: 29f428f629f2c76c9cd995f3bc5766bcae469f1e
ms.sourcegitcommit: d24e12d49708bbe78db450466eb4fccbc2eb5f99
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85613338"
---
应用服务的各个平台组件（包括 Azure VM、存储、网络连接、Web 框架、管理和集成功能）都得到了积极保护和加强。 应用服务持续进行严格的符合性检查，以确保：

- 你的应用资源与其他客户的 Azure 资源隔离开来，从而[受到保护](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)。
- [定期更新 VM 实例和运行时软件](../articles/app-service/overview-patch-os-runtime.md)，以解决新发现的漏洞。 
- 你的应用与其他 Azure 资源（例如 [SQL 数据库](https://azure.microsoft.com/services/sql-database/)）之间的密钥（例如连接字符串）通信一直在 Azure 中进行，不跨越任何网络边界。 存储密钥时始终对其加密。
- 通过应用服务连接功能（例如[混合连接](../articles/app-service/app-service-hybrid-connections.md)）进行的所有通信均已加密。 
- 与 Azure PowerShell、Azure CLI、Azure SDK、REST API 等远程管理工具的连接均已加密。
- 24 小时威胁管理可保护基础结构和平台免受恶意软件、分布式拒绝服务 (DDoS)、中间人 (MITM) 和其他威胁的危害。

<!-- For more information on infrastructure and platform security in Azure, see [Azure Trust Center](https://azure.microsoft.com/overview/trusted-cloud/). -->