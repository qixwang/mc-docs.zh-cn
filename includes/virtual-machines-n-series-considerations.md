---
title: include 文件
description: include 文件
services: virtual-machines-linux
author: rockboyfor
ms.service: virtual-machines-linux
ms.topic: include
origin.date: 06/19/2018
ms.date: 11/11/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: a2dab92c05da4a686d956ec6b09611d8a03222b0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73831467"
---
## <a name="deployment-considerations"></a>部署注意事项

* 有关 N 系列 VM 的可用性，请查看[可用产品(按区域)](https://www.azure.cn/home/features/products-by-region)。

* N 系列 VM 只能按 Resource Manager 部署模型部署。

* N 系列的 VM 在对其磁盘支持的 Azure 存储类型方面有所不同。 NCv3 VM 仅支持高级磁盘存储 (SSD) 所支持的 VM 磁盘。

<!-- Not Available on  NC, NV, NCv2, NCv3, ND, NVv2  -->

* 如果需要部署的 N 系列 VM 较多，请考虑使用标准预付费套餐订阅或其他购买选项。 如果使用的是 [Azure 试用帐户](https://www.azure.cn/pricing/1rmb-trial/)，则仅可使用有限数量的 Azure 计算核心。

* 可能需要提高 Azure 订阅中的核心配额（按区域）以及单独针对 NCv3 核心的配额。 若要请求提高配额，可免费[提出在线客户支持请求](https://support.azure.cn/support/support-azure/)。 默认限制可能因订阅类别而异。

<!-- Not Available on NC, NCv2, ND, or NV -->
<!-- Update_Description: update meta properties, wording update -->