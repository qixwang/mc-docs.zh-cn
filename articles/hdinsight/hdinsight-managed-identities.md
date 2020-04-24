---
title: Azure HDInsight 中的托管标识
description: 提供 Azure HDInsight 中托管标识的实现概述。
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
origin.date: 11/20/2019
ms.date: 03/23/2020
ms.openlocfilehash: 552cd21cd8cb55884f82e60cded5abd023520b98
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79295962"
---
# <a name="managed-identities-in-azure-hdinsight"></a>Azure HDInsight 中的托管标识

托管标识是在 Azure Active Directory (Azure AD) 中注册的标识，其凭据由 Azure 管理。 使用托管标识，无需在 Azure AD 中注册服务主体，也无需维护证书等凭据。

可以在 Azure HDInsight 中使用托管标识，根据需要访问 Azure AD 域服务或访问 Azure Data Lake Storage Gen2 中的文件。

有两种类型的托管标识：用户分配的托管标识和系统分配的托管标识。 Azure HDInsight 仅支持用户分配的托管标识。 HDInsight 不支持系统分配的托管标识。 用户分配的托管标识创建为独立的 Azure 资源，可将其分配到一个或多个 Azure 服务实例。 相比之下，系统分配的托管标识是在 Azure AD 中创建的，系统会自动在特定的 Azure 服务实例上直接启用它。 然后，系统分配的该托管标识的生存期将绑定到启用该托管标识的服务实例的生存期。

## <a name="hdinsight-managed-identity-implementation"></a>HDInsight 托管标识的实现

在 Azure HDInsight 中，托管标识是在群集的每个节点上预配的。 但是，这些标识组件只可由 HDInsight 服务使用。 目前没有任何支持的方法可用于通过 HDInsight 群集节点上安装的托管标识生成访问令牌。 对于某些 Azure 服务，托管标识是使用某个终结点实现的。使用该终结点，可以自行获取用来与其他 Azure 服务交互的访问令牌。

## <a name="create-a-managed-identity"></a>创建托管标识

可以通过以下任何方法创建托管标识：

* [Azure 门户](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* [Azure 资源管理器](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)
* [Azure CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)

托管标识的剩余配置步骤取决于使用该托管标识的方案。

## <a name="managed-identity-scenarios-in-azure-hdinsight"></a>Azure HDInsight 中的托管标识方案

Azure HDInsight 中的多种方案都会使用托管标识。 有关详细的设置和配置说明，请参阅相关文档：

* [Azure Data Lake Storage Gen2](hdinsight-hadoop-use-data-lake-storage-gen2.md#create-a-user-assigned-managed-identity)

* [客户管理的密钥磁盘加密](disk-encryption.md)

## <a name="faq"></a>常见问题解答
### <a name="what-happens-if-i-delete-the-managed-identity-after-the-cluster-creation"></a>如果在创建群集后删除托管标识，会发生什么情况？
需要托管标识时，群集会遇到问题。 创建群集后，当前没有办法更新或更改托管标识。 建议确保在群集运行时不删除托管标识。 也可重新创建群集并分配一个新的托管标识。

## <a name="next-steps"></a>后续步骤

* [什么是 Azure 资源的托管标识？](../active-directory/managed-identities-azure-resources/overview.md)
