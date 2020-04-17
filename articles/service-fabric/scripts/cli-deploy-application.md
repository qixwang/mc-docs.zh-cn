---
title: Azure Service Fabric CLI (sfctl) 脚本部署示例
description: 使用 Azure Service Fabric CLI 将应用程序部署到 Azure Service Fabric 群集
services: service-fabric
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
origin.date: 04/16/2018
ms.date: 01/06/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: e43b0e8fcd5f93a9f7f916f4061a2945a508594f
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75741881"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster-using-the-service-fabric-cli"></a>使用 Service Fabric CLI 将应用程序部署到 Service Fabric 群集

此示例脚本将应用程序包复制到群集映像存储，在群集中注册应用程序类型，并从应用程序类型创建应用程序实例。 此时还将创建任何默认服务。

如果需要，请安装 [Service Fabric CLI](../service-fabric-cli.md)。

## <a name="sample-script"></a>示例脚本

```sh
#!/bin/bash

# Select cluster
sfctl cluster select \
    --endpoint http://svcfab1.chinanorth2.cloudapp.chinacloudapi.cn:19080

# Upload the application files to the image store
# (note the last folder name, Debug in this example)
sfctl application upload \
    --path  C:\Code\svcfab-vs\svcfab-vs\pkg\Debug \
    --show-progress

# Register the application (manifest files) from the image store
# (Note the last folder from the previous command is used: Debug)
sfctl application provision \
    --application-type-build-path Debug \
    --timeout 500

# Create an instance of the registered application and 
# auto deploy any defined services
sfctl application create \
    --app-name fabric:/MyApp \
    --app-type MyAppType \
    --app-version 1.0.0

```

## <a name="clean-up-deployment"></a>清理部署

完成后，可使用[删除](cli-remove-application.md)脚本删除应用程序。 删除脚本会删除应用程序实例、注销应用程序类型，并从映像存储中删除应用程序包。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [Service Fabric CLI 文档](../service-fabric-cli.md)。

在 [Service Fabric CLI 示例](../samples-cli.md)中可找到 Azure Service Fabric 的其他 Service Fabric CLI 示例。

<!--Update_Description: update meta properties, update link -->