---
title: 如何将模型部署到计算实例
titleSuffix: Azure Machine Learning
description: 了解如何使用计算实例将 Azure 机器学习模型部署为 Web 服务。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: mnark
author: MrudulaN
ms.reviewer: larryfr
ms.date: 10/25/2019
ms.openlocfilehash: fcb27cbf5836bc16bee434919103b10676e234ac
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75599357"
---
# <a name="deploy-a-model-to-azure-machine-learning-compute-instances"></a>将模型部署到 Azure 机器学习计算实例

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

> [!NOTE]
> 计算实例（预览版）仅适用于区域为“美国中北部”或“英国南部”的工作区   。
>如果你的工作区在任何其他区域，则可以继续创建并使用[笔记本 VM](concept-compute-instance.md#notebookvm)。  可以使用本文中的步骤，将模型部署到计算实例或笔记本 VM。

了解如何使用 Azure 机器学习将模型部署为 Azure 机器学习计算实例上的 Web 服务。 如果满足下列任一条件，请使用计算实例：

- 你需要快速部署并验证你的模型。
- 正在测试一个开发中的模型。

> [!TIP]
> 将模型从计算实例上的 Jupyter Notebook 部署到同一 VM 上的 Web 服务是本地部署  。 在这种情况下，“本地”计算机是计算实例。 有关部署的详细信息，请参阅[使用 Azure 机器学习部署模型](service/how-to-deploy-and-where.md)。

## <a name="prerequisites"></a>先决条件

- 一个有计算实例运行的 Azure 机器学习工作区。 有关详细信息，请参阅[设置环境和工作区](tutorial-1st-experiment-sdk-setup.md)。

## <a name="deploy-to-the-compute-instances"></a>部署到计算实例

用于演示本地部署的示例笔记本包含在计算实例中。 使用以下步骤加载笔记本，并将模型部署为 VM 上的 Web 服务：

1. 从 [Azure 机器学习工作室](https://ml.azure.com)选择 Azure 机器学习计算实例。

1. 打开 `samples-*` 子目录，然后打开 `how-to-use-azureml/deploy-to-local/register-model-deploy-local.ipynb`。 打开后，运行笔记本。

    ![笔记本上运行的本地服务的屏幕截图](./media/how-to-deploy-local-container-notebook-vm/deploy-local-service.png)

1. 笔记本会显示其上有服务运行的 URL 和端口。 例如，`https://localhost:6789`。 还可以运行包含 `print('Local service port: {}'.format(local_service.port))` 的单元格以显示端口。

    ![正在运行的本地服务端口的屏幕截图](./media/how-to-deploy-local-container-notebook-vm/deploy-local-service-port.png)

1. 若要从计算实例测试服务，请使用 `https://localhost:<local_service.port>` URL。 若要从远程客户端进行测试，请获取在计算实例上运行的服务的公共 URL。 可以使用以下公式来确定公共 URL； 
    * 笔记本 VM：`https://<vm_name>-<local_service_port>.<azure_region_of_workspace>.notebooks.azureml.net/score`。 
    * 计算实例：`https://<vm_name>-<local_service_port>.<azure_region_of_workspace>.instances.azureml.net/score`。 

    例如， 
    * 笔记本 VM：`https://vm-name-6789.northcentralus.notebooks.azureml.net/score` 
    * 计算实例：`https://vm-name-6789.northcentralus.instances.azureml.net/score`

## <a name="test-the-service"></a>测试服务

若要将示例数据提交到正在运行的服务，请使用以下代码。 将 `service_url` 值替换为在上一步获取的 URL：

```python
import requests
import json
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10],
    [10,9,8,7,6,5,4,3,2,1]
]})
test_sample = bytes(test_sample,encoding = 'utf8')
access_token = "your bearer token"
headers = {'Content-Type':'application/json', 'Authorization': 'Bearer ' + access_token}
service_url = "https://vm-name-6789.northcentralus.notebooks.azureml.net/score"
# for a compute instance, the url would be https://vm-name-6789.northcentralus.instances.azureml.net/score
resp = requests.post(service_url, test_sample, headers=headers)
print("prediction:", resp.text)
```

## <a name="next-steps"></a>后续步骤

* [如何使用自定义 Docker 映像部署模型](service/how-to-deploy-custom-docker-image.md)
* [部署疑难解答](how-to-troubleshoot-deployment.md)
* [使用 SSL 保护 Azure 机器学习 Web 服务](how-to-secure-web-service.md)
* [使用部署为 Web 服务的机器学习模型](how-to-consume-web-service.md)
* [使用 Application Insights 监视 Azure 机器学习模型](how-to-enable-app-insights.md)
* [为生产环境中的模型收集数据](how-to-enable-data-collection.md)