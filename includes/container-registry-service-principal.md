---
title: include 文件
description: include 文件
services: container-registry
author: rockboyfor
ms.service: container-registry
ms.topic: include
origin.date: 08/03/2018
ms.date: 11/12/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 60b62ef3bc22a2fb59541056e790a46248fdb1b0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72303278"
---
## <a name="create-a-service-principal"></a>创建服务主体

若要创建可以访问容器注册表的服务主体，请在本地安装的 [Azure CLI](https://docs.azure.cn/zh-cn/cli/install-azure-cli?view=azure-cli-latest) 中运行以下脚本。 此脚本已针对 Bash Shell 格式化。

运行脚本之前，请将 `ACR_NAME` 变量更新为容器注册表的名称。 `SERVICE_PRINCIPAL_NAME` 值必须在 Azure Active Directory 租户中唯一。 如果收到“`'http://acr-service-principal' already exists.`”错误，请为服务主体指定另一名称。

如果需要授予其他权限，可以选择修改 `--role`az ad sp create-for-rbac[ 命令中的 ][az-ad-sp-create-for-rbac] 值。

运行脚本后，请记下服务主体的 **ID** 和**密码**。 获得其凭据后，可以配置应用程序和服务使其作为服务主体对容器注册表进行身份验证。

<!-- URL is D:\gitrep\azure-docs-cli-python-samples\container-registry\service-principal-create\service-principal-create.sh-->

```azurecli
#!/bin/bash

# Modify for your environment.
# ACR_NAME: The name of your Azure Container Registry
# SERVICE_PRINCIPAL_NAME: Must be unique within your AD tenant
ACR_NAME=<container-registry-name>
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Obtain the full registry ID for subsequent command args
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create the service principal with rights scoped to the registry.
# Default permissions are for docker pull access. Modify the '--role'
# argument value as desired:
# reader:      pull only
# contributor: push and pull
# owner:       push, pull, and assign roles
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --scopes $ACR_REGISTRY_ID --role reader --query password --output tsv)
SP_APP_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output the service principal's credentials; use these in your services and
# applications to authenticate to the container registry.
echo "Service principal ID: $SP_APP_ID"
echo "Service principal password: $SP_PASSWD"
```

## <a name="use-an-existing-service-principal"></a>使用现有的服务主体

若要向现有服务主体授予注册表访问权限，必须为服务主体分配新角色。 与创建新的服务主体一样，可以授予“拉取”、“推送和拉取”以及“所有者”访问权限。

以下脚本使用 [az role assignment create][az-role-assignment-create] 命令向  *变量中指定的服务主体授予“拉取”* `SERVICE_PRINCIPAL_ID`权限。 如果要授予不同的访问级别，请调整 `--role` 值。

<!-- URL is D:\gitrep\azure-docs-cli-python-samples\container-registry\service-principal-assign-role\service-principal-assign-role.sh-->

```azurecli
#!/bin/bash

# Modify for your environment. The ACR_NAME is the name of your Azure Container
# Registry, and the SERVICE_PRINCIPAL_ID is the service principal's 'appId' or
# one of its 'servicePrincipalNames' values.
ACR_NAME=mycontainerregistry
SERVICE_PRINCIPAL_ID=<service-principal-ID>

# Populate value required for subsequent command args
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Assign the desired role to the service principal. Modify the '--role' argument
# value as desired:
# reader:      pull only
# contributor: push and pull
# owner:       push, pull, and assign roles
az role assignment create --assignee $SERVICE_PRINCIPAL_ID --scope $ACR_REGISTRY_ID --role reader
```

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: https://docs.azure.cn/zh-cn/cli/ad/sp?view=azure-cli-latest#az_ad_sp_create_for_rbac
[az-role-assignment-create]: https://docs.azure.cn/zh-cn/cli/role/assignment?view=azure-cli-latest#az_role_assignment_create

<!-- Update_Description: wording update, update meta properties -->