---
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 11/03/2016
ms.date: 09/10/2019
ms.author: v-tawe
ms.openlocfilehash: e7fffa13038ab9ed23355af761c244a1b9528710
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71059569"
---
## <a name="deployment-customization"></a>部署自定义

部署过程假设推送的 .zip 文件包含随时可运行的应用。 默认情况下，不会运行自定义。 若要启用通过持续集成获取的同一生成进程，请将以下内容添加到应用程序设置：

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

使用 .zip 推送部署时，此设置默认为“false”  。 持续集成部署的设置默认为“true”  。 设置为“true”时，在部署期间将使用与部署相关的设置  。 可以将这些设置配置为应用设置或在位于 .zip 文件根目录中的 .deployment 配置文件中进行配置。 有关详细信息，请参阅部署参考中的 [Repository and deployment-related settings](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings)（存储库和与部署相关的设置）。