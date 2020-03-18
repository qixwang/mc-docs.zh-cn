---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 03/02/2020
ms.date: ''
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: ae7d4ae97bdbc1d4dec40ad1afdb4a0a1851a74e
ms.sourcegitcommit: 96739f602d9f632e5d465e9f7b67d35b06d3cdbe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209652"
---
Azure 计算提供独立于特定硬件类型并专用于单个客户的虚拟机大小。  这些虚拟机大小非常适合于与其他客户的工作负载（涉及符合性和法规要求等元素）高度隔离的工作负载。  客户还可以选择利用[对嵌套虚拟机的 Azure 支持](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)，对这些独立的虚拟机资源进一步细分。

使用独立大小可保证你的虚拟机将是在特定服务器实例上唯一运行的虚拟机。  当前的独立虚拟机产品/服务包括：
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms

    <!--Not Available on * Standard_GS5-->
    <!--Not Available on * Standard_G5-->
    <!--Not Available on * Standard_DS15_v2-->
    
* Standard_D15_v2
* Standard_F72s_v2

可以在[此处](/virtual-machines/windows/sizes-memory)详细了解每个可用的独立大小。

<!--Not Available on /DS15_v2-->

## <a name="retiring-d15_v2ds15_v2-isolation-on-may-15-2020"></a>D15_v2/DS15_v2 隔离将在 2020 年 5 月 15 日停用
**2020 年 2 月 10 日更新：“隔离”停用时间线已延长到 2020 年 5 月 15 日**

<!--Not Available on Azure Dedicated Host is now GA, which allows you to run your organization's Linux and Windows virtual machines on single-tenant physical servers. We plan to fully replace isolated Azure VMs with Azure Dedicated Host.-->

在 **2020 年 5 月 15 日**之后，D15_v2/DS15_v2 Azure VM 将不再是硬件隔离的 VM。
 
<!--Not Available on /DS15_v2-->

## <a name="how-does-this-affect-me"></a>这对我有何影响？
在 2020 年 5 月 15 日之后，我们将不再为你的 D15_v2/DS15_v2 Azure 虚拟机提供隔离保证。 

## <a name="what-actions-should-i-take"></a>我应该采取什么措施？
如果对你而言硬件隔离不是必需的，则无需采取任何措施。 

如果对你而言隔离是必需的，则你需要在 2020 年 5 月 15 日之前执行以下任一操作：

•   将你的工作负荷[迁移](https://azure.microsoft.com/blog/introducing-azure-dedicated-host)到 Azure 专用主机。

•   [请求访问](https://aka.ms/D15iRequestAccess) D15i_v2 和 DS15i_v2 Azure VM 以获得相同的性价比。 此选项仅适用于标准预付款套餐和一年期的预留实例方案。    

•   将你的工作负荷[迁移](https://azure.microsoft.com/blog/resize-virtual-machines/)到另一台 Azure 独立虚拟机。 

有关详细信息，请参阅下文：

## <a name="timeline"></a>时间线
| 日期 | 操作 | 
| --- | --- |
| 2019 年 11 月 18 日 | D/DS15i_v2（PAYG，1 年期 RI）的可用性 |
| 2020 年 5 月 14 日  | 可以购买 D/DS15i_v2 1 年期 RI 的最后一天 | 
| 2020 年 5 月 15 日   | D/DS15_v2 隔离保证已取消 | 
| 2021年 5 月 15日  | 停用 D/DS15i_v2（在 2019 年 11 月 18 日之前购买了 D/DS15_v2 的 3 年期 RI 的所有客户除外）| 
| 2022 年 11 月 17 日  | 在 3 年期 RI 结束后停用 D/DS15i_v2（适用于在 2019 年 11 月 18 日之前购买了 D/DS15_v2 的 3 年期 RI 的客户） | 

## <a name="faq"></a>常见问题
### <a name="q-is-the-size-dds15_v2-going-to-get-retired"></a>问：D/DS15_v2 大小是否将被停用？
**答**：否，只有“隔离”功能将被停用。 如果你不需要隔离，则无需采取任何措施。

<!--Not Available RI-->

### <a name="q-is-the-size-dds15i_v2-going-to-get-retired"></a>问：D/DS15i_v2 大小是否将被停用？
**答**：是的，该大小仅在 2021 年 5 月 15 日之前可用。 在 2019 年 11 月 18 日之前购买了 D/DS15_v2 的 3 年期 RI 的客户在 2022 年 11 月 17 日之前一直可以访问 D/DS15i_v2。

<!--Not Available RI, D/DS15i_v2 -->

### <a name="q-why-am-i-not-seeing-the-new-dds15i_v2-sizes-in-the-portal"></a>问：为什么在门户中看不到新的 D/DS15i_v2 大小？
**答**：如果你是当前的 D/DS15_v2 的客户，并且想要使用新的 D/DS15i_v2 大小，请填写此[表单](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0FTPNXHdWpJlO27GE-bHitUMkZUWEFPNjFPNVgyMkhZS05FSzlPTzRIOS4u)

### <a name="q-why-i-am-not-seeing-any-quota-for-the-new-dds15i_v2-sizes"></a>问：为什么看不到新的 D/DS15i_v2 大小的任何配额？
**答**：如果你是当前的 D/DS15_v2 的客户，并且想要使用新的 D/DS15i_v2 大小，请填写此[表单](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0FTPNXHdWpJlO27GE-bHitUNU1XUkhZWkNXQUFMNEJWUk9VWkRRVUJPMy4u)

### <a name="q-when-are-the-other-isolated-sizes-going-to-retire"></a>问：其他独立大小将于何时停用？
**答**：在官方正式停用这些大小之前，我们将提前 12 个月进行提醒。

### <a name="q-is-there-a-downtime-when-my-vm-lands-on-a-non-isolated-hardware"></a>问：当我的虚拟机落脚于非隔离的硬件上时，是否会出现停机？
**答**：如果你不需要隔离，则无需采取任何措施，并且你不会看到停机。

### <a name="q-are-there-any-cost-changes-for-moving-to-a-non-isolated-virtual-machine"></a>问：移动到非独立虚拟机是否有任何成本变化？
**答**：否 

### <a name="q-i-already-purchased-1--or-3-year-reserved-instance-for-d15_v2-or-ds15_v2-how-will-the-discount-be-applied-to-my-vm-usage"></a>问：我已购买了 D15_v2 或 Ds15_v2 的 1-年期或 3 年期预留实例。 折扣将如何应用于我的 VM 使用情况？
**答**：在 2019 年 11 月 18 日之前购买的 RI 会自动将覆盖范围扩展到新的独立 VM 系列。 

| RI |  实例大小灵活性 | 权益资格 |   
| --- | --- | --- |
|   D15_v2  |   关闭     |   D15_v2 和 D15i_v2 |    
|   D15_v2  |   启用  |   D15_v2 系列和 D15i_v2 将全部享受 RI 权益。 |    
|   D14_v2  |   启用  |   D15_v2 系列和 D15i_v2 将全部享受 RI 权益。 |    

对于 Dsv2 系列也是如此。

### <a name="q-i-want-to-purchase-additional-reserved-instances-for-dv2-which-one-should-i-choose"></a>问：我想为 Dv2 购买额外的预留实例。 我应该选择哪一种？
**答**：在 2019 年 11 月 18 日之后购买的所有 RI 都具有以下行为。 

| RI |  实例大小灵活性 | 权益资格 |   
| --- | --- | --- |
| D15_v2 |  关闭 |   仅限 D15_v2  
| D15_v2 |  启用 |    D15_v2 系列将享受 RI 权益。 新的 D15i_v2 将没有资格享受此 RI 类型的 RI 权益。 | 
| D15i_v2 |     关闭 | 仅限 D15i_v2 |  
| D15i_v2 |     启用  | 仅限 D15i_v2 | 

实例大小灵活性不能应用于任何其他大小，例如 D2_v2、D4_v2 或 D15_v2。 对于 Dsv2 系列也是如此。  

### <a name="q-can-i-buy-a-new-3-year-ri-for-d15i_v2-and-ds15i_v2"></a>问：是否可以为 D15i_v2 和 DS15i_v2 购买新的 3 年期 RI？
**答**：很遗憾，不可以，进行新购买时只有 1 年期 RI 可供选择。

### <a name="q-can-i-move-my-existing-d15_v2ds15_v2-reserve-instance-to-an-isolated-size-reserved-instance"></a>问：是否可以将现有的 D15_v2/DS15_v2 预留实例移动到独立大小预留实例？
**答**：此操作不是必需的，因为此权益将同时适用于独立大小和非独立大小。 但 Azure 支持将现有 D15_v2/DS15_v2 预留实例更改为 D15i_v2/DS15i_v2。 对于所有其他 Dv2/Dsv2 预留实例，请为独立大小使用现有的预留实例或购买新的预留实例。

### <a name="q-im-an-azure-service-fabric-customer-relying-on-the-silver-or-gold-durability-tiers-does-this-change-impact-me"></a>问：我是依赖于白银或黄金耐久性层级的 Azure Service Fabric 客户。 此更改是否会影响我？
**答**：不会。 Service Fabric 的[耐久性层级](https://docs.azure.cn/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster)提供的保证即使在此更改发生后也将继续履行。 如果你出于其他原因而需要物理硬件隔离，可能仍需采取上述措施之一。 

<!-- Update_Description: new article about virtual machines common isolation -->
<!--NEW.date: 03/02/2020-->