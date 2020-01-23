---
title: Azure Service Fabric CLI (sfctl) 脚本删除示例
description: 使用 Azure Service Fabric CLI 从 Azure Service Fabric 群集中删除应用程序
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
origin.date: 12/06/2017
ms.date: 01/06/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 6e4cf897e7d7964233bb9d02d5d13a207dfcb2a3
ms.sourcegitcommit: 713136bd0b1df6d9da98eb1da7b9c3cee7fd0cee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75742297"
---
# <a name="remove-an-application-from-a-service-fabric-cluster-using-the-service-fabric-cli"></a>使用 Service Fabric CLI 从 Service Fabric 群集中删除应用程序

此示例脚本删除了正在运行的 Service Fabric 应用程序实例，并从群集中取消注册了应用程序类型和版本。  删除应用程序实例时也会删除与该应用程序关联的所有正在运行的服务实例。 接下来，会从映像存储区中删除应用程序文件。 

如果需要，请安装 [Service Fabric CLI](../service-fabric-cli.md)。

## <a name="sample-script"></a>示例脚本

```sh
#!/bin/bash

# Select cluster
sfctl cluster select \
    --endpoint http://svcfab1.chinanorth2.cloudapp.chinacloudapi.cn:19080

# Delete the application
sfctl application delete \
    --application-id svcfab_app \
    --timeout 500

# Unprovision the application type
sfctl application unprovision \
    --application-type-name svcfab_appType \
    --application-type-version 1.0.0 \
    --timeout 500

# Delete the application files from the image store
sfctl store delete \
    --content-path myappfolder
```

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅 [Service Fabric CLI 文档](../service-fabric-cli.md)。

在 [Service Fabric CLI 示例](../samples-cli.md)中可找到 Azure Service Fabric 的其他 Service Fabric CLI 示例。

<!--Update_Description: update meta properties, update link -->