---
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 11/09/2018
ms.author: v-lingwu
ms.openlocfilehash: f2f0c25a79088cf4dd59cbfe3bb766b1548ab83a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67845566"
---
### <a name="retrieve-output-files"></a>检索输出文件

可以使用 Azure 门户下载 ffmpeg 任务生成的输出 MP3 文件。 

1. 单击“所有服务” > “存储帐户”，然后单击存储帐户的名称。
2. 单击“Blob” > “输出”。
3. 右键单击一个输出 MP3 文件，然后单击“下载”。 在浏览器中按提示打开或保存该文件。

![下载输出文件](./media/batch-common-tutorial-download/download.png)

也可以编程方式从计算节点或存储容器下载这些文件（但在本示例中未演示）。
