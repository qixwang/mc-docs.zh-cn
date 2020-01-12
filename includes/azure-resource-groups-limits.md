---
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: include
origin.date: 08/19/2019
ms.date: 01/06/2020
ms.author: v-yeche
ms.openlocfilehash: 4ecd38518cf8102b0bf86dcbe96b1fdd55b73231
ms.sourcegitcommit: bc5f8b4f8ccd7c723f64055825508d1dfcc2162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2020
ms.locfileid: "75865141"
---
| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 每个[资源组](../articles/azure-resource-manager/management/overview.md#resource-groups)的资源数（按资源类型） |800 |某些资源类型可能超过 800 的限制。 请参阅[不限于每个资源组 800 个实例的资源](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md)。 |
| 部署历史记录中每个资源组的部署数 |800<sup>1</sup> |800 |
| 每个部署的资源数 |800 |800 |
| 管理锁数（按唯一的作用域） |20 个 |20 个 |
| 标记数（按资源或资源组） |50 |50 |
| 标记键长度 |512 |512 |
| 标记值长度 |256 |256 |

<sup>1</sup>如果达到每个资源组的部署数限制 800，则会从历史记录中删除不再需要的部署。 从部署历史记录中删除条目不会影响已部署的资源。 有关详细信息，请参阅[解决部署计数超出 800 的错误](../articles/azure-resource-manager/templates/deployment-quota-exceeded.md)。

#### <a name="template-limits"></a>模板限制

| Value | 默认限制 | 最大限制 |
| --- | --- | --- |
| parameters |256 |256 |
| 变量 |256 |256 |
| 资源（包括副本计数） |800 |800 |
| Outputs |64 |64 |
| 模板表达式 |24,576 个字符 |24,576 个字符 |
| 已导出模板中的资源 |200 |200 | 
| 模板大小 |4 MB |4 MB |
| 参数文件大小 |64 KB |64 KB |

通过使用嵌套模板，可超出某些模板限制。 有关详细信息，请参阅[部署 Azure 资源时使用链接的模板](../articles/azure-resource-manager/templates/linked-templates.md)。 若要减少参数、变量或输出的数量，可以将几个值合并为一个对象。

<!-- Not Available on Microsoft Azure content  [Objects as parameters](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md)-->
<!-- URL DIRECT TO https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters-->
