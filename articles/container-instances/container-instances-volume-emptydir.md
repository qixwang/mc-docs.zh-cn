---
title: 将 emptyDir 卷装载到容器组
description: 了解如何在 Azure 容器实例中装载 emptyDir 卷以在容器组中的容器之间共享数据
ms.topic: article
origin.date: 02/08/2018
ms.date: 01/15/2020
ms.author: v-yeche
ms.openlocfilehash: fc0dd283d592e4b7faa9bd543d484314d87778d9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78213735"
---
<!--Verified successfully-->
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>在 Azure 容器实例中装载 emptyDir 卷

了解如何在 Azure 容器实例中装载 *emptyDir* 卷以在容器组中的容器之间共享数据。 使用 emptyDir  卷作为容器化工作负荷的临时缓存。

> [!NOTE]
> 当前只有 Linux 容器能装载 *emptyDir* 卷。

<!--Not Available on  While we are working to bring all features to Windows containers, you can find current platform differences in the [overview](container-instances-overview.md#linux-and-windows-containers).-->

## <a name="emptydir-volume"></a>emptyDir 卷

*emptyDir* 卷提供了容器组中的每个容器都可访问的可写目录。 该组中的容器可以读取和写入此卷中的相同文件，并且可以在每个容器中使用相同或不同路径装载此卷。

一些示例用于 *emptyDir* 卷：

* 暂存空间
* 长时间运行任务期间的检查点
* 存储由挎斗容器检索的数据以及由应用程序容器提供的数据

*emptyDir* 卷中的数据将一直保留到容器崩溃。 但是，并不保证重新启动的容器能够持久保留 *emptyDir* 卷中的数据。 如果停止容器组，则不会持久保留 emptyDir  卷。

## <a name="mount-an-emptydir-volume"></a>装载 emptyDir 卷

若要将 emptyDir 卷装载到容器实例中，必须使用 Azure 资源管理器模板进行部署。

<!--Not Available on [Azure Resource Manager template](https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups)-->

首先，在模板的容器组 `properties` 节中填充 `volumes` 数组。 接下来，针对容器组中希望装载 *emptyDir* 卷的每个容器，在容器定义的 `properties` 节中填充 `volumeMounts` 数组。

例如，以下资源管理器模板创建了一个包含两个容器的容器组，每个容器均装载了 *emptyDir* 卷：

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-emptydir.json -->

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "volume-demo-emptydir",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-02-01-preview",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                }
              ],
              "volumeMounts": [
                {
                  "name": "emptydir1",
                  "mountPath": "/mnt/empty"
                }
              ]
            }
          },
          {
            "name": "[variables('container2name')]",
            "properties": {
              "image": "[variables('container2image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "volumeMounts": [
                {
                  "name": "emptydir1",
                  "mountPath": "/mnt/empty"
                }
              ]
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            }
          ]
        },
        "volumes": [
          {
            "name": "emptydir1",
            "emptyDir": {}
          }
        ]
      }
    }
  ]
}
```

若要查看使用 Azure 资源管理器模板进行的容器实例部署示例，请参阅[在 Azure 容器实例中部署多容器组](container-instances-multi-container-group.md)。

## <a name="next-steps"></a>后续步骤

了解如何在 Azure 容器实例中装载其他卷类型：

* [在 Azure 容器实例中装载 Azure 文件共享](container-instances-volume-azure-files.md)
* [在 Azure 容器实例中装载 gitRepo 卷](container-instances-volume-gitrepo.md)
* [在 Azure 容器实例中装载机密卷](container-instances-volume-secret.md)

<!-- Update_Description: new article about container instances volume emptydir -->
<!--NEW.date: 01/15/2020-->