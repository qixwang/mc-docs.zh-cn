---
title: 在 Azure 门户中为 Windows VM 创建完全限定的域名
description: 了解如何在 Azure 门户中为基于 Resource Manager 的虚拟机创建完全限定域名或 FQDN。
author: rockboyfor
ms.service: virtual-machines-windows
ms.subservice: networking
ms.topic: article
ms.workload: infrastructure-services
origin.date: 08/15/2018
ms.date: 07/06/2020
ms.author: v-yeche
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a9eab304498a12dc043d367403fdba12152789e2
ms.sourcegitcommit: 89118b7c897e2d731b87e25641dc0c1bf32acbde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85945813"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>在 Azure 门户中为 Windows VM 创建完全限定的域名

在 [Azure 门户](https://portal.azure.cn)中创建虚拟机 (VM) 时，此门户会自动为虚拟机创建公共 IP 资源。 可以使用此 IP 地址远程访问 VM。 虽然门户不会创建[完全限定的域名](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)（简称 FQDN），但可以在创建 VM 后创建一个完全限定的域名。 本文演示了创建 DNS 名称或 FQDN 的步骤。

## <a name="create-a-fqdn"></a>创建 FQDN
阅读本文的前提是已创建 VM。 如果需要，可以[在门户中创建 VM](quick-create-portal.md) 或[通过 Azure PowerShell 创建 VM](quick-create-powershell.md)。 在 VM 正常运行后，请按照以下步骤操作：

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

现在可以使用此 DNS 名称（例如远程桌面协议 (RDP)）远程连接至 VM。

## <a name="next-steps"></a>后续步骤
你的 VM 已经有公共 IP 和 DNS 名称，现在可以部署常用应用程序框架或服务，例如 IIS、SQL 或 SharePoint。

也可以深入了解如何[使用 Resource Manager](../../azure-resource-manager/management/overview.md)，以获取生成 Azure 部署的相关提示。

<!-- Update_Description: update meta properties, wording update, update link -->