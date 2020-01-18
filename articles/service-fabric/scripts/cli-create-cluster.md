---
title: Azure CLI 脚本部署示例
description: 如何使用 Azure 命令行接口 (CLI) 在 Azure 中创建安全的 Service Fabric Linux 群集。
services: service-fabric
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.topic: sample
origin.date: 01/18/2018
ms.date: 01/06/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: f9d6b2c413bcaf734937cbfb46f0eeff69ec89a7
ms.sourcegitcommit: 713136bd0b1df6d9da98eb1da7b9c3cee7fd0cee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75741882"
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>在 Azure 中创建安全的 Service Fabric Linux 群集

此命令创建一个自签名证书，将其添加到密钥保管库并在本地下载该证书。  新证书用于在群集部署时保护群集。  也可以使用现有证书而不是创建一个新证书。  不管怎样，证书的使用者名称都必须与用于访问 Service Fabric 群集的域匹配。 只有满足此匹配，才能为群集的 HTTPS 管理终结点和 Service Fabric Explorer 提供 SSL。 无法从 CA 获取 `.cloudapp.chinacloudapi.cn` 域的 SSL 证书。 必须获取群集的自定义域名。 从 CA 请求证书时，该证书的使用者名称必须与用于群集的自定义域名匹配。

根据需要安装 [Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

## <a name="sample-script"></a>示例脚本

```sh
#!/bin/bash

# Variables
ResourceGroupName="aztestclustergroup" 
ClusterName="aztestcluster" 
Location="chinaeast" 
Password="q6D7nN%6ck@6" 
Subject="aztestcluster.chinaeast.cloudapp.chinacloudapi.cn" 
VaultName="aztestkeyvault" 
VmPassword="Mypa$$word!321"
VmUserName="sfadminuser"

# Create resource group
az group create --name $ResourceGroupName --location $Location 

# Create secure five node Linux cluster. Creates a key vault in a resource group
# and creates a certficate in the key vault. The certificate's subject name must match 
# the domain that you use to access the Service Fabric cluster.  The certificate is downloaded locally.
az sf cluster create --resource-group $ResourceGroupName --location $Location \ 
  --certificate-output-folder . --certificate-password $Password --certificate-subject-name $Subject \
  --cluster-name $ClusterName --cluster-size 5 --os UbuntuServer1604 --vault-name $VaultName \ 
  --vault-resource-group $ResourceGroupName --vm-password $VmPassword --vm-user-name $VmUserName

```

## <a name="clean-up-deployment"></a>清理部署

运行脚本示例后，可以使用以下命令删除资源组、群集以及所有相关资源。

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [az sf cluster create](https://docs.azure.cn/cli/sf/cluster?view=azure-cli-latest#az-sf-cluster-create) | 新建 Service Fabric 群集。  |

## <a name="next-steps"></a>后续步骤

在 [Service Fabric CLI 示例](../samples-cli.md)中可找到 Azure Service Fabric 的其他 Service Fabric CLI 示例。

<!--Update_Description: update meta properties, update wording -->