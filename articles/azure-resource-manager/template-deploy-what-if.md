---
title: 模板部署 what-if（预览版）
description: 在部署 Azure 资源管理器模板之前确定资源将会发生的更改。
author: rockboyfor
ms.topic: conceptual
origin.date: 11/20/2019
ms.date: 12/09/2019
ms.author: v-yeche
ms.openlocfilehash: 65214025b2398ac695561af5e793833b52b63021
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74884912"
---
<!--MOONCAKE: We submit the private preview request on https://aka.ms/armtemplatepreviews-->
<!--Wait for reply-->
# <a name="resource-manager-template-deployment-what-if-operation-preview"></a>资源管理器模板部署 what-if 操作（预览版）

在部署模板之前，你可能想要预览将会发生的更改。 Azure 资源管理器提供 what-if（假设）操作，让你在部署模板时了解资源发生的更改。 what-if 操作不会对现有资源进行任何更改， 而是预测在部署指定的模板时发生的更改。

> [!NOTE]
> what-if 操作目前以预览版提供。 若要使用此操作，必须[注册预览版](https://aka.ms/armtemplatepreviews)。 在预览版中，结果有时可能会显示资源将发生更改，但实际上并不会发生更改。 我们正在努力减少这些问题，但需要大家的帮助。 请在 [https://aka.ms/armwhatifissues](https://aka.ms/armwhatifissues) 上报告这些问题。

可以通过 `New-AzDeploymentWhatIf` PowerShell 命令或[部署 - What If](https://docs.microsoft.com/rest/api/resources/deployments/whatif) REST 操作来使用 what-if 操作。

在 PowerShell 中，输出如下所示：

![资源管理器模板部署 what-if 操作 fullresourcepayloads 和更改类型](./media/template-deploy-what-if/resource-manager-deployment-whatif-change-types.png)

## <a name="change-types"></a>更改类型

what-if 操作列出六种不同的更改类型：

- **创建**：资源当前不存在，但已在模板中定义。 将创建该资源。

- **删除**：仅当为部署使用[完整模式](deployment-modes.md)时，此更改类型才适用。 资源存在，但未在模板中定义。 使用完整模式时，将删除该资源。 此更改类型仅包括[支持完整模式删除](complete-mode-deletion.md)的资源。

- **忽略**：资源存在，但未在模板中定义。 不会部署或修改资源。

- **无更改**：资源存在，且已在模板中定义。 将重新部署资源，但资源的属性不会更改。 当 [ResultFormat](#result-format) 设置为 `FullResourcePayloads`（默认值）时，将返回此更改类型。

- **修改**：资源存在，且已在模板中定义。 将重新部署资源，且资源的属性会更改。 当 [ResultFormat](#result-format) 设置为 `FullResourcePayloads`（默认值）时，将返回此更改类型。

- **部署**：资源存在，且已在模板中定义。 将重新部署资源。 资源的属性可能会更改，也可能不会更改。 当没有足够的信息来确定是否有任何属性发生更改时，操作将返回此更改类型。 仅当 [ResultFormat](#result-format) 设置为 `ResourceIdOnly` 时，才会看到此状况。

## <a name="deployment-scope"></a>部署范围

可将 what-if 操作用于订阅或资源组级别的部署。 可以使用 `-ScopeType` 参数设置部署范围。 接受的值为 `Subscription` 和 `ResourceGroup`。 本文演示资源组部署。

若要了解订阅级部署，请参阅[在订阅级别创建资源组和资源](deploy-to-subscription.md#)。

## <a name="result-format"></a>结果格式

可以控制返回的有关所预测更改的详细级别。 将 `ResultFormat` 参数设置为 `FullResourcePayloads` 可获取会发生更改的资源列表，以及会发生更改的属性的详细信息。 将 `ResultFormat` 参数设置为 `ResourceIdOnly` 可获取会发生更改的资源列表。 默认值为 `FullResourcePayloads`。  

以下屏幕截图显示了两种不同的输出格式：

- 完整资源有效负载

    ![资源管理器模板部署 what-if 操作 fullresourcepayloads 输出](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-fullresourcepayload.png)

- 仅限资源 ID

    ![资源管理器模板部署 what-if 操作 resourceidonly 输出](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-resourceidonly.png)

## <a name="run-what-if-operation"></a>运行 what-if 操作

### <a name="set-up-environment"></a>设置环境

为了了解 what-if 的工作原理，让我们运行一些测试。 首先，通过[用于创建存储帐户的 Azure 快速入门模板](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json)部署一个模板。 默认存储帐户类型为 `Standard_LRS`。 将使用此存储帐户来测试 what-if 如何报告更改。

```powershell
New-AzResourceGroup `
  -Name ExampleGroup `
  -Location chinaeast
New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

### <a name="test-modification"></a>测试修改

部署完成后，即可测试 what-if 操作。 运行 what-if 命令，但同时将存储帐户类型更改为 `Standard_GRS`。

```powershell
New-AzDeploymentWhatIf `
  -ScopeType ResourceGroup `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" `
  -storageAccountType Standard_GRS
```

what-if 输出类似于：

![资源管理器模板部署 what-if 操作输出](./media/template-deploy-what-if/resource-manager-deployment-whatif-output.png)

请注意，输出顶部的颜色用于指示更改类型。

输出的底部显示 SKU 名称（存储帐户类型）将从 **Standard_LRS** 更改为 **Standard_GRS**。

列出为已删除的某些属性实际上不会更改。 在上图中，这些属性是 accessTier、encryption.keySource 和该节中的其他属性。 当属性不在模板中时，它们可能被错误地报告为已删除，但在部署过程中会自动设置为默认值。 此结果在 what-if 响应中被视为“干扰信息”。 最终部署的资源将具有为属性设置的值。 当 what-if 操作成熟时，将从结果中筛选出这些属性。

### <a name="test-deletion"></a>测试删除

what-if 操作支持使用[部署模式](deployment-modes.md)。 设置为完整模式时，将删除不在模板中的资源。 以下示例部署一个处于完整模式的[未定义任何资源的模板](https://github.com/Azure/azure-docs-json-samples/blob/master/empty-template/azuredeploy.json)。

```powershell
New-AzDeploymentWhatIf `
  -ScopeType ResourceGroup `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/empty-template/azuredeploy.json" `
  -Mode Complete
```

由于该模板中未定义任何资源，且部署模式设置为“完整”，因此将删除存储帐户。

![资源管理器模板部署 what-if 操作输出 - 完整部署模式](./media/template-deploy-what-if/resource-manager-deployment-whatif-output-mode-complete.png)

请务必记住，what-if 不会进行任何实际的更改。 存储帐户仍在资源组中。

## <a name="next-steps"></a>后续步骤

- 如果发现 what-if 预览版提供了错误的结果，请在 [https://aka.ms/armwhatifissues](https://aka.ms/armwhatifissues) 上报告问题。
- 若要使用 Azure PowerShell 部署模板，请参阅[使用资源管理器模板和 Azure PowerShell 部署资源](resource-group-template-deploy.md)。
- 若要使用 REST 部署模板，请参阅[使用资源管理器模板和资源管理器 REST API 部署资源](resource-group-template-deploy-rest.md)。
- 若要在出错时回滚到成功的部署，请参阅[出错时回滚到成功的部署](rollback-on-error.md)。

<!-- Update_Description: new article about template deploy what if -->
<!--NEW.date: 11/25/2019-->