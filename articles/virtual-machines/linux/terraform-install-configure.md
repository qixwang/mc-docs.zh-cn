---
title: 安装并配置 Terraform 以预配 Azure 资源
description: 了解如何安装和配置用于创建 Azure 资源的 Terraform
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rockboyfor
manager: digimobile
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
origin.date: 09/20/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.openlocfilehash: e171d3fe2858665930288f7fdf470e0c8fa3f8d0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428008"
---
# <a name="install-and-configure-terraform-to-provision-azure-resources"></a>安装并配置 Terraform 以预配 Azure 资源

借助 Terraform，可以轻松使用[简单模板语言](https://www.terraform.io/docs/configuration/syntax.html)来定义、预览和部署云基础结构。 本文介绍使用 Terraform 在 Azure 中预配资源的必要步骤。

> [!NOTE]
> 要获得特定于 Terraform 的支持，请直接使用以下社区通道之一联系 Terraform：
>
>   • 社区门户的 [Terraform 部分](https://discuss.hashicorp.com/c/terraform-core)包含问题、用例和有用模式。
>
>   • 有关与提供程序相关的问题，请访问社区门户的 [Terraform 提供程序](https://discuss.hashicorp.com/c/terraform-providers)部分。

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

<!--Not Available on [Cloud Shell](/terraform/terraform-cloud-shell)-->

## <a name="install-terraform"></a>安装 Terraform

若要安装 Terraform，请将适用于操作系统的程序包[下载](https://www.terraform.io/downloads.html)到单独安装目录。 下载内容包含一个可执行文件，应为其定义全局路径。 有关如何在 Linux 和 Mac 上设置路径的说明，请转到[此网页](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux)。 有关如何在 Windows 上设置路径的说明，请转到[此网页](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)。

使用 `terraform` 目录验证路径配置。 将显示可用 Terraform 选项的列表，如以下示例输出所示：

```console
azureuser@Azure:~$ terraform
Usage: terraform [--version] [--help] <command> [args]
```

## <a name="set-up-terraform-access-to-azure"></a><a name="set-up-terraform-access-to-azure"></a>设置 Terraform 对 Azure 的访问权限

要使 Terraform 能够将资源预配到 Azure，请创建 [Azure AD 服务主体](https://docs.azure.cn/cli/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)。 服务主体允许你的 Terraform 脚本在 Azure 订阅中预配资源。

如果有多个 Azure 订阅，请先使用 [az account list](https://docs.azure.cn/cli/account?view=azure-cli-latest#az-account-list) 查询帐户，以获取订阅 ID 和租户 ID 值列表：

[!INCLUDE [azure-cli-2-azurechinacloud-environment-parameter](../../../includes/azure-cli-2-azurechinacloud-environment-parameter.md)]

```azurecli
az account list --query "[].{name:name, subscriptionId:id, tenantId:tenantId}"
```

若要使用所选订阅，请使用 [az account set](https://docs.azure.cn/cli/account?view=azure-cli-latest#az-account-set) 为此会话设置订阅。 设置 `SUBSCRIPTION_ID` 环境变量，用于保存从要使用的订阅返回的 `id` 字段值：

```azurecli
az account set --subscription="${SUBSCRIPTION_ID}"
```

现在，可以创建一个服务主体以与 Terraform 一起使用。 使用 [az ad sp create-for-rbac](https://docs.azure.cn/cli/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac)，并将“范围”设置为你的订阅，如下所示  ：

```azurecli
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

会返回你的 `appId`、`password`、`sp_name` 和 `tenant`。 记下 `appId` 和 `password`。

## <a name="configure-terraform-environment-variables"></a>配置 Terraform 环境变量

若要配置 Terraform 以使用 Azure AD 服务主体，请设置以下环境变量，然后由 [Azure Terraform 模块](https://registry.terraform.io/modules/Azure)使用这些环境变量。 如果使用的是 Azure 云而不是 Azure 公共域，则还可设置环境。

- `ARM_SUBSCRIPTION_ID`
- `ARM_CLIENT_ID`
- `ARM_CLIENT_SECRET`
- `ARM_TENANT_ID`
- `ARM_ENVIRONMENT`

<!-- MOONCAKE: Configure the ARM_ENVIRONMENT=china -->

可以使用以下示例 shell 脚本设置这些变量：

```bash
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id

# Not needed for public, required for usgovernment, german, china
export ARM_ENVIRONMENT=china
```
<!-- MOONCAKE: Configure the ARM_ENVIRONMENT=china -->

## <a name="run-a-sample-script"></a>运行示例脚本

在空白目录下创建文件 `test.tf`，并粘贴到以下脚本中。

```hcl
provider "azurerm" {
}
resource "azurerm_resource_group" "rg" {
        name = "testResourceGroup"
        location = "chinanorth"
}
```

保存该文件，然后初始化 Terraform 部署。 此步骤将下载创建 Azure 资源组所需的 Azure 模块。

```bash
terraform init
```

输出类似于以下示例：

```console
* provider.azurerm: version = "~> 0.3"

Terraform has been successfully initialized!
```

可以使用 `terraform plan` 预览 Terraform 脚本要完成的操作。 准备好创建资源组时，请按如下所示应用 Terraform 计划：

```bash
terraform apply
```

输出类似于以下示例：

```console
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.rg
      id:       <computed>
      location: "chinanorth"
      name:     "testResourceGroup"
      tags.%:   <computed>

azurerm_resource_group.rg: Creating...
  location: "" => "chinanorth"
  name:     "" => "testResourceGroup"
  tags.%:   "" => "<computed>"
azurerm_resource_group.rg: Creation complete after 1s
```

## <a name="next-steps"></a>后续步骤

在本文中，你安装了 Terraform 或使用本地 Shell 配置了 Azure 凭据并开始在 Azure 订阅中创建资源。 若要在 Azure 中创建更完整的 Terraform 部署，请参阅以下文章：

> [!div class="nextstepaction"]
> [使用 Terraform 创建 Azure VM](terraform-create-complete-vm.md)

<!-- Update_Description: update meta properties, update link, wording update -->