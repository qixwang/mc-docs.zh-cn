---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 31cfa4b21b0e7cfeb12c04caa9fe78ebb84a7a9a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63846577"
---
## <a name="issue-custom-image-provisioning-errors"></a>问题：自定义映像；预配错误
上传或捕获用作专用 VM 映像的通用化 VM 映像时，将发生预配错误，反之亦然。 前者会导致预配超时错误，后者会导致预配失败。 若要部署自定义映像且不出错，必须确保在捕获过程中映像类型不会更改。

下表列出了通用和专用映像的可能组合、将遇到的错误类型和修复错误需要执行的操作。

<!--Update_Description: wording update, update link-->