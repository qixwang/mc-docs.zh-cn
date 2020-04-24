---
title: include 文件
description: include 文件
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
origin.date: 09/10/2018
ms.date: 09/26/2018
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 37844d21d5f67a91d81f4a4276b8268eb8812982
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63848067"
---
以下限制适用于 Azure 备份。

| 限制标识符 | 默认限制 |
| --- | --- |
| 可针对每个保管库注册的服务器/计算机数量 |对于 Windows Server/客户端/SCDPM 为 50 个 <br/> 对于 IaaS VM 为 1000 个 |
| 存储在 Azure 保管库存储中的数据的数据源大小 |最大 54400 GB<sup>1</sup> |
| 可在每个 Azure 订阅中创建的备份保管库的数目 |每个区域 500 个恢复服务保管库 |
| 每天计划备份的次数 |对于 Windows Server/客户端为每天 3 个 <br/> 对于 SCDPM 为每天 2 个 <br/> 对于 IaaS Vm 为每天 1 个 |
| 将数据磁盘附加到 Azure 虚拟机进行备份 |16 |
| 附加到 Azure 虚拟机以进行备份的单个数据磁盘的大小| 4095 GB|

* <sup>1</sup>54400 GB 限制不适用于 IaaS VM 备份。
 

<!-- ms.date: 09/26/2018 -->
