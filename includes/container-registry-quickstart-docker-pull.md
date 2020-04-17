---
title: include 文件
description: include 文件
services: container-registry
author: rockboyfor
ms.service: container-registry
ms.topic: include
origin.date: 01/23/2019
ms.date: 02/18/2019
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 81bf959e69bbad5e4b8c1e1b1df1c27f77212d50
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63823450"
---
<!--Verify successfully-->
## <a name="run-image-from-registry"></a>从注册表运行映像

现在，可以使用 `hello-world:v1`docker run[ 从容器注册表拉取并运行 ][docker-run] 容器映像：

```
docker run <acrLoginServer>/hello-world:v1  
```

示例输出： 

```
Unable to find image 'mycontainerregistry007.azurecr.cn/hello-world:v1' locally
v1: Pulling from hello-world
Digest: sha256:662dd8e65ef7ccf13f417962c2f77567d3b132f12c95909de6c85ac3c326a345
Status: Downloaded newer image for mycontainerregistry007.azurecr.cn/hello-world:v1

Hello from Docker!
This message shows that your installation appears to be working correctly.

[...]
```

<!-- LINKS - External -->
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/

<!--Update_Description: new articles on container registry quickstart docker pull -->
<!--ms.date: 02/18/2019-->