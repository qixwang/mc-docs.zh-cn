---
title: 在专用主机上部署
description: 使用专用主机实现工作负载的真正主机级隔离
ms.topic: article
origin.date: 01/10/2020
ms.date: 01/15/2020
ms.author: v-yeche
ms.openlocfilehash: 7b4bdd2df11c39871b7d57e0114898e5b09a0fca
ms.sourcegitcommit: ada94ca4685855f58616e4bf1dd5ca757878dfdc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77428604"
---
<!--not available on DEDICATED HOSTS-->
<!-- API must be 2019-12-01 while 2018-10-01 ON MOONCAKE-->
# <a name="deploy-on-dedicated-hosts"></a>在专用主机上部署

“专用”是指一个 Azure 容器实例 (ACI) SKU，它为安全运行的容器提供独立且专用的计算环境。 使用专用 SKU 会使每个容器组在 Azure 数据中心拥有专用的物理服务器，确保工作负载完全隔离，有助于满足组织的安全性和符合性要求。 

专用 SKU 适用于需要从物理服务器角度隔离工作负载的容器工作负载。

## <a name="using-the-dedicated-sku"></a>使用专用 SKU

> [!IMPORTANT]
> 只能在当前推出的最新 API 版本 (2019-12-01) 中使用专用 SKU。请在部署模板中指定此 API 版本。 此外，使用专用 SKU 的任何订阅的默认限制均为 0。 如果要将此 SKU 用于生产容器部署，请创建 [Azure 支持请求][azure-support]

从 API 版本 2019-12-01 开始，部署模板的容器组属性部分下有一个“sku”属性，它是 ACI 部署所必需的。 目前，可以将此属性用作 ACI 的 Azure 资源管理器部署模板的一部分。 若要详细了解如何使用模板来部署 ACI 资源，可参阅[教程：使用资源管理器模板部署多容器组](/container-instances/container-instances-multi-container-group)。 

此 sku 属性可以使用下列值之一：
* Standard - 标准 ACI 部署选择，仍可保证虚拟机监控程序级别的安全性 
* Dedicated - 用于工作负载级别的隔离，对容器组使用专用物理主机

## <a name="modify-your-json-deployment-template"></a>修改 JSON 部署模板

在指定了容器组资源的部署模板中，确保 `"apiVersion": "2019-12-01",`。 在容器组资源的 properties 节中，设置 `"sku": "Dedicated",`。

下面是使用专用 SKU 的容器组部署模板的 resources 节的示例代码片段：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "containerGroupName": {
        "type": "string",
        "defaultValue": "myContainerGroup",
        "metadata": {
          "description": "Container Group name."
        }
      }
    },
    "resources": [
        {
            "name": "[parameters('containerGroupName')]",
            "type": "Microsoft.ContainerInstance/containerGroups",
            "apiVersion": "2019-12-01",
            "location": "[resourceGroup().location]",
            "properties": {
                "sku": "Dedicated",
                "containers": [
                    {
                        "name": "container1",
                        "properties": {
                            "image": "nginx",
                            "command": [
                                "/bin/sh",
                                "-c",
                                "while true; do echo `date`; sleep 1000000; done"
                            ],
                            "ports": [
                                {
                                    "protocol": "TCP",
                                    "port": 80
                                }
                            ],
                            "environmentVariables": [],
                            "resources": {
                                "requests": {
                                    "memoryInGB": 1.0,
                                    "cpu": 1
                                }
                            }
                        }
                    }
                ],
                "restartPolicy": "Always",
                "ipAddress": {
                    "ports": [
                        {
                            "protocol": "TCP",
                            "port": 80
                        }
                    ],
                    "type": "Public"
                },
                "osType": "Linux",
            },
            "location": "chinaeast22chinanorth2",
            "tags": {}
        }
    ]
}
```

## <a name="deploy-your-container-group"></a>部署容器组

如果在桌面上创建并编辑了部署模板文件，可以通过拖动文件的方式将其上传到 Cloud Shell 目录。

<!--Not Available on  you can upload it to your Cloud Shell directory by dragging the file into it.-->

使用 [az group create][az-group-create] 命令创建资源组。

```azurecli
az group create --name myResourceGroup --location chinaeast2
```

使用 [az group deployment create][az-group-deployment-create] 命令部署模板。

```azurecli
az group deployment create --resource-group myResourceGroup --template-file deployment-template.json
```

将在几秒钟内收到来自 Azure 的初始响应。 部署完成后，所有与之相关且由 ACI 服务保存的数据将使用提供的密钥进行加密。

<!-- LINKS - Internal -->

[az-group-create]: https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create
[az-group-deployment-create]: https://docs.azure.cn/cli/group/deployment?view=azure-cli-latest#az-group-deployment-create

<!-- LINKS - External -->

[azure-support]: https://support.azure.cn/support/support-azure/

<!-- Update_Description: new article about container instances dedicated hosts -->
<!--NEW.date: 01/15/2020-->