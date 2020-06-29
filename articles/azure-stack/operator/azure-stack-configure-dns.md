---
title: 在 Azure Stack Hub 中更新 DNS 转发器
description: 了解如何在 Azure Stack Hub 中更新 DNS 转发器。
author: WenJason
ms.topic: conceptual
origin.date: 11/21/2019
ms.date: 06/22/2020
ms.author: v-jay
ms.reviewer: thoroet
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: e0a23e72bc3680037f89261841fa9b4fb9195103
ms.sourcegitcommit: d86e169edf5affd28a1c1a4476d72b01a7fb421d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85096298"
---
# <a name="update-the-dns-forwarder-in-azure-stack-hub"></a>在 Azure Stack Hub 中更新 DNS 转发器

Azure Stack Hub 基础结构至少需要一个可访问的 DNS 转发器来解析外部名称。 必须提供 DNS 转发器才能部署 Azure Stack Hub。 该输入在 Azure Stack Hub 内部 DNS 服务器中用作转发器，并为身份验证、市场管理或使用情况等服务启用外部名称解析。

DNS 是一项可更改的关键数据中心基础结构服务。 如果 DNS 更改，则必须更新 Azure Stack Hub。

本文介绍如何使用特权终结点 (PEP) 在 Azure Stack Hub 中更新 DNS 转发器。 建议使用两个可靠的 DNS 转发器 IP 地址。

## <a name="steps-to-update-the-dns-forwarder"></a>更新 DNS 转发器的步骤

1. 连接到[特权终结点](azure-stack-privileged-endpoint.md)。 不需开具支持票证来解锁特权终结点。

2. 运行以下命令，查看当前配置的 DNS 转发器。 或者，也可以使用管理员门户区域属性：

   ```powershell
   Get-AzsDnsForwarder
   ```

3. 运行以下命令更新 Azure Stack Hub，以使用新的 DNS 转发器：

   ```powershell
    Set-AzsDnsForwarder -IPAddress "IPAddress 1","IPAddress 2"
   ```

4. 请查看命令输出中是否有错误。

## <a name="next-steps"></a>后续步骤

[防火墙集成](azure-stack-firewall.md)
