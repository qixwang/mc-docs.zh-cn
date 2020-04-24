---
title: Azure CLI 脚本示例 - 使用 NGINX 创建 Linux VM | Azure
description: Azure CLI 脚本示例 - 使用 NGINX 创建 Linux VM
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 02/27/2017
ms.date: 08/12/2019
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: 69a99e3a35884d96344f5cded86d662019b9fc03
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "69539047"
---
# <a name="create-a-vm-with-nginx"></a>使用 NGINX 创建 VM

此脚本创建一个 Azure 虚拟机，并使用 Azure 虚拟机自定义脚本扩展安装 NGINX。 运行此脚本后，可通过虚拟机的公共 IP 地址访问演示网站。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

# Sign in the Azure China Cloud
az cloud set -n AzureChinaCloud
az login 

# Create a resource group.
az group create --name myResourceGroup --location chinanorth

# Create a new virtual machine, this creates SSH keys if not present.
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys

# Open port 80 to allow web traffic to host.
az vm open-port --port 80 --resource-group myResourceGroup --name myVM 

# Use CustomScript extension to install NGINX.
az vm extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --vm-name myVM \
  --resource-group myResourceGroup \
  --settings '{"commandToExecute":"apt-get -y update && apt-get -y install nginx"}'
```

## <a name="custom-script-extension"></a>自定义脚本扩展

自定义脚本扩展将此脚本复制到虚拟机上。 然后运行该脚本，安装和配置 NGINX Web 服务器。

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="clean-up-deployment"></a>清理部署

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、虚拟机和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az group create](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az vm create](https://docs.azure.cn/cli/vm?view=azure-cli-latest#az-vm-create) | 创建虚拟机。 此命令还指定要使用的虚拟机映像和管理凭据。  |
| [az vm open-port](https://docs.azure.cn/cli/network/nsg/rule?view=azure-cli-latest) | 创建网络安全组规则，以允许入站流量。 在此示例中，将为 HTTP 流量打开端口 80。 |
| [azure vm extension set](https://docs.azure.cn/cli/vm/extension?view=azure-cli-latest) | 将虚拟机扩展添加到 VM 并运行该扩展。 在此示例中，使用自定义脚本扩展来安装 NGINX。|
| [az group delete](https://docs.azure.cn/cli/group?view=azure-cli-latest#az-group-delete)| 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.azure.cn/cli/index?view=azure-cli-latest)。

可以在 [Azure Linux VM 文档](../linux/cli-samples.md?toc=%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。

<!--Update_Description: update meta properties, update cmdlet -->