---
author: rockboyfor
ms.topic: include
origin.date: 10/09/2019
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: ae5ec0b0f1fd97a40e88e7505880bb8b2d9cbe8c
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79290706"
---
在 Linux 或[适用于 Linux 的 Windows 子系统][install-wsl]或 MacOS 上的基于 bash 的 shell 中，使用 `curl` 下载 Consul Helm 图表版本，如下所示：

```bash
# Specify the Consul Helm chart version that will be leveraged throughout these instructions
CONSUL_HELM_VERSION=0.10.0

curl -sL "https://github.com/hashicorp/consul-helm/archive/v$CONSUL_HELM_VERSION.tar.gz" | tar xz
mv consul-helm-$CONSUL_HELM_VERSION consul-helm
```

<!-- LINKS - external -->

[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

<!--Update_Description: new articles on consul download bash -->
<!--New.date: 11/04/2019-->