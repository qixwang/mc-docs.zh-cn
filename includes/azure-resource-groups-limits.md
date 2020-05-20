---
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: include
origin.date: 02/24/2020
ms.date: 05/18/2020
ms.author: v-yeche
ms.openlocfilehash: 20e24d3b683e35a15bf0967c4621d6744786c4f4
ms.sourcegitcommit: 8d56bc6baeb42d675695ecef1909d76f5c4a6ae3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83406189"
---
| 资源 | 限制 |
| --- | --- |
| 每个[资源组](../articles/azure-resource-manager/management/overview.md#resource-groups)的资源 | 资源不受资源组限制。 相反，它们受资源组中资源类型的限制。 请参阅下一行。 |
| 每个资源组的资源（按资源类型） |800 - 某些资源类型可能超过 800 的限制。 请参阅[不限于每个资源组 800 个实例的资源](../articles/azure-resource-manager/management/resources-without-resource-group-limit.md)。 |
| 部署历史记录中每个资源组的部署数 |800<sup>1</sup> |
| 每个部署的资源数 |800 |
| 管理锁数（按唯一的作用域） |20 个 |
| 标记数（按资源或资源组） |50 |
| 标记键长度 |512 |
| 标记值长度 |256 |

<sup>1</sup>如果达到每个资源组的部署数限制 800，则会从历史记录中删除不再需要的部署。 从部署历史记录中删除条目不会影响已部署的资源。 有关详细信息，请参阅[解决部署计数超出 800 的错误](../articles/azure-resource-manager/templates/deployment-quota-exceeded.md)。

#### <a name="template-limits"></a>模板限制

| Value | 限制 |
| --- | --- |
| parameters |256 |
| 变量 |256 |
| 资源（包括副本计数） |800 |
| Outputs |64 |
| 模板表达式 |24,576 个字符 |
| 已导出模板中的资源 |200 |
| 模板大小 |4 MB |
| 参数文件大小 |64 KB |

通过使用嵌套模板，可超出某些模板限制。 有关详细信息，请参阅[部署 Azure 资源时使用链接的模板](../articles/azure-resource-manager/templates/linked-templates.md)。 若要减少参数、变量或输出的数量，可以将几个值合并为一个对象。

<!-- Not Available on Microsoft Azure content  [Objects as parameters](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md)-->
<!-- URL DIRECT TO https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters-->
