---
title: 模板部署 what-if（预览版）
description: 在部署 Azure 资源管理器模板之前确定资源将会发生的更改。
author: rockboyfor
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: v-yeche
ms.openlocfilehash: 404984828963ae1755a66e27b2dce4037a155969
ms.sourcegitcommit: b469d275694fb86bbe37a21227e24019043b9e88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82596084"
---
<!--PRIVATE VIEW NOT SUIT FOR OUTSIDE OF MICROSOFT-->
<!--RELEASE BEFORE CONFIRM-->
<!--MOONCAKE: We submit the private preview request on https://aka.ms/armtemplatepreviews-->
<!--Wait for reply-->
# <a name="arm-template-deployment-what-if-operation-preview"></a>ARM 模板部署 what-if 操作（预览版）

在部署 Azure 资源管理器 (ARM) 模板之前，可能需要预览将要进行的更改。 Azure 资源管理器提供 what-if（假设）操作，让你在部署模板时了解资源发生的更改。 what-if 操作不会对现有资源进行任何更改， 而是预测在部署指定的模板时发生的更改。

> [!NOTE]
> what-if 操作目前以预览版提供。 在预览版中，结果有时可能会显示资源将发生更改，但实际上并不会发生更改。 我们正在努力减少这些问题，但需要大家的帮助。 请在 [https://aka.ms/whatifissues](https://aka.ms/whatifissues) 上报告这些问题。

可将 what-if 操作与 PowerShell 命令或 REST API 操作配合使用。

## <a name="install-powershell-module"></a>安装 PowerShell 模块

若要在 PowerShell 中使用 what-if，请从 PowerShell 库安装 Az.Resources 模块预览版。

### <a name="install-preview-version"></a>安装预览版

若要安装预览版模块，请使用以下命令：

```powershell
Install-Module Az.Resources -RequiredVersion 1.12.1-preview -AllowPrerelease
```

### <a name="uninstall-alpha-version"></a>卸载 alpha 版本

如果以前安装了 alpha 版本的 what-if 模块，请卸载该模块。 alpha 版本仅适用于注册了抢鲜预览版的用户。 如果未安装该预览版，则可跳过此部分。

1. 以管理员身份运行 PowerShell
1. 检查安装的 Az.Resources 模块版本。

   ```powershell
   Get-InstalledModule -Name Az.Resources -AllVersions | select Name,Version
   ```

1. 如果已安装版本的版本号格式为 2.x.x-alpha，请卸载该版本  。

   ```powershell
   Uninstall-Module Az.Resources -RequiredVersion 2.0.1-alpha5 -AllowPrerelease
   ```

1. 取消注册用于安装预览版的 what-if 存储库。

   ```powershell
   Unregister-PSRepository -Name WhatIfRepository
   ```

现在可以使用 what-if 了。

## <a name="see-results"></a>查看结果

在 PowerShell 中，输出的结果进行了颜色编码，方便你查看不同类型的更改。

![资源管理器模板部署 what-if 操作 fullresourcepayloads 和更改类型](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

文本输出如下：

```powershell
Resource and property changes are indicated with these symbols:
  - Delete
  + Create
  ~ Modify

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
    - tags.Owner: "Team A"
    ~ properties.addressSpace.addressPrefixes: [
      - 0: "10.0.0.0/16"
      + 0: "10.0.0.0/15"
      ]
    ~ properties.subnets: [
      - 0:

          name:                     "subnet001"
          properties.addressPrefix: "10.0.0.0/24"

      ]

Resource changes: 1 to modify.
```

## <a name="what-if-commands"></a>what-if 命令

可以使用 Azure PowerShell 或 Azure REST API 执行 what-if 操作。

### <a name="azure-powershell"></a>Azure PowerShell

若要在部署模板之前预览一下所做的更改，请将 `-Whatif` 开关参数添加到部署命令。

* 对于资源组部署，请使用 `New-AzResourceGroupDeployment -Whatif`
* 对于订阅级别的部署，请使用 `New-AzSubscriptionDeployment -Whatif` 和 `New-AzDeployment -Whatif`

也可使用 `-Confirm` 开关参数来预览所做的更改，让系统显示是否继续部署的提示。

* 对于资源组部署，请使用 `New-AzResourceGroupDeployment -Confirm`
* 对于订阅级别的部署，请使用 `New-AzSubscriptionDeployment -Confirm` 和 `New-AzDeployment -Confirm`

上述命令返回适用于手动检查的文本摘要。 若要获取一个适用于以编程方式在其中检查更改的对象，请使用以下命令：

* 对于资源组部署，请使用 `$results = Get-AzResourceGroupDeploymentWhatIfResult`
* 对于订阅级别的部署，请使用 `$results = Get-AzSubscriptionDeploymentWhatIfResult` 或 `$results = Get-AzDeploymentWhatIfResult`

### <a name="azure-rest-api"></a>Azure REST API

对于 REST API，请使用：

* 对于资源组部署，请使用[部署 - What If](https://docs.microsoft.com/rest/api/resources/deployments/whatif)
* 对于订阅级别的部署，请使用[部署 - 订阅范围的 What If](https://docs.microsoft.com/rest/api/resources/deployments/whatifatsubscriptionscope)

## <a name="change-types"></a>更改类型

what-if 操作列出六种不同的更改类型：

- **创建**：资源当前不存在，但已在模板中定义。 将创建该资源。

- **删除**：仅当为部署使用[完整模式](deployment-modes.md)时，此更改类型才适用。 资源存在，但未在模板中定义。 使用完整模式时，将删除该资源。 此更改类型仅包括[支持完整模式删除](complete-mode-deletion.md)的资源。

- **忽略**：资源存在，但未在模板中定义。 不会部署或修改资源。

- **无更改**：资源存在，且已在模板中定义。 将重新部署资源，但资源的属性不会更改。 当 [ResultFormat](#result-format) 设置为 `FullResourcePayloads`（默认值）时，将返回此更改类型。

- **修改**：资源存在，且已在模板中定义。 将重新部署资源，且资源的属性会更改。 当 [ResultFormat](#result-format) 设置为 `FullResourcePayloads`（默认值）时，将返回此更改类型。

- **部署**：资源存在，且已在模板中定义。 将重新部署资源。 资源的属性可能会更改，也可能不会更改。 当没有足够的信息来确定是否有任何属性发生更改时，操作将返回此更改类型。 仅当 [ResultFormat](#result-format) 设置为 `ResourceIdOnly` 时，才会看到此状况。

## <a name="result-format"></a>结果格式

可以控制返回的有关所预测更改的详细级别。 在部署命令 (`New-Az*Deployment`) 中，请使用 -WhatIfResultFormat 参数  。 在编程对象命令 (`Get-Az*DeploymentWhatIf`) 中，请使用 ResultFormat 参数  。

将 format 参数设置为 FullResourcePayloads 可获取将会更改的资源的列表，以及将会更改的属性的详细信息  。 将 format 参数设置为 ResourceIdOnly 可获取将会更改的资源的列表  。 默认值为 FullResourcePayloads  。  

以下结果显示了两种不同的输出格式：

- 完整资源有效负载

  ```powershell
  Resource and property changes are indicated with these symbols:
    - Delete
    + Create
    ~ Modify

  The deployment will update the following scope:

  Scope: /subscriptions/./resourceGroups/ExampleGroup

    ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
      - tags.Owner: "Team A"
      ~ properties.addressSpace.addressPrefixes: [
        - 0: "10.0.0.0/16"
        + 0: "10.0.0.0/15"
        ]
      ~ properties.subnets: [
        - 0:

          name:                     "subnet001"
          properties.addressPrefix: "10.0.0.0/24"

        ]

  Resource changes: 1 to modify.
  ```

- 仅限资源 ID

  ```powershell
  Resource and property changes are indicated with this symbol:
    ! Deploy

  The deployment will update the following scope:

  Scope: /subscriptions/./resourceGroups/ExampleGroup

    ! Microsoft.Network/virtualNetworks/vnet-001

  Resource changes: 1 to deploy.
  ```

## <a name="run-what-if-operation"></a>运行 what-if 操作

### <a name="set-up-environment"></a>设置环境

为了了解 what-if 的工作原理，让我们运行一些测试。 首先，部署一个[用于创建虚拟网络的模板](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-before.json)。 将使用此虚拟网络来测试 what-if 如何报告更改。

```azurepowershell
New-AzResourceGroup `
  -Name ExampleGroup `
  -Location centralus
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-before.json"
```

### <a name="test-modification"></a>测试修改

部署完成后，即可测试 what-if 操作。 这一次，请部署一个[用于更改虚拟网络的模板](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/what-if/what-if-after.json)。 该模板中缺少一个原始标记，已删除了一个子网，并且已更改了地址前缀。

```azurepowershell
New-AzResourceGroupDeployment `
  -Whatif `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

what-if 输出类似于：

![资源管理器模板部署 what-if 操作输出](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

文本输出如下：

```powershell
Resource and property changes are indicated with these symbols:
  - Delete
  + Create
  ~ Modify

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  ~ Microsoft.Network/virtualNetworks/vnet-001 [2018-10-01]
    - tags.Owner: "Team A"
    ~ properties.addressSpace.addressPrefixes: [
      - 0: "10.0.0.0/16"
      + 0: "10.0.0.0/15"
      ]
    ~ properties.subnets: [
      - 0:

        name:                     "subnet001"
        properties.addressPrefix: "10.0.0.0/24"

      ]

Resource changes: 1 to modify.
```

请注意，输出顶部的颜色用于指示更改类型。

输出的底部显示了“已删除所有者”标记。 地址前缀已从 10.0.0.0/16 更改为 10.0.0.0/15。 已删除名为 subnet001 的子网。 请记住，并未实际部署这些更改。 如果部署该模板，则可预览会发生的更改。

列出为已删除的某些属性实际上不会更改。 当属性不在模板中时，它们可能被错误地报告为已删除，但在部署过程中会自动设置为默认值。 此结果在 what-if 响应中被视为“干扰信息”。 最终部署的资源将具有为属性设置的值。 当 what-if 操作成熟时，将从结果中筛选出这些属性。

## <a name="programmatically-evaluate-what-if-results"></a>以编程方式评估 what-if 结果

现在，让我们将命令设置为变量，以编程方式评估 what-if 结果。

```azurepowershell
$results = Get-AzResourceGroupDeploymentWhatIfResult `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"
```

可以看到每项更改的摘要。

```azurepowershell
foreach ($change in $results.Changes)
{
  $change.Delta
}
```

## <a name="confirm-deletion"></a>确认删除

what-if 操作支持使用[部署模式](deployment-modes.md)。 设置为完整模式时，将删除不在模板中的资源。 以下示例部署一个处于完整模式的[未定义任何资源的模板](https://github.com/Azure/azure-docs-json-samples/blob/master/empty-template/azuredeploy.json)。

若要在部署模板之前预览所做的更改，请在部署命令中使用 `-Confirm` 开关参数。 如果更改符合预期，请确认你想要完成此部署。

```azurepowershell
New-AzResourceGroupDeployment `
  -Confirm `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/empty-template/azuredeploy.json" `
  -Mode Complete
```

由于该模板中未定义任何资源，且部署模式设置为完成，因此会删除虚拟网络。

![资源管理器模板部署 what-if 操作输出 - 完整部署模式](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-mode-complete.png)

文本输出如下：

```powershell
Resource and property changes are indicated with this symbol:
  - Delete

The deployment will update the following scope:

Scope: /subscriptions/./resourceGroups/ExampleGroup

  - Microsoft.Network/virtualNetworks/vnet-001

      id:
"/subscriptions/./resourceGroups/ExampleGroup/providers/Microsoft.Network/virtualNet
works/vnet-001"
      location:        "centralus"
      name:            "vnet-001"
      tags.CostCenter: "12345"
      tags.Owner:      "Team A"
      type:            "Microsoft.Network/virtualNetworks"

Resource changes: 1 to delete.

Are you sure you want to execute the deployment?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

你会看到预期的更改，并且可以确认你想要运行此部署。

## <a name="next-steps"></a>后续步骤

- 如果发现 what-if 预览版提供了错误的结果，请在 [https://aka.ms/whatifissues](https://aka.ms/whatifissues) 上报告问题。
- 若要使用 Azure PowerShell 来部署模板，请参阅[使用 ARM 模板和 Azure PowerShell 来部署资源](deploy-powershell.md)。
- 若要使用 REST 来部署模板，请参阅[使用 ARM 模板和资源管理器 REST API 来部署资源](deploy-rest.md)。


<!-- Update_Description: update meta properties, wording update, update link -->
