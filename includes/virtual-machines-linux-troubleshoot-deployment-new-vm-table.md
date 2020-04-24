---
author: rockboyfor
ms.service: virtual-machines-linux
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: bd4c393d03cfaed80bf44d9bb6bcb789c170f3c8
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63849840"
---
下表列出了可能的 Linux 通用和专用 OS 映像的上传与捕获组合。 使用 Y 表示处理不会有任何错误的组合，使用 N 表示会出现错误的组合。下表提供了有关各种错误的原因和解决方法。

| OS | 上传专用 OS 映像 | 上传通用 OS 映像 | 捕获专用 OS 映像 | 捕获通用 OS 映像 |
| --- | --- | --- | --- | --- |
| Linux 通用 |N<sup>1</sup> |Y |N<sup>3</sup> |Y |
| Linux 专用 |Y |N<sup>2</sup> |Y |N<sup>4</sup> |

<!-- Update_Description: update meta properties, wording update -->