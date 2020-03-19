---
title: 生成和导出证书以用于 Azure 虚拟 WAN 用户 VPN 连接 | Azure
description: 在 Windows 10 或 Windows Server 2016 上使用 PowerShell 创建自签名根证书、导出公钥和生成客户端证书。
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: conceptual
origin.date: 10/09/2019
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: b8a3847805bada77bce9730f357511dc8eebe5d4
ms.sourcegitcommit: e94ed1c9eff4e88be2ca389909e60b14cc0d92f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084499"
---
<!--Verified successfully-->
# <a name="generate-and-export-certificates-for-virtual-wan-user-vpn-connections"></a>生成和导出证书以用于虚拟 WAN 用户 VPN 连接

用户 VPN 连接使用证书进行身份验证。 本文介绍如何在 Windows 10 或 Windows Server 2016 上使用 PowerShell 创建自签名根证书并生成客户端证书。

必须在运行 Windows 10 或 Windows Server 2016 的计算机上执行本文中的步骤。 用于生成证书的 PowerShell cmdlet 是操作系统的一部分，在其他版本的 Windows 上不正常工作。 只需 Windows 10 或 Windows Server 2016 计算机即可生成证书。 生成证书后，可上传证书，或在任何支持的客户端操作系统上安装该证书。

[!INCLUDE [Export public key](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="next-steps"></a>后续步骤

继续阅读[用户 VPN 连接的虚拟 WAN 步骤](virtual-wan-about.md)

<!-- Update_Description: new article about certificates point to site -->
<!--NEW.date: 03/09/2020-->