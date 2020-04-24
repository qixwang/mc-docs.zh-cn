---
title: 创建模板 - Visual Studio Code
description: 使用 Visual Studio Code 和可在资源管理器模板上运行的 Azure 资源管理器工具扩展。
author: rockboyfor
origin.date: 03/04/2019
ms.date: 01/06/2020
ms.topic: quickstart
ms.author: v-yeche
ms.openlocfilehash: d4607fdab651653690e0ba0ce1918b9a9385d301
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79291257"
---
# <a name="quickstart-create-azure-resource-manager-templates-by-using-visual-studio-code"></a>快速入门：使用 Visual Studio Code 创建 Azure 资源管理器模板

了解如何使用 Visual Studio Code 和 Azure 资源管理器工具扩展创建和编辑 Azure 资源管理器模板。 可以在 Visual Studio Code 中不使用扩展创建资源管理器模板，但是该扩展提供自动完成选项，可以简化模板开发。 若要了解与部署和管理 Azure 解决方案关联的概念，请参阅[模板部署概述](overview.md)。

在本快速入门中，你将部署存储帐户：

![资源管理器模板快速入门 visual studio 代码关系图](./media/quickstart-create-templates-use-visual-studio-code/resource-manager-template-quickstart-vscode-diagram.png)

如果没有 Azure 订阅，请在开始前[创建一个试用帐户](https://www.azure.cn/pricing/1rmb-trial/)。

## <a name="prerequisites"></a>必备条件

若要完成本文，需要做好以下准备：

- [Visual Studio Code](https://code.visualstudio.com/)。
- 资源管理器工具扩展。 若要安装此扩展，请使用以下步骤：

    1. 打开 Visual Studio Code。
    2. 按 **CTRL+SHIFT+X** 打开“扩展”窗格
    3. 搜索“Azure 资源管理器工具”，然后选择“安装”。  
    4. 选择“重新加载”完成扩展安装  。

## <a name="open-a-quickstart-template"></a>打开快速入门模板

无需从头开始创建模板，可以通过 [Azure 快速入门模板](https://github.com/Azure/azure-quickstart-templates/)打开一个模板。 Azure 快速入门模板是资源管理器模板的存储库。

本快速入门中使用的模板称为[创建标准存储帐户](https://github.com/Azure/azure-quickstart-templates/tree/master/101-storage-account-create/)。 该模板定义 Azure 存储帐户资源。

1. 在 Visual Studio Code 中，选择“文件” **“打开文件”。** > 
2. 在“文件名”中粘贴以下 URL： 

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

3. 选择“打开”以打开该文件。 
4. 选择“文件” **“另存为”，将该文件作为** azuredeploy.json> 保存到本地计算机。  

## <a name="edit-the-template"></a>编辑模板

若要体验如何使用 Visual Studio Code 来编辑模板，请将额外的一个元素添加到 `outputs` 节以显示存储 URI。

1. 将额外的一个输出添加到导出的模板：

    ```json
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
    }
    ```

    完成后，outputs 节如下所示：

    ```json
    "outputs": {
      "storageAccountName": {
        "type": "string",
        "value": "[variables('storageAccountName')]"
      },
      "storageUri": {
        "type": "string",
        "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
      }
    }
    ```

    如果在 Visual Studio Code 中复制并粘贴了代码，请尝试重新键入 **value** 元素，以体验资源管理器工具扩展的 IntelliSense 功能。

    ![资源管理器模板 - Visual Studio Code - IntelliSense](./media/quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. 选择“文件” **“保存”以保存文件。** > 

## <a name="deploy-the-template"></a>部署模板

可通过多种方法来部署模板。  在本快速入门中，使用 Azure CLI 或 Azure PowerShell 部署模板。

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

<!--Not Available on Cloud Shell-->

1. 从 Azure 本地 shell 运行以下命令。 选择用于显示 PowerShell 代码或 CLI 代码的选项卡。

    # <a name="cli"></a>[CLI](#tab/CLI)
    ```azurecli
    az cloud set -n AzureChinaCloud
    az login
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the name for this deployment:" &&
    read deploymentName &&
    echo "Enter the location (i.e. chinaeast):" &&
    read location &&
    az group create --name $resourceGroupName --location $location &&
    az group deployment create --name $deploymentName --resource-group $resourceGroupName --template-file "azuredeploy.json"
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    Connect-AzAccount -Environment AzureChinaCloud
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $deploymentName = Read-Host -Prompt "Enter the name for this deployment"
    $location = Read-Host -Prompt "Enter the location (i.e. chinaeast)"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -Name $deploymentName -ResourceGroupName $resourceGroupName -TemplateFile "azuredeploy.json"
    ```

    ---

    如果将模板文件保存到了 **azuredeploy.json** 之外的其他文件中，其更新其名称。

    以下屏幕截图显示了一个示例部署：

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![Azure CLI 部署模板](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure PowerShell 部署模板](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template-powershell.png)

    ---

    输出部分的存储帐户名称和存储 URL 在屏幕截图上突出显示。 在下一步需要此存储帐户名称。

2. 运行以下 CLI 或 PowerShell 命令，列出新建的存储帐户：

    # <a name="cli"></a>[CLI](#tab/CLI)
    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the Storage Account name:" &&
    read storageAccountName &&
    az storage account show --resource-group $resourceGroupName --name $storageAccountName
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $storageAccountName = Read-Host -Prompt "Enter the Storage Account name"
    Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
    ```

    ---

若要了解有关使用 Azure 存储帐户的更多信息，请参阅[快速入门：使用 Azure 门户上载、下载和列出 blob](../../storage/blobs/storage-quickstart-blobs-portal.md)。

## <a name="clean-up-resources"></a>清理资源

不再需要 Azure 资源时，请通过删除资源组来清理部署的资源。

1. 在 Azure 门户上的左侧菜单中选择“资源组”  。
2. 在“按名称筛选”字段中输入资源组名称。 
3. 选择资源组名称。  应会看到，该资源组中总共有六个资源。
4. 在顶部菜单中选择“删除资源组”。 

## <a name="next-steps"></a>后续步骤

本快速入门的主要关注点是如何使用 Visual Studio Code 来编辑 Azure 快速入门模板中的现有模板。 此外还介绍了如何通过本地电脑使用 CLI 或 PowerShell 部署模板。 Azure 快速入门模板中的模板可能并未提供你所需的一切。若要详细了解模板开发，请参阅新的初学者教程系列：

<!--Not Available on Cloud Shell-->

> [!div class="nextstepaction"]
> [初学者教程](./template-tutorial-create-first-template.md)

<!-- Update_Description: update meta properties, wording update, update link -->
