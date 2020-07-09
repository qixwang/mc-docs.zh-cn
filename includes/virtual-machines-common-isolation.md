---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 09/18/2019
ms.date: 07/06/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: d3db846b9198a346d1c7aa1d60fe7fedd8aff50f
ms.sourcegitcommit: 89118b7c897e2d731b87e25641dc0c1bf32acbde
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85945774"
---
Azure 计算提供独立于特定硬件类型并专用于单个客户的虚拟机大小。  这些虚拟机大小非常适合于与其他客户的工作负载（涉及符合性和法规要求等元素）高度隔离的工作负载。  客户还可以选择利用[对嵌套虚拟机的 Azure 支持](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)，对这些独立的虚拟机资源进一步细分。

<a name="vm-isolation-size"></a>

使用独立大小可保证你的虚拟机将是在特定服务器实例上唯一运行的虚拟机。  当前的独立虚拟机产品/服务包括：
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms

    <!--Not Available on * Standard_GS5-->
    <!--Not Available on * Standard_G5-->
    
* Standard_DS15_v2 <sup>*</sup>
* Standard_D15_v2 <sup>*</sup>
* Standard_F72s_v2

<sup>*</sup>隔离保证已于 2020 年 5 月 15 日停用

## <a name="retiring-d15_v2ds15_v2-isolation-on-may-15-2020"></a>D15_v2/DS15_v2 隔离将在 2020 年 5 月 15 日停用
**2020 年 2 月 10 日更新：“隔离”停用时间线已延长到 2020 年 5 月 15 日**

Azure 专用主机现在是 GA，使你可在单租户物理服务器上运行组织的 Linux 和 Windows 虚拟机。 我们计划用 Azure 专用主机完全替换隔离的 Azure VM。 在 **2020 年 5 月 15 日**之后，D15_v2/DS15_v2 Azure VM 将不再是硬件隔离的 VM。

## <a name="how-does-this-affect-me"></a>这对我有何影响？
在 2020 年 5 月 15 日之后，我们将不再为你的 D15_v2/DS15_v2 Azure 虚拟机提供隔离保证。 

## <a name="what-actions-should-i-take"></a>我应该采取什么措施？
如果对你而言硬件隔离不是必需的，则无需采取任何措施。 

如果对你而言隔离是必需的，则你需要在 2020 年 5 月 15 日之前执行以下任一操作：

• 将你的工作负载[迁移](https://azure.microsoft.com/blog/introducing-azure-dedicated-host)到 Azure 专用主机。

<!--Not Available on (RESERVED INSTANCE SCENARIOS) • [Request access](https://aka.ms/D15iRequestAccess) to a D15i_v2 and DS15i_v2 Azure VM, to get the same price performance. This option is only available for Standard Pay-in-Advance Offer and one-year reserved instance scenarios.-->

• 将你的工作负载[迁移](https://azure.microsoft.com/blog/resize-virtual-machines/)到另一台 Azure 独立虚拟机。 

<!--Not Available on ## Timeline-->

## <a name="faq"></a>常见问题
### <a name="q-is-the-size-dds15_v2-going-to-get-retired"></a>问：D/DS15_v2 大小是否将被停用？
**答**：否，只有“隔离”功能将被停用。 如果你不需要隔离，则无需采取任何措施。

<!--Not Available RI CONTENT AND Q&A-->
<!--Not Available ### Q: Is the size D/DS15i_v2 going to get retired?-->
<!--Not Available ### Q: Why am I not seeing the new D/DS15i_v2 sizes in the portal?-->
<!--Not Available ### Q: Why I am not seeing any quota for the new D/DS15i_v2 sizes?-->

### <a name="q-when-are-the-other-isolated-sizes-going-to-retire"></a>问：其他独立大小将于何时停用？
**答**：在官方正式停用这些大小之前，我们将提前 12 个月进行提醒。

### <a name="q-is-there-a-downtime-when-my-vm-lands-on-a-non-isolated-hardware"></a>问：当我的虚拟机落脚于非隔离的硬件上时，是否会出现停机？
**答**：如果你不需要隔离，则无需采取任何措施，并且你不会看到停机。

### <a name="q-are-there-any-cost-changes-for-moving-to-a-non-isolated-virtual-machine"></a>问：移动到非独立虚拟机是否有任何成本变化？
**答**：否 

<!--Not Available ### Q: I already purchased 1- or 3-year Reserved Instance for D15_v2 or Ds15_v2. How will the discount be applied to my VM usage?-->
<!--Not Available ### Q: I want to purchase additional Reserved Instances for Dv2. Which one should I choose?-->
<!--Not Available ### Q: Can I buy a new 3-year RI for D15i_v2 and DS15i_v2?-->
<!--Not Available ### Q: Can I move my existing D15_v2/DS15_v2 Reserve Instance to an isolated size Reserved Instance?-->

### <a name="q-im-an-azure-service-fabric-customer-relying-on-the-silver-or-gold-durability-tiers-does-this-change-impact-me"></a>问：我是依赖于白银或黄金耐久性层级的 Azure Service Fabric 客户。 此更改是否会影响我？
**答**：否。 Service Fabric 的[耐久性层级](https://docs.azure.cn/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster)提供的保证即使在此更改发生后也将继续履行。 如果你出于其他原因而需要物理硬件隔离，可能仍需采取上述措施之一。 

<!-- Update_Description: update meta properties, wording update, update link -->