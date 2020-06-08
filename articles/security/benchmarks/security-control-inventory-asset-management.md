---
title: Azure 安全控制 - 清单和资产管理
description: Azure 安全控制 - 清单和资产管理
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: v-tawe
ms.custom: security-benchmark
origin.date: 04/14/2020
ms.openlocfilehash: 829256ca3a6ce3ba0f80cc005c0304463a7f53d7
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84200091"
---
# <a name="security-control-inventory-and-asset-management"></a>安全控制：清单和资产管理

清单和资产管理建议侧重于解决与主动管理（清点、跟踪和更正）所有 Azure 资源相关的问题，以便仅向已获授权的资源授予访问权限，并标识和删除未授权的资源和非管理资源。

## <a name="61-use-automated-asset-discovery-solution"></a>6.1：使用自动化资产发现解决方案

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 6.1 | 1.1、1.2、1.3、1.4、9.1、12.1 | 客户 |

使用 Azure Resource Graph 查询/发现订阅中的所有资源（例如计算、存储、网络、端口和协议等）。  确保租户中具有适当的（读取）权限，并枚举所有 Azure 订阅以及订阅中的资源。

尽管可以通过 Resource Graph 发现经典 Azure 资源，但我们强烈建议你今后还是创建并使用 Azure 资源管理器资源。

- [如何使用 Azure Resource Graph 创建查询](https://docs.azure.cn/governance/resource-graph/first-query-portal)

- [如何查看 Azure 订阅](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [了解 Azure RBAC](https://docs.azure.cn/role-based-access-control/overview)

## <a name="62-maintain-asset-metadata"></a>6.2：维护资产元数据

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 6.2 | 1.5 | 客户 |

将标记应用到 Azure资源，以便有条理地将元数据组织成某种分类。

- [如何创建和使用标记](https://docs.azure.cn/azure-resource-manager/resource-group-using-tags)

## <a name="63-delete-unauthorized-azure-resources"></a>6.3：删除未经授权的 Azure 资源

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 6.3 | 1.6 | 客户 |

在适用的情况下，请使用标记、管理组和单独的订阅来组织和跟踪资产。 定期核对清单，确保及时地从订阅中删除未经授权的资源。


- [如何创建管理组](https://docs.azure.cn/governance/management-groups/create)

- [如何创建和使用标记](https://docs.azure.cn/azure-resource-manager/resource-group-using-tags)

## <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6.4：定义并维护已获批 Azure 资源的清单

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 6.4 | 2.1 | 客户 |

根据组织需求，创建已获批 Azure 资源以及已获批用于计算资源的软件的清单。

## <a name="65-monitor-for-unapproved-azure-resources"></a>6.5：监视未批准的 Azure 资源

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 6.5 | 2.3、2.4 | 客户 |

使用 Azure Policy 对可以在订阅中创建的资源类型施加限制。

使用 Azure Resource Graph 查询/发现订阅中的资源。  确保环境中存在的所有 Azure 资源已获得批准。

- [如何配置和管理 Azure Policy](https://docs.azure.cn/governance/policy/tutorials/create-and-manage)

- [如何使用 Azure Graph 创建查询](https://docs.azure.cn/governance/resource-graph/first-query-portal)


## <a name="66-remove-unapproved-azure-resources-and-software-applications"></a>6.6：删除未批准的 Azure 资源和软件应用程序

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 6.6 | 2.5 | 客户 |

使用 Azure 安全中心的文件完整性监视（更改跟踪）来识别虚拟机上安装的所有软件。 可以实现自己的未授权软件删除过程。 还可以使用第三方解决方案来识别未获批软件。

- [如何使用文件完整性监视](https://docs.azure.cn/security-center/security-center-file-integrity-monitoring#using-file-integrity-monitoring)



## <a name="67-use-only-approved-applications"></a>6.7：仅使用已批准的应用程序

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 6.7 | 2.6 | 客户 |

使用 Azure 安全中心自适应应用程序控制确保仅执行已授权软件，并阻止所有未授权软件在 Azure 虚拟机上执行。

- [如何使用 Azure 安全中心自适应应用程序控制](https://docs.azure.cn/security-center/security-center-adaptive-application)

## <a name="68-use-only-approved-azure-services"></a>6.8：仅使用已批准的 Azure 服务

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 6.8 | 2.6 | 客户 |

使用 Azure Policy 限制可在环境中预配的服务。

- [如何配置和管理 Azure Policy](https://docs.azure.cn/governance/policy/tutorials/create-and-manage)

- [如何使用 Azure Policy 拒绝特定的资源类型](https://docs.azure.cn/governance/policy/samples/not-allowed-resource-types)

## <a name="69-maintain-an-inventory-of-approved-software-titles"></a>6.9：维护已获批软件的清单

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 6.9 | 2.7 | 客户 |

使用 Azure 安全中心自适应应用程序控制指定规则可能适用或不适用的文件类型。

如果该方案不满足要求，则实施第三方解决方案。

- [如何使用 Azure 安全中心自适应应用程序控制](https://docs.azure.cn/security-center/security-center-adaptive-application)


## <a name="610-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.10：限制用户在计算资源中执行脚本的功能

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 6.10 | 2.9 | 客户 |

根据脚本的类型，可以使用特定于操作系统的配置或第三方资源来限制用户在 Azure 计算资源中执行脚本的能力。  还可以利用 Azure 安全中心自适应应用程序控制来确保仅执行已授权软件，并阻止所有未授权软件在 Azure 虚拟机上执行。

- [如何在 Windows 环境中控制 PowerShell 脚本的执行](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6)

- [如何使用 Azure 安全中心自适应应用程序控制](https://docs.azure.cn/security-center/security-center-adaptive-application)

## <a name="611-physically-or-logically-segregate-high-risk-applications"></a>6.11：以物理或逻辑方式隔离高风险应用程序

| Azure ID | CIS ID | 责任方 |
|--|--|--|
| 6.11 | 2.9 | 客户 |

业务运营所需的软件可能会给组织带来更高的风险，应将其隔离在其自己的虚拟机和/或虚拟网络中，并通过 Azure 防火墙或网络安全组进行充分的保护。

- [如何创建虚拟网络](https://docs.azure.cn/virtual-network/quick-create-portal)

- [如何创建采用安全配置的 NSG](https://docs.azure.cn/virtual-network/tutorial-filter-network-traffic)


## <a name="next-steps"></a>后续步骤

- 请参阅下一个安全控制：[安全配置](security-control-secure-configuration.md)