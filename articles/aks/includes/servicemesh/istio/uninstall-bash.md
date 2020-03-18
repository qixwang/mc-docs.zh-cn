---
author: rockboyfor
ms.topic: include
origin.date: 11/15/2019
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: b4ab78c575e40d6600ca18121f029f662b052073
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79290704"
---
若要删除机密，请运行以下命令：

```bash
kubectl get secret --all-namespaces -o json | jq '.items[].metadata | ["kubectl delete secret -n", .namespace, .name] | join(" ")' -r | fgrep "istio." | xargs -t0 bash -c
```

<!-- Update_Description: update meta properties, wording update, update link -->