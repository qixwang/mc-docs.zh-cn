---
author: rockboyfor
ms.service: container-instances
ms.topic: include
origin.date: 07/22/2020
ms.date: 08/10/2020
ms.testscope: no
ms.testdate: 03/02/2020
ms.author: v-yeche
ms.openlocfilehash: 2f1c48022fc0a9a6519f916c2505fa9488873b41
ms.sourcegitcommit: ac70b12de243a9949bf86b81b2576e595e55b2a6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919356"
---
| 资源 | 限制 |
| --- | :--- |
| 每个区域每个订阅的标准 sku 容器组数 | 100<sup>1</sup> |
| 每个区域每个订阅的专用 sku 容器组数 | 0<sup>1</sup> |
| 每个容器组的容器数 | 60 |
| 每个容器组的卷数 | 20 |
| 每个订阅每个区域的标准 sku 核心数（CPU 数） | 10<sup>1,2</sup> | 
| 每个订阅每个区域的 V100 GPU 的标准 sku 核心数（CPU 数） | 0<sup>1,2</sup> |
| 每个 IP 的端口数 | 5 |
| 容器实例日志大小 - 正在运行的实例 | 4 MB |
| 容器实例日志大小 - 已停止的实例 | 16 KB 或 1,000 行 |
| 每小时创建容器次数 |300<sup>1</sup> |
| 每 5 分钟创建容器次数 | 100<sup>1</sup> |
| 每小时删除容器次数 | 300<sup>1</sup> |
| 每 5 分钟删除容器次数 | 100<sup>1</sup> |

<!--ONLY AVAILABLE ON V100 TO MATCH NCv3-->

<sup>1</sup>要请求提高上限，请创建一个 [Azure 支持请求][azure-support]。 从包括 [Azure免费帐户](https://www.azure.cn/pricing/1rmb-trial-full)的免费订阅到“标准预付费套餐”订阅。<br />
<sup>2</sup> [标准预付费套餐](https://www.azure.cn/pricing/1rmb-trial-full)订阅的默认限制。 其他类别类型的上限可能有所不同。<br/>

<!-- LINKS - External -->

[azure-support]: https://support.azure.cn/support/support-azure/

<!-- Update_Description: update meta properties, wording update, update link -->