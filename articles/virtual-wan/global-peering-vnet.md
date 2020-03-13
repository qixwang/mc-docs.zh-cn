---
title: 为 Azure 虚拟 WAN 配置全局 VNet 对等互连 | Azure
description: 将不同区域中的 VNet 连接到虚拟 WAN 中心。
services: virtual-wan
author: rockboyfor
ms.service: virtual-wan
ms.topic: conceptual
origin.date: 11/04/2019
ms.date: 03/02/2020
ms.author: v-yeche
ms.openlocfilehash: 26b11c5c4896f689d151e227d5a599e3facbe46f
ms.sourcegitcommit: 69cadf1fa0ed81751c48fbce919a6bb44b1053ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209250"
---
# <a name="configure-global-vnet-peering-cross-region-vnet-for-virtual-wan"></a>为虚拟 WAN 配置全局 VNet 对等互连（跨区域 VNet）

可以将不同区域中的 VNet 连接到虚拟 WAN 中心。

## <a name="before-you-begin"></a>准备阶段

验证是否符合以下条件：

* 跨区域 VNet（辐射）未连接到其他虚拟 WAN 中心。 一个辐射只能连接到一个虚拟中心。
* VNet（辐射）不包含虚拟网络网关（例如，Azure VPN 网关或 ExpressRoute 虚拟网络网关）。 如果 VNet 包含虚拟网络网关，则必须先删除该网关，然后才能将辐射 VNet 连接到中心。

<a name="register"></a>
## <a name="register-this-feature"></a>注册此功能

可以使用 PowerShell 注册此功能。 如果在下面的示例中选择“试用”，则会打开 Azure Cloud Shell，无需在计算机本地安装 PowerShell cmdlet。 如有必要，可以使用 'Select-AzSubscription -SubscriptionId <subid>' cmdlet 更改订阅。

```powershell
Register-AzProviderFeature -FeatureName AllowCortexGlobalVnetPeering -ProviderNamespace Microsoft.Network
Register-AzResourceProvider -ProviderNamespace 'Microsoft.Network'
```

<a name="verify"></a>
## <a name="verify-registration"></a>验证注册

```powershell
Get-AzProviderFeature -FeatureName AllowCortexGlobalVnetPeering -ProviderNamespace Microsoft.Network
```

<a name="hub"></a>
## <a name="connect-a-vnet-to-the-hub"></a>将 VNet 连接到中心

此步骤在中心与跨区域 VNet 之间创建对等互连。 针对要连接的每个 VNet 重复这些步骤。

1. 在虚拟 WAN 的页面上，单击“虚拟网络连接”。 
2. 在虚拟网络连接页上，单击“+添加连接”。 
3. 在“添加连接”页上填写以下字段  ：

    * **连接名称** - 为连接命名。
    * **中心** - 选择要与此连接关联的中心。
    * **订阅** - 验证订阅。
    * **虚拟网络** - 选择要连接到此中心的虚拟网络。 此虚拟网络不能包含现有的虚拟网络网关。
4. 单击“确定”以创建对等互连连接。 

## <a name="next-steps"></a>后续步骤

若要详细了解虚拟 WAN，请参阅[虚拟 WAN 概述](virtual-wan-about.md)。

<!-- Update_Description: new article about global peering vnet -->
<!--NEW.date: 03/02/2020-->