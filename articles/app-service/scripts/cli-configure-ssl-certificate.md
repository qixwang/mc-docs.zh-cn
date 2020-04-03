---
title: CLI：将 SSL 证书上传并绑定到应用
description: 了解如何使用 Azure CLI 自动部署和管理应用服务应用。 此示例介绍如何将自定义 SSL 证书绑定到应用。
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.devlang: azurecli
ms.topic: sample
origin.date: 12/11/2017
ms.date: 03/30/2020
ms.author: v-tawe
ms.custom: mvc, seodec18
ms.openlocfilehash: 5f37fd823c27112ed55680790aaf11865a5e4624
ms.sourcegitcommit: 44d3fe59952847e5394bbe6c05bd6f333bb56345
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2020
ms.locfileid: "80522114"
---
# <a name="bind-a-custom-ssl-certificate-to-an-app-service-app-using-cli"></a>使用 CLI 将自定义 SSL 证书绑定到应用服务应用

此示例脚本使用其相关资源在应用服务中创建一个应用，然后将自定义域名的 SSL 证书绑定到该应用。 在此示例中，需要以下项：

* 访问域注册机构的 DNS 配置页的权限。
* 需要上传和绑定的 SSL 证书的有效 .PFX 文件及其密码。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

如果选择在本地安装并使用 CLI，则需使用 Azure CLI 2.0 或更高版本。 若要查找版本，请运行 `az --version`。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/install-azure-cli?view=azure-cli-lastest)。

## <a name="sample-script"></a>示例脚本

```azurecli
#!/bin/bash

fqdn=<replace-with-www.{yourdomain}>
pfxPath=<replace-with-path-to-your-.PFX-file>
pfxPassword=<replace-with-your=.PFX-password>
resourceGroup=myResourceGroup
webappname=mywebapp$RANDOM

# Create a resource group.
az group create --location chinanorth --name $resourceGroup

# Create an App Service plan in Basic tier (minimum required by custom domains).
az appservice plan create --name $webappname --resource-group $resourceGroup --sku B1

# Create a web app.
az webapp create --name $webappname --resource-group $resourceGroup \
--plan $webappname

echo "Configure a CNAME record that maps $fqdn to $webappname.chinacloudsites.cn"
read -p "Press [Enter] key when ready ..."

# Before continuing, go to your DNS configuration UI for your custom domain and follow the 
# instructions at https://docs.azure.cn/app-service/app-service-web-tutorial-custom-domain#step-2-create-the-dns-records to configure a CNAME record for the 
# hostname "www" and point it your web app's default domain name.

# Map your prepared custom domain name to the web app.
az webapp config hostname add --webapp-name $webappname --resource-group $resourceGroup \
--hostname $fqdn

# Upload the SSL certificate and get the thumbprint.
thumbprint=$(az webapp config ssl upload --certificate-file $pfxPath \
--certificate-password $pfxPassword --name $webappname --resource-group $resourceGroup \
--query thumbprint --output tsv)

# Binds the uploaded SSL certificate to the web app.
az webapp config ssl bind --certificate-thumbprint $thumbprint --ssl-type SNI \
--name $webappname --resource-group $resourceGroup

echo "You can now browse to https://$fqdn"
```

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [`az group create`](/cli/group?view=azure-cli-latest#az-group-create) | 创建用于存储所有资源的资源组。 |
| [`az appservice plan create`](/cli/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | 创建应用服务计划。 |
| [`az webapp create`](/cli/webapp?view=azure-cli-latest#az-webapp-create) | 创建应用服务应用。 |
| [`az webapp config hostname add`](/cli/webapp/config/hostname?view=azure-cli-latest#az-webapp-config-hostname-add) | 将自定义域映射到应用服务应用。 |
| [`az webapp config ssl upload`](/cli/webapp/config/ssl?view=azure-cli-latest#az-webapp-config-ssl-upload) | 将 SSL 证书上传到应用服务应用。 |
| [`az webapp config ssl bind`](/cli/webapp/config/ssl?view=azure-cli-latest#az-webapp-config-ssl-bind) | 将上传的 SSL 证书绑定到应用服务应用。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/overview?view=azure-cli-lastest)。

可以在 [Azure 应用服务文档](../samples-cli.md)中找到其他应用服务 CLI 脚本示例。
