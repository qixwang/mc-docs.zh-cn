---
title: include 文件
description: include 文件
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 02/02/2018
ms.date: 03/09/2020
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: 69814b04678c3e03c25ad1c965664ff9fa3973e5
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78155484"
---
回到本地终端窗口，将 Azure 远程功能添加到本地 Git 存储库。 将 *\<deploymentLocalGitUrl-from-create-step>* 替换为从[创建 Web 应用](#create-a-web-app)保存的 Git 远程 URL。

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

使用以下命令推送到 Azure 远程库以部署应用。 当 Git 凭据管理器提示输入凭据时，请确保输入在[配置部署用户](/app-service/containers/tutorial-python-postgresql-app#configure-a-deployment-user)中创建的凭据，而不是用于登录到 Azure 门户的凭据。

```bash
git push azure master
```

此命令可能需要花费几分钟时间运行。 运行时，该命令会显示类似于以下示例的信息：
