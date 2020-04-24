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
ms.openlocfilehash: 80b4c5a9f613ff4626100d15e7bc70fdfc26a25e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63823449"
---
<!--Verify successfully-->
## <a name="push-image-to-registry"></a>将映像推送到注册表

要将映像推送到 Azure 容器注册表，首先必须具有一个映像。 如果还没有任何本地容器映像，请运行以下 [docker pull][docker-pull] 命令，从 Docker Hub 拉取现有映像。 就此示例来说，请拉取 `hello-world` 映像。

```
docker pull hello-world
```

将映像推送到注册表之前，必须使用 ACR 登录服务器的完全限定的名称进行标记。 登录服务器名称采用 *\<registry-name\>.azurecr.cn*（全小写）格式，例如 *mycontainerregistry007.azurecr.cn*。

使用 [docker tag][docker-tag] 命令标记映像。 使用 ACR 实例的登录服务器名称替换 `<acrLoginServer>`。

```
docker tag hello-world <acrLoginServer>/hello-world:v1
```

最后，使用 [docker push][docker-push] 将映像推送到 ACR 实例。 使用 ACR 实例的登录服务器名称替换 `<acrLoginServer>`。 此示例创建 **hello-world** 存储库，其中包含 `hello-world:v1` 映像。

```
docker push <acrLoginServer>/hello-world:v1
```

将映像推送到容器注册表后，请从本地 Docker 环境中删除 `hello-world:v1` 映像。 （请注意，此 [docker rmi][docker-rmi] 命令不从 Azure 容器注册表中的 **hello-world** 存储库删除该映像。）

```
docker rmi <acrLoginServer>/hello-world:v1
```

<!-- LINKS - External -->
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/

<!-- LINKS - Internal -->

<!--Update_Description: new articles on container registry quickstart docker push -->
<!--ms.date: 02/18/2019-->