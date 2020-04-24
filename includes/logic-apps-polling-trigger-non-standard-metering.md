---
ms.openlocfilehash: 9cab506afcfd8d432082115de74265b0a20f67d6
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63826120"
---
若要估计更准确的使用成本，请考虑任何给定天可能到达的消息或事件数，而不是仅基于轮询间隔进行计算。 当某个事件或消息满足触发器条件时，许多触发器将立即尝试读取满足条件的任何和所有其他等待事件或消息。 此行为意味着，即使你选择较长的轮询间隔，触发器也基于符合启动工作流条件的等待事件或消息的数量进行触发。 遵循此行为的触发器包括 Azure 服务总线和 Azure 事件中心。

因此，例如，假设你设置了一个每天检查终结点的触发器。 当触发器检查终结点并找到 15 个满足条件的事件时，触发器触发并运行相应工作流 15 次。 逻辑应用会计量这 15 个工作流执行的所有操作，包括触发器请求。 若要计算潜在成本，请尝试使用 [Azure 定价计算器](https://azure.microsoft.com/pricing/calculator/)。