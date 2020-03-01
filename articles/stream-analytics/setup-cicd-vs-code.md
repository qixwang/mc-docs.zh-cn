---
title: 使用 CI/CD npm 包部署 Azure 流分析作业
description: 本文介绍如何使用 Azure 流分析 CI/CD npm 包设置持续集成和部署过程。
services: stream-analytics
author: lingliw
ms.author: v-lingwu
manager: digimobile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
origin.date: 01/28/2020
ms.date: 2/6/2020
ms.openlocfilehash: 7358157bac634ecf877a2b4a6c4c6cf56779c45d
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78154620"
---
# <a name="deploy-an-azure-stream-analytics-job-using-cicd-npm-package"></a>使用 CI/CD npm 包部署 Azure 流分析作业 

可以使用 Azure 流分析 CI/CD npm 包为流分析作业设置持续的集成和部署过程。 本文介绍通常如何在任何 CI/CD 系统中使用 npm 包，以及使用 Azure Pipelines 进行部署的具体说明。

## <a name="build-the-vs-code-project"></a>生成 VS Code 项目

可使用 **asa-streamanalytics-cicd** npm 包启用 Azure 流分析作业的持续集成和部署。 npm 包提供了用于生成[流分析 Visual Studio Code 项目](quick-create-vs-code.md)的 Azure 资源管理器模板的工具。 它可以在 Windows、macOS 和 Linux 上使用，而无需安装 Visual Studio Code。

安装包后，请使用以下命令输出 Azure 资源管理器模板。 **scriptPath** 参数是项目中 **asaql** 文件的绝对路径。 请确保 asaproj.json 和 JobConfig.json 文件与脚本文件位于同一文件夹中。 如果未指定 **outputPath**，则模板将放置在项目的 **bin** 文件夹下的 **Deploy** 文件夹中。

```powershell
azure-streamanalytics-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```
示例（在 macOS 上）
```powershell
azure-streamanalytics-cicd build -scriptPath "/Users/roger/projects/samplejob/script.asaql" 
```

成功生成流分析 Visual Studio Code 项目后，会在 bin/[Debug/Retail]/Deploy 文件夹下生成以下两个 Azure 资源管理器模板文件  ： 

*  资源管理器模板文件

       [ProjectName].JobTemplate.json 

*  资源管理器参数文件

       [ProjectName].JobTemplate.parameters.json   

parameters.json 文件中的默认参数来自 Visual Studio Code 项目中的设置。 如果要部署到其他环境，请相应地替换参数。

> [!NOTE]
> 对于所有凭据，默认值均设置为 null。 部署到云之前，必须先设置这些值  。

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
深入了解如何[使用资源管理器模板文件和 Azure PowerShell 进行部署](/azure-resource-manager/resource-group-template-deploy)。

若要将 Azure Data Lake Store Gen1 的托管标识用作输出接收器，需要在部署到 Azure 之前使用 PowerShell 提供对服务主体的访问权限。 了解有关如何[使用资源管理器模板部署具有托管标识的 ADLS Gen1](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment) 的详细信息。
## <a name="next-steps"></a>后续步骤

* [快速入门：在 Visual Studio Code（预览版）中创建 Azure 流分析云作业](quick-create-vs-code.md)
* [使用 Visual Studio Code（预览版）在本地测试流分析查询](visual-studio-code-local-run.md)
* [使用 Visual Studio Code（预览版）浏览 Azure 流分析](visual-studio-code-explore-jobs.md)
