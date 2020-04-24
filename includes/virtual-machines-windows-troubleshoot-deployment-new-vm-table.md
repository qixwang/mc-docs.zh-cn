---
author: rockboyfor
ms.service: virtual-machines-windows
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 4900dc0c5754a0263b7e099992cb0eb6269e6d61
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63846576"
---
下表列出了可能的 Windows 通用和专用OS 映像的上传和捕获组合。 使用 Y 表示处理不会有任何错误的组合，使用 N 表示会出现错误的组合。下表提供了有关各种错误的原因和解决方法。

| 操作系统 | 上传专用 OS 映像 | 上传通用 OS 映像 | 捕获专用 OS 映像 | 捕获通用 OS 映像 |
| --- | --- | --- | --- | --- |
| Windows 通用 |N<sup>1</sup> |Y |N<sup>3</sup> |Y |
| Windows 专用 |Y |N<sup>2</sup> |Y |N<sup>4</sup> |

<!--Update_Description: wording update, update link-->