---
title: 从 Azure CLI 管理 IoT Central | Microsoft Docs
description: 本文介绍如何使用 CLI 创建和管理 IoT Central 应用程序。 可以使用 CLI 查看、修改和删除应用程序。
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 02/11/2020
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 1ab69a061928de74fc755a2d0a387e2872b94692
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80343182"
---
# <a name="manage-iot-central-from-azure-cli"></a>从 Azure CLI 管理 IoT Central

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

如果不在 [Azure IoT Central 应用程序管理器](https://aka.ms/iotcentral)网站上创建和管理 IoT Central 应用程序，可以使用 [Azure CLI](/cli/) 来管理应用程序。

## <a name="prerequisites"></a>必备条件

如果没有 Azure 订阅，可在开始前创建一个 [试用帐户](https://www.azure.cn/pricing/1rmb-trial) 。

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

如果你偏好于在本地计算机上运行 Azure CLI，请参阅[安装 Azure CLI](/cli/install-azure-cli)。 在本地运行 Azure CLI 时，请在尝试运行本文所述的命令之前，使用 **az login** 命令登录到 Azure。

> [!TIP]
> 如果需要在不同的 Azure 订阅中运行 CLI 命令，请参阅[更改活动订阅](/cli/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#change-the-active-subscription)。

## <a name="create-an-application"></a>创建应用程序

使用 [az iotcentral app create](https://docs.microsoft.com/cli/azure/iotcentral/app#az-iotcentral-app-create) cmdlet 在 Azure 订阅中创建一个 IoT Central 应用程序。 例如：

```azurecli
# Create a resource group for the IoT Central application
az group create --location "China East" \
    --name "MyIoTCentralResourceGroup"
```

```azurecli
# Create an IoT Central application
az iotcentral app create \
  --resource-group "MyIoTCentralResourceGroup" \
  --name "myiotcentralapp" --subdomain "mysubdomain" \
  --sku ST1 --template "iotc-pnp-preview@1.0.0" \
  --display-name "My Custom Display Name"
```

这些命令首先在“中国东部”区域中为应用程序创建一个资源组。 下表描述了与 **az iotcentral app create** 命令结合使用的参数：

| 参数         | 说明 |
| ----------------- | ----------- |
| resource-group    | 包含该应用程序的资源组。 此资源组必须已存在于订阅中。 |
| location          | 此命令默认使用资源组中的位置。 |
| name              | 应用程序在 Azure 门户中的名称。 |
| subdomain         | 应用程序 URL 中的子域。 在该示例中，应用程序 URL 为 https://mysubdomain.azureiotcentral.com 。 |
| sku               | 目前，可以使用 **ST1** 或 **ST2**。 请参阅 [Azure IoT Central 定价](https://azure.microsoft.com/pricing/details/iot-central/)。 |
| template          | 要使用的应用程序模板。 有关详细信息，请参阅下表。 |
| display-name      | UI 中显示的应用程序名称。 |

[!INCLUDE [iot-central-template-list](../../../includes/iot-central-template-list.md)]

## <a name="view-your-applications"></a>查看应用程序

使用 [az iotcentral app list](https://docs.microsoft.com/cli/azure/iotcentral/app#az-iotcentral-app-list) 命令可列出 IoT Central 应用程序和查看元数据。

## <a name="modify-an-application"></a>修改应用程序

使用 [az iotcentral app update](https://docs.microsoft.com/cli/azure/iotcentral/app#az-iotcentral-app-update) 命令可更新 IoT Central 应用程序的元数据。 例如，若要更改应用程序的显示名称：

```azurecli
az iotcentral app update --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup \
  --set displayName="My new display name"
```

## <a name="remove-an-application"></a>删除应用程序

使用 [az iotcentral app delete](https://docs.microsoft.com/cli/azure/iotcentral/app#az-iotcentral-app-delete) 命令可删除 IoT Central 应用程序。 例如：

```azurecli
az iotcentral app delete --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup
```

## <a name="next-steps"></a>后续步骤

了解如何从 Azure CLI 管理 Azure IoT Central 应用程序后，建议接下来学习：

> [!div class="nextstepaction"]
> [管理应用程序](howto-administer.md)
