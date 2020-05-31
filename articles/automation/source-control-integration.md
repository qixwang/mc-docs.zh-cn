---
title: Azure 自动化中的源代码管理集成
description: 本文介绍 Azure 自动化中源代码管理与 GitHub 的集成。
services: automation
ms.subservice: process-automation
origin.date: 12/10/2019
ms.date: 04/20/2020
ms.topic: conceptual
ms.openlocfilehash: 40f5082dedbf90a5d3efe2efcbb52542311a86ee
ms.sourcegitcommit: be0a8e909fbce6b1b09699a721268f2fc7eb89de
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84199651"
---
# <a name="source-control-integration-in-azure-automation"></a>Azure 自动化中的源代码管理集成

 源代码管理与 Azure 自动化的集成支持从源代码管理存储库进行单向同步。 通过源代码管理，可以使用 GitHub 或 Azure Repos 源代码管理存储库中的脚本使自动化帐户中的 Runbook 保持最新。 借助此功能，可以轻松地将已在开发环境中测试过的代码提升到生产自动化帐户。
 
 可以通过源代码管理集成轻松地与团队协作、跟踪更改，以及回退到旧版 Runbook。 例如，通过源代码管理可以将源代码管理中的不同分支同步到开发、测试和生产自动化帐户。 

>[!NOTE]
>本文进行了更新，以便使用新的 Azure PowerShell Az 模块。 你仍然可以使用 AzureRM 模块，至少在 2020 年 12 月之前，它将继续接收 bug 修补程序。 若要详细了解新的 Az 模块和 AzureRM 兼容性，请参阅[新 Azure Powershell Az 模块简介](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0)。 有关适用于混合 Runbook 辅助角色的 Az 模块安装说明，请参阅安装 [Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)。

## <a name="source-control-types"></a>源代码管理类型

Azure 自动化支持三种类型的源代码管理：

* GitHub
* Azure Repos (Git)
* Azure Repos (TFVC)

## <a name="prerequisites"></a>先决条件

* 源代码管理存储库（GitHub 或 Azure Repos）
* 一个[运行方式帐户](manage-runas-account.md)

> [!NOTE]
> 源代码管理同步作业在用户的自动化帐户下运行，并且按与其他自动化作业相同的费率计费。

## <a name="configuring-source-control"></a>配置源代码管理

本部分介绍如何为自动化帐户配置源代码管理。 可以使用 Azure 门户或 PowerShell。

### <a name="configure-source-control-in-azure-portal"></a>在 Azure 门户中配置源代码管理

在 Azure 门户中使用以下过程配置源代码管理。

1. 在自动化帐户中选择“源代码管理”，然后单击“添加”。  

    ![选择“源代码管理”](./media/source-control-integration/select-source-control.png)

2. 选择“源代码管理类型”，然后单击“身份验证”。   

3. 此时会打开一个提示登录的浏览器窗口。 按照提示完成身份验证。

4. 在“源代码管理摘要”页上，使用字段填充下面定义的源代码管理属性。 完成后单击“保存”  。 

    |属性  |说明  |
    |---------|---------|
    |源代码管理名称     | 源代码管理的易记名称。 此名称只能包含字母和数字。        |
    |源代码管理类型     | 源代码管理机制的类型。 可用选项包括：</br> * GitHub</br>* Azure Repos (Git)</br> * Azure Repos (TFVC)        |
    |存储库     | 存储库或项目的名称。 将检索前 200 个存储库。 若要搜索存储库，请在字段中键入名称，然后单击“在 GitHub 中搜索”。 |
    |分支     | 从中提取源文件的分支。 分支目标确定不适用于 TFVC 源代码管理类型。          |
    |文件夹路径     | 包含要同步的 Runbook 的文件夹，例如 /Runbooks。  仅同步指定文件夹中的 Runbook。 不支持递归。        |
    |自动同步<sup>1</sup>     | 该设置用于在源代码管理存储库中提交时打开或关闭自动同步。        |
    |发布 Runbook     | 如果在从源代码管理完成同步后要自动发布 Runbook，请设置为“打开”；否则请设置为“关闭”。           |
    |说明     | 用于指定有关源代码管理的其他详细信息的文本。        |

    <sup>1</sup> 只有项目管理员才能在配置源代码管理与 Azure 存储库的集成时启用自动同步。

   ![源代码管理摘要](./media/source-control-integration/source-control-summary.png)

> [!NOTE]
> 你的源代码管理存储库登录名可能不同于 Azure 门户的登录名。 配置源代码管理时，请确保使用源代码管理存储库的正确帐户登录。 如果有疑问，请在浏览器中打开新的标签页，从 dev.azure.com  、visualstudio.com  或 github.com  注销，然后尝试重新连接到源代码管理。

### <a name="configure-source-control-in-powershell"></a>在 PowerShell 中配置源代码管理

也可以使用 PowerShell 在 Azure 自动化中配置源代码管理。 若要使用 PowerShell cmdlet 执行此操作，需要个人访问令牌 (PAT)。 使用 [New-AzAutomationSourceControl](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationsourcecontrol?view=azps-3.5.0) cmdlet 创建源代码管理连接。 此 cmdlet 需要 PAT 的安全字符串。 若要了解如何创建安全字符串，请参阅 [ConvertTo-SecureString](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-6)。

以下附属部分演示如何在 PowerShell 中创建 GitHub、Azure Repos (Git) 和 Azure Repos (TFVC) 的源代码管理连接。 

#### <a name="create-source-control-connection-for-github"></a>创建 GitHub 的源代码管理连接

```powershell
New-AzAutomationSourceControl -Name SCGitHub -RepoUrl https://github.com/<accountname>/<reponame>.git -SourceType GitHub -FolderPath "/MyRunbooks" -Branch master -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

#### <a name="create-source-control-connection-for-azure-repos-git"></a>创建 Azure Repos (Git) 的源代码管理连接

> [!NOTE]
> Azure Repos (Git) 使用一个访问“dev.azure.com”  （而不是旧格式中使用的“visualstudio.com”  ）的 URL。 旧的 URL 格式 `https://<accountname>.visualstudio.com/<projectname>/_git/<repositoryname>` 已弃用，但仍受支持。 新格式为首选格式。


```powershell
New-AzAutomationSourceControl -Name SCReposGit -RepoUrl https://dev.azure.com/<accountname>/<adoprojectname>/_git/<repositoryname> -SourceType VsoGit -AccessToken <secureStringofPAT> -Branch master -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

#### <a name="create-source-control-connection-for-azure-repos-tfvc"></a>创建 Azure Repos (TFVC) 的源代码管理连接

> [!NOTE]
> Azure Repos (TFVC) 使用一个访问“dev.azure.com”  （而不是旧格式中使用的“visualstudio.com”  ）的 URL。 旧的 URL 格式 `https://<accountname>.visualstudio.com/<projectname>/_versionControl` 已弃用，但仍受支持。 新格式为首选格式。

```powershell
New-AzAutomationSourceControl -Name SCReposTFVC -RepoUrl https://dev.azure.com/<accountname>/<adoprojectname>/_git/<repositoryname> -SourceType VsoTfvc -AccessToken <secureStringofPAT> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName> -FolderPath "/Runbooks"
```

#### <a name="personal-access-token-pat-permissions"></a>个人访问令牌 (PAT) 权限

源代码管理需要一些最低的 PAT 权限。 以下附属部分包含 GitHub 和 Azure Repos 所需的最低权限。

##### <a name="minimum-pat-permissions-for-github"></a>GitHub 所需的最低 PAT 权限

下表定义了 GitHub 所需的最低 PAT 权限。 有关在 GitHub 中创建 PAT 的详细信息，请参阅[为命令行创建个人访问令牌](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)。

|作用域  |说明  |
|---------|---------|
|**`repo`**     |         |
|`repo:status`     | 访问提交状态         |
|`repo_deployment`      | 访问部署状态         |
|`public_repo`     | 访问公共存储库         |
|**`admin:repo_hook`**     |         |
|`write:repo_hook`     | 写入存储库挂钩         |
|`read:repo_hook`|读取存储库挂钩|

##### <a name="minimum-pat-permissions-for-azure-repos"></a>Azure Repos 所需的最低 PAT 权限

下表列出了 Azure Repos 所需的最低 PAT 权限。 有关在 Azure Repos 中创建 PAT 的详细信息，请参阅[使用个人访问令牌对访问进行身份验证](https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate)。

| 作用域  |  访问类型  |
|---------| ----------|
| `Code`      | 读取  |
| `Project and team` | 读取 |
| `Identity` | 读取     |
| `User profile` | 读取     |
| `Work items` | 读取    |
| `Service connections` | 读取、查询和管理<sup>1</sup>    |

<sup>1</sup> 只有在启用了自动同步时才需要“`Service connections`”权限。

## <a name="synchronizing"></a>正在同步

遵循以下步骤来与源代码管理同步。 

1. 从“源代码管理”页上的表格中选择源。 

2. 单击“开始同步”  以开始同步过程。 

3. 单击“同步作业”选项卡，查看当前或以前的同步作业的状态。  

4. 在“源代码管理”下拉菜单中，选择一个源代码管理机制。 

    ![同步状态](./media/source-control-integration/sync-status.png)

5. 单击某个作业可以查看作业输出。 以下示例是源代码管理同步作业的输出。

    ```output
    ===================================================================

    Azure Automation Source Control.
    Supported runbooks to sync: PowerShell Workflow, PowerShell Scripts, DSC Configurations, Graphical, and Python 2.

    Setting AzureRmEnvironment.

    Getting AzureRunAsConnection.

    Logging in to Azure...

    Source control information for syncing:

    [Url = https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl] [FolderPath = /Runbooks]

    Verifying url: https://ContosoExample.visualstudio.com/ContosoFinanceTFVCExample/_versionControl

    Connecting to VSTS...

    Source Control Sync Summary:

    2 files synced:
     - ExampleRunbook1.ps1
     - ExampleRunbook2.ps1

    ==================================================================

    ```

6. 在“源代码管理同步作业摘要”页上选择“所有日志”可以查看其他日志。  这些附加的日志条目可帮助你排查使用源代码管理时可能遇到的问题。

## <a name="disconnecting-source-control"></a>断开连接源代码管理

若要从源代码管理存储库断开连接：

1. 在自动化帐户中的“帐户设置”下，打开“源代码管理”。  

2. 选择要删除的源代码管理机制。 

3. 在“源代码管理摘要”页上，单击“删除”。 

## <a name="handling-encoding-issues"></a>处理编码问题

如果有多个人正在使用不同的编辑器编辑你的源代码管理存储库中的 Runbook，可能会出现编码问题。 有关此情形的详细信息，请参阅[编码问题的常见原因](https://docs.microsoft.com/powershell/scripting/components/vscode/understanding-file-encoding#common-causes-of-encoding-issues)。

## <a name="updating-the-pat"></a>更新 PAT

目前，无法使用 Azure 门户更新源代码管理中的 PAT。 在 PAT 过期或撤销后，可通过以下方式之一使用新的访问令牌来更新源代码管理：

* 使用 [REST API](https://docs.microsoft.com/rest/api/automation/sourcecontrol/update)。
* 使用 [Update-AzAutomationSourceControl](https://docs.microsoft.com/powershell/module/az.automation/update-azautomationsourcecontrol) cmdlet。

## <a name="next-steps"></a>后续步骤

若要详细了解 Runbook 类型、其优点和限制，请参阅 [Azure 自动化 Runbook 类型](automation-runbook-types.md)。
