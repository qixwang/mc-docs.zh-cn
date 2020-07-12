---
author: rockboyfor
ms.topic: include
origin.date: 10/09/2019
ms.date: 07/13/2020
ms.author: v-yeche
ms.openlocfilehash: 3290d9f03a66dd93635942f5601145d8c678fcbb
ms.sourcegitcommit: 6c9e5b3292ade56d812e7e214eeb66aeb9b8776e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86218804"
---
在 Windows 上基于 PowerShell 的 shell 中，使用 `Invoke-WebRequest` 下载 Consul Helm 图表版本，然后使用 `Expand-Archive` 进行解压缩，如下所示：

```powershell
# Specify the Consul Helm chart version that will be leveraged throughout these instructions
$CONSUL_HELM_VERSION="0.10.0"

# Enforce TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/hashicorp/consul-helm/archive/v$CONSUL_HELM_VERSION.zip" -OutFile "consul-helm-$CONSUL_HELM_VERSION.zip"
Expand-Archive -Path "consul-helm-$CONSUL_HELM_VERSION.zip" -DestinationPath .
Move-Item -Path consul-helm-$CONSUL_HELM_VERSION -Destination consul-helm
```

<!--Update_Description: new articles on consul download powershell -->
<!--New.date: 11/04/2019-->