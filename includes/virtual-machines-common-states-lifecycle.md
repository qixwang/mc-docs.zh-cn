---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 08/09/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 58e71e55799c12966b92fd3ff39fa213e6448f2a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63848249"
---
Azure 虚拟机 (VM) 经历的不同状态可以归类为“预配”状态和“电源”状态。   本文旨在介绍这些状态并专门突出显示了何时会对客户收取实例使用费用。 

## <a name="power-states"></a>电源状态

电源状态表示 VM 的上一个已知状态。

![VM 电源状态图](./media/virtual-machines-common-states-lifecycle/vm-power-states.png)

<br>
下表描述每个实例状态并指示是否会对其收取实例使用费用。

<table>
<tr>
<th>
状态
</th>
<th>
说明
</th>
<th>
实例使用计费
</th>
</tr>
<tr>
<td>
<p><b>正在启动</b></p>
</td>
<td>
<p>VM 正在启动。</p>
<code>&quot;statuses&quot;: [<br>
   {<br>
      &quot;code&quot;: &quot;PowerState/starting&quot;,<br>
       &quot;level&quot;: &quot;Info&quot;,<br>
        &quot;displayStatus&quot;: &quot;VM starting&quot;<br>
    }<br>
    ]</code><br>
</td>
<td>
<p><b>不计费</b></p>
</td>
</tr>
<tr>
<td>
<p><b>正在运行</b></p>
</td>
<td>
<p>VM 的正常工作状态</p>
<code>&quot;statuses&quot;: [<br>
 {<br>
 &quot;code&quot;: &quot;PowerState/running&quot;,<br>
 &quot;level&quot;: &quot;Info&quot;,<br>
 &quot;displayStatus&quot;: &quot;VM running&quot;<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>计费</b></p>
</td>
</tr>
<tr>
<td>
<p><b>正在停止</b></p>
</td>
<td>
<p>这是一种过渡性状态。 完成后，会显示为“已停止”。</p>
<code>&quot;statuses&quot;: [<br>
 {<br>
 &quot;code&quot;: &quot;PowerState/stopping&quot;,<br>
 &quot;level&quot;: &quot;Info&quot;,<br>
 &quot;displayStatus&quot;: &quot;VM stopping&quot;<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>计费</b></p>
</td>
</tr>
<tr>
<td>
<p><b>已停止</b></p>
</td>
<td>
<p>VM 已在来宾 OS 中关闭，或者已使用 PowerOff API 关闭。</p>
<p>硬件仍然分配给 VM 并保留在主机上。 </p>
<code>&quot;statuses&quot;: [<br>
 {<br>
 &quot;code&quot;: &quot;PowerState/stopped&quot;,<br>
 &quot;level&quot;: &quot;Info&quot;,<br>
 &quot;displayStatus&quot;: &quot;VM stopped&quot;<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>不计费&#42;</b></p>
</td>
</tr>
<tr>
<td>
<p><b>正在解除分配</b></p>
</td>
<td>
<p>过渡性状态。 完成后，VM 会显示为“已解除分配”。</p>
<code>&quot;statuses&quot;: [<br>
 {<br>
 &quot;code&quot;: &quot;PowerState/deallocating&quot;,<br>
 &quot;level&quot;: &quot;Info&quot;,<br>
 &quot;displayStatus&quot;: &quot;VM deallocating&quot;<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>不计费&#42;</b></p>
</td>
</tr>
<tr>
<td>
<p><b>已解除分配</b></p>
</td>
<td>
<p>VM 已成功停止并从主机中删除。 </p>
<code>&quot;statuses&quot;: [<br>
 {<br>
 &quot;code&quot;: &quot;PowerState/deallocated&quot;,<br>
 &quot;level&quot;: &quot;Info&quot;,<br>
 &quot;displayStatus&quot;: &quot;VM deallocated&quot;<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>不计费</b></p>
</td>
</tr>
</tbody>
</table>

&#42;某些 Azure 资源（例如磁盘和网络）不管实例状态如何均收费。 

## <a name="provisioning-states"></a>预配状态

预配状态是用户在 VM 上启动的控制平面操作的状态。 以下状态独立于 VM 的电源状态。

- **创建** - 创建 VM。

- **更新** - 更新现有 VM 的模型。 某些针对 VM 的非模型更改（例如“启动/重启”）也属于更新。

- **删除** - 删除 VM。

- **解除分配** - 是指停止 VM 并将其从主机中删除。 可以将 VM 的解除分配视为一种更新，因此它会显示与更新相关的预配状态。

下面是在平台接受用户启动的操作之后的过渡操作状态：

<br>

<table>
<tbody>
<tr>
<td width="162">
<p><b>状态</b></p>
</td>
<td width="366">
<p>说明</p>
</td>
</tr>
<tr>
<td width="162">
<p><b>正在创建</b></p>
</td>
<td width="366">
<code>&quot;statuses&quot;: [<br>
 {<br>
 &quot;code&quot;: &quot;ProvisioningState/creating&quot;,<br>
 &quot;level&quot;: &quot;Info&quot;,<br>
 &quot;displayStatus&quot;: &quot;Creating&quot;<br>
 }</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>正在更新</b></p>
</td>
<td width="366">
<code>&quot;statuses&quot;: [<br>
 {<br>
 &quot;code&quot;: &quot;ProvisioningState/updating&quot;,<br>
 &quot;level&quot;: &quot;Info&quot;,<br>
 &quot;displayStatus&quot;: &quot;Updating&quot;<br>
 }<br>
 ]</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>正在删除</b></p>
</td>
<td width="366">
<code>&quot;statuses&quot;: [<br>
 {<br>
 &quot;code&quot;: &quot;ProvisioningState/deleting&quot;,<br>
 &quot;level&quot;: &quot;Info&quot;,<br>
 &quot;displayStatus&quot;: &quot;Deleting&quot;<br>
 }<br>
 ]</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>OS 预配状态</b></p>
</td>
<td width="366">
<p>如果 VM 是使用 OS 映像而非专用映像创建的，可能会观察到以下子状态：</p>
<p>1. <b>OSProvisioningInprogress</b> &ndash; VM 正在运行，来宾 OS 的安装正在进行。 <p /> 
<code> &quot;statuses&quot;: [<br>
 {<br>
 &quot;code&quot;: &quot;ProvisioningState/creating/OSProvisioningInprogress&quot;,<br>
 &quot;level&quot;: &quot;Info&quot;,<br>
 &quot;displayStatus&quot;: &quot;OS Provisioning In progress&quot;<br>
 }<br>
]</code><br>
<p>2. <b>OSProvisioningComplete</b> &ndash; 短时状态。 VM 会快速过渡到“成功”状态，除非需要安装扩展。 安装扩展可能需要一定的时间。 <br />
<code> &quot;statuses&quot;: [<br>
 {<br>
 &quot;code&quot;: &quot;ProvisioningState/creating/OSProvisioningComplete&quot;,<br>
 &quot;level&quot;: &quot;Info&quot;,<br>
 &quot;displayStatus&quot;: &quot;OS Provisioning Complete&quot;<br>
 }<br>
]</code><br>
<p><b>注意</b>：如果存在 OS 故障或者 OS 没有及时安装，则 OS 预配可能会过渡到“失败”状态。 会根据部署在基础结构上的 VM 对客户收费。</p>
</td>
</tr>
</table>

操作完成后，VM 会过渡到下述某个状态：

- **成功** - 用户启动的操作已完成。

    ```
  "statuses": [ 
  {
     "code": "ProvisioningState/succeeded",
     "level": "Info",
     "displayStatus": "Provisioning succeeded",
     "time": "time"
  }
  ]
    ```

- **失败** - 表示操作失败。 若要获取详细信息和可能的解决方案，请查看错误代码。

    ```
  "statuses": [
    {
      "code": "ProvisioningState/failed/InternalOperationError",
      "level": "Error",
      "displayStatus": "Provisioning failed",
      "message": "Operation abandoned due to internal error. Please try again later.",
      "time": "time"
    }
    ]
    ```

## <a name="vm-instance-view"></a>VM 实例视图

实例视图 API 提供 VM 运行状态信息。 有关详细信息，请参阅 [Virtual Machines - Instance View](https://docs.microsoft.com/rest/api/compute/virtualmachines/instanceview)（虚拟机 - 实例视图）API 文档。

<!-- Not Available on [Resource Explorer] (https://resources.azure.com/)-->

预配状态在 VM 属性和实例视图中可见。 电源状态在 VM 的实例视图中提供。

<!-- Update_Description: update meta properties, wording update -->