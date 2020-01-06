---
title: SKU 不可用错误
description: 介绍在使用 Azure 资源管理器部署资源时如何解决 SKU 不可用错误。
ms.topic: troubleshooting
origin.date: 10/19/2018
ms.author: v-yeche
ms.date: 01/06/2020
ms.openlocfilehash: dc4a42ae2b08bbc9ef520e7c4618bfa92f3f748c
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75631357"
---
# <a name="resolve-errors-for-sku-not-available"></a>解决 SKU 不可用错误

本文介绍如何解决 **SkuNotAvailable** 错误。 如果在该区域或满足业务需求的备用区域中找不到合适的 SKU，请将 [SKU 请求](https://support.azure.cn/support/support-azure/)提交到 Azure 支持。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="symptom"></a>症状

部署资源（通常为虚拟机）时，会收到以下错误代码和错误消息：

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>原因

当所选的资源 SKU（如 VM 大小）不可用于所选的位置时，会收到此错误。

<!--Not Available on [Spot error messages](../../virtual-machines/error-codes-spot.md)-->

## <a name="solution-1---powershell"></a>解决方案 1 - PowerShell

要确定区域中可用的 SKU，请使用 [Get-AzComputeResourceSku](https://docs.microsoft.com/powershell/module/az.compute/get-azcomputeresourcesku) 命令。 按位置对结果进行筛选。 必须安装最新版本 PowerShell 才能运行此命令。

```powershell
Get-AzComputeResourceSku | where {$_.Locations -icontains "chinaeast"}
```

结果包括位置的 SKU 列表以及针对该 SKU 的任何限制。 请注意，SKU 可能被列为 `NotAvailableForSubscription`。

```powershell
ResourceType          Name        Locations   Restriction                      Capability           Value
------------          ----        ---------   -----------                      ----------           -----
virtualMachines       Standard_A0 chinaeast   NotAvailableForSubscription      MaxResourceVolumeMB   20480
virtualMachines       Standard_A1 chinaeast   NotAvailableForSubscription      MaxResourceVolumeMB   71680
virtualMachines       Standard_A2 chinaeast   NotAvailableForSubscription      MaxResourceVolumeMB  138240
```

## <a name="solution-2---azure-cli"></a>解决方案 2 - Azure CLI

要确定区域中可用的 SKU，请使用 `az vm list-skus` 命令。 使用 `--location` 参数将输出筛选到正在使用的位置。 使用 `--size` 参数按部分大小名称搜索。

```azurecli
az vm list-skus --location chinaeast --size Standard_F --output table
```

该命令将返回类似下面的结果：

```azurecli
ResourceType     Locations       Name              Zones    Capabilities    Restrictions
---------------  --------------  ----------------  -------  --------------  --------------
virtualMachines  chinaeast  Standard_F1                ...             None
virtualMachines  chinaeast  Standard_F2                ...             None
virtualMachines  chinaeast  Standard_F4                ...             None
...
```

## <a name="solution-3---azure-portal"></a>解决方案 3 - Azure 门户

要确定区域中可用的 SKU，请使用[门户](https://portal.azure.cn)。 登录到门户，并通过接口添加资源。 设置值时，可看到该资源的可用 SKU。 不需要完成部署。

例如，开始创建虚拟机的过程。 若要查看其他可用的大小，请选择“更改大小”  。

![创建 VM](./media/error-sku-not-available/create-vm.png)

可以筛选和滚动到可用的大小。

![可用的 SKU](./media/error-sku-not-available/available-sizes.png)

## <a name="solution-4---rest"></a>解决方案 4 - REST

要确定区域中可用的 SKU，请使用[资源 Sku - 列表](https://docs.microsoft.com/rest/api/compute/resourceskus/list)操作。

它会用以下格式返回可用的 SKU 和区域：

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "chinaeast"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "chinaeast"
      ],
      "restrictions": []
    },
    ...
  ]
}
```

<!-- Update_Description: update meta properties, wording update, update link -->