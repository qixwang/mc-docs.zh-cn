---
author: cherylmc
ms.date: 12/06/2019
ms.service: expressroute
ms.topic: include
ms.author: cherylmc
ms.openlocfilehash: 7babc56f383d7c5833bb89e6987115c6388d4f01
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75859555"
---
安装最新版本的 Azure 服务管理 (SM) PowerShell 模块和 ExpressRoute 模块。 
1. 按照[安装服务管理模块](https://docs.microsoft.com/en-us/powershell/azure/servicemanagement/install-azure-ps)一文中的说明安装 Azure 服务管理模块。 如果已安装 Az 或 RM 模块，请确保使用“-AllowClobber”。
2. 导入已安装的模块。 使用以下示例时，请调整路径以反映已安装的 PowerShell 模块的位置和版本。

   ```powershell
   Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.3.0\Azure.psd1'
   Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.3.0\ExpressRoute\ExpressRoute.psd1'
   ```
3. 若要登录到 Azure 帐户，请使用提升的权限打开 PowerShell 控制台，并连接到帐户。 使用以下示例帮助你通过服务管理模块进行连接：

   ```powershell
   Add-AzureAccount
   ```