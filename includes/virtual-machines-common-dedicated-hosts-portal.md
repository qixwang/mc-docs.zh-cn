---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
ms.date: 05/25/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: e685f60296f4bbd34813fefe0e954d849ab1761c
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801837"
---
<!--Verified on Portal-->
## <a name="limitations"></a>限制

- 专用主机上目前不支持虚拟机规模集。
- 专用主机可用的大小和硬件类型因区域而异。 请参阅主机[定价页](https://www.azure.cn/pricing/details/virtual-machines/)来了解详细信息。

<!--MOONCAKE CUSTOMIZATION ON 05/22/2020-->

## <a name="create-a-host-group"></a>创建主机组

**主机组**是表示专用主机集合的新资源。 你在某个区域中创建主机组，并向其中添加主机。 规划高可用性时，可以为专用主机进行以下选择： 

<!--Not Available on there are additional options.-->
<!--Not Available on one or both of-->
<!--Not Available on and an availability zone-->

<!--Not Available on - Span across multiple availability zones.-->

- 跨映射到物理机架的多个容错域。 
 
在这种情况下，你需要为主机组提供容错域计数。

<!--Not Available on  If you do not want to span fault domains in your group, use a fault domain count of 1.-->
<!--Not Available on You can also decide to use both availability zones and fault domains. -->

在此示例中，我们将创建使用 2 个容错域的主机组。 

<!--CORRECT TO REMOVE 1 availability zone and-->


1. 打开 [Azure 门户](https://portal.azure.cn)。
1. 选择左上角的“创建资源”。
1. 搜索“主机组”，然后从结果中选择“主机组”。
1. 在“主机组”页中，选择“创建”。
1. 选择要使用的订阅，然后选择“新建”以创建新的资源组。
1. 键入“myDedicatedHostsRG”作为“名称”，然后选择“确定”。
1. 对于“主机组名称”，请键入“myHostGroup”。
1. 对于“位置”，请选择“中国东部”。 
    
    <!--Not Available on 1. For **Availability Zone**, select **1**.-->
    
1. 选择 **2** 作为“容错域计数”。
1. 选择“查看 + 创建”，然后等待验证。
1. 看到“验证通过”消息后，选择“创建”以创建主机组。 

应当只需很短时间便可创建主机组。

<!--MOONCAKE CUSTOMIZATION ON 05/22/2020-->

## <a name="create-a-dedicated-host"></a>创建专用主机

现在，在主机组中创建一个专用主机。 除了主机名称外，还需要提供主机的 SKU。 主机 SKU 捕获受支持的 VM 系列以及专用主机的硬件代系。

有关主机 SKU 和定价的详细信息，请参阅 [Azure 专用主机定价](https://www.azure.cn/pricing/details/virtual-machines/)。

如果为主机组设置了容错域计数，则系统会要求你为主机指定容错域。  

1. 选择左上角的“创建资源”。
1. 搜索“专用主机”，然后从结果中选择“专用主机”。
1. 在“专用主机”页中，选择“创建”。
1. 选择要使用的订阅。
1. 选择“myDedicatedHostsRG”作为“资源组”。
1. 在“实例详细信息”中，键入“myHost”作为“名称”，并选择“中国东部”作为位置。
1. 在“硬件配置文件”中，对于“大小系列”，请选择“标准 Es3 系列 - 类型 1”；对于“主机组”，请选择“myHostGroup”；对于“容错域”，请选择 1。 至于其余字段，请保留默认值。
1. 完成后，选择“查看 + 创建”，然后等待验证。
1. 看到“验证通过”消息后，选择“创建”以创建主机。 


