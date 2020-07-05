---
title: 虚拟机 vCPU 配额
description: 了解 Azure 虚拟机的 vCPU 配额。
author: rockboyfor
ms.service: virtual-machines
ms.subservice: sizes
ms.workload: infrastructure-services
ms.topic: article
origin.date: 05/31/2018
ms.date: 07/06/2020
ms.author: v-yeche
ms.openlocfilehash: bdcaf68dd5f021a10290ab8157544a8087f191c0
ms.sourcegitcommit: 89118b7c897e2d731b87e25641dc0c1bf32acbde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85945747"
---
# <a name="virtual-machine-vcpu-quotas"></a>虚拟机 vCPU 配额

虚拟机和虚拟机规模集的 vCPU 配额已根据每个区域中的每个订阅划分成两层。 第一层是区域的 vCPU 总数，第二层是各种 VM 大小系列核心（如 D 系列 vCPU）。 每当部署新 VM 时，VM 的 vCPU 数不能超过 VM 大小系列的 vCPU 配额或区域 vCPU 配额总数。 如果超过了上述任一配额，将不允许部署 VM。 此外，区域中的虚拟机总数也有一个配额。 可以在 [Azure 门户](https://portal.azure.cn)的“订阅”页的“用量 + 配额”部分中查看其中每项配额的详细信息，或者，可以使用 PowerShell 查询这些值。 

> [!NOTE]
> 配额基于所使用的核心总数（已分配和已解除分配）进行计算。 如果需要额外核心，则[请求增加配额](https://support.azure.cn/support/support-azure/)或删除不再需要的 VM。 

## <a name="check-usage"></a>检查使用情况

可以使用 [Get-AzVMUsage](https://docs.microsoft.com/powershell/module/az.compute/get-azvmusage) cmdlet 来检查配额的使用情况。

```powershell
Get-AzVMUsage -Location "China East"
```

输出类似于以下内容：

```
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional vCPUs                         4   260 Count
Virtual Machines                             4 10000 Count
Virtual Machine Scale Sets                   1  2000 Count
Standard B Family vCPUs                      1    10 Count
Standard DSv2 Family vCPUs                   1   100 Count
Standard Dv2 Family vCPUs                    2   100 Count
Basic A Family vCPUs                         0   100 Count
Standard A0-A7 Family vCPUs                  0   250 Count
Standard D Family vCPUs                      0   100 Count
Standard DS Family vCPUs                     0   100 Count
Standard F Family vCPUs                      0   100 Count
Standard FS Family vCPUs                     0   100 Count
Standard Av2 Family vCPUs                    0   100 Count
Standard Dv2 Promo Family vCPUs              0   100 Count
Standard DSv2 Promo Family vCPUs             0   100 Count
Standard Dv3 Family vCPUs                    0   100 Count
Standard DSv3 Family vCPUs                   0   100 Count
Standard Ev3 Family vCPUs                    0   100 Count
Standard ESv3 Family vCPUs                   0   100 Count
Standard FSv2 Family vCPUs                   0   100 Count
Standard NCv3 Family vCPUs                   0     0 Count
Standard Storage Managed Disks               2 10000 Count
Premium Storage Managed Disks                1 10000 Count
```

<!-- Not Available on Standard A8-A11 -->
<!-- Not Available on Standard G, GS -->
<!-- Not Available on Standard NV, NC -->
<!-- Not Available on Standard H, MS -->
<!-- Not Available on Standard LS, LSv2 -->
<!-- Not Available on Standard ND, NCV2 -->

<!--Not Available on ## Reserved VM Instances-->

## <a name="next-steps"></a>后续步骤

有关计费和配额的详细信息，请参阅 [Azure 订阅和服务限制、配额与约束](/azure-resource-manager/management/azure-subscription-service-limits?toc=/billing/TOC.json)。

<!-- Update_Description: update meta properties, wording update, update link -->