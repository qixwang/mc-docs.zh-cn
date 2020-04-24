---
title: Azure CLI 脚本示例 - 运行批处理作业
description: 此脚本将创建一个批处理作业，并将一系列任务添加到该作业。 它还演示了如何监视作业及其任务。
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: tysonn
ms.assetid: ''
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
origin.date: 12/12/2019
ms.date: 2/3/2020
ms.author: v-lingwu
ms.openlocfilehash: b7527c74375decb0e4dacb7c9b75dffbb8dbcd5a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77028582"
---
# <a name="cli-example-run-a-job-and-tasks-with-azure-batch"></a>CLI 示例：使用 Azure Batch 运行作业和任务

此脚本将创建一个批处理作业，并将一系列任务添加到该作业。 它还演示了如何监视作业及其任务。 

如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI 2.0](/cli/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

```azurecli
#!/bin/bash

# Create a resource group.
az group create --name myResourceGroup --location chinanorth

# Create a general-purpose storage account in your resource group.
az storage account create \
    --resource-group myResourceGroup \
    --name mystorageaccount \
    --location chinanorth \
    --sku Standard_LRS

# Create a Batch account.
az batch account create \
    --name mybatchaccount \
    --storage-account mystorageaccount \
    --resource-group myResourceGroup \
    --location chinanorth

# Authenticate against the account directly for further CLI interaction.
az batch account login \
    --name mybatchaccount \
    --resource-group myResourceGroup \
    --shared-key-auth

# Create a new Linux pool with a virtual machine configuration. 
az batch pool create \
    --id mypool \
    --vm-size Standard_A1 \
    --target-dedicated 2
    --image canonical:ubuntuserver:16.04.0-LTS \
    --node-agent-sku-id "batch.node.ubuntu 16.04"


# Create a new job to encapsulate the tasks that are added.
az batch job create \
    --id myjob \
    --pool-id mypool

# Add tasks to the job. Here the task is a basic shell command.
az batch task create \
    --job-id myjob \
    --task-id task1 \
    --command-line "/bin/bash -c 'printenv AZ_BATCH_TASK_WORKING_DIR'"

# To add many tasks at once, specify the tasks
# in a JSON file, and pass it to the command. See tasks.json for formatting.
az batch task create \
    --job-id myjob \
    --json-file tasks.json

# Update the job so that it is automatically
# marked as completed once all the tasks are finished.
az batch job set \
--job-id myjob \
--on-all-tasks-complete terminateJob

# Monitor the status of the job.
az batch job show --job-id myjob

# Monitor the status of a task.
az batch task show \
    --job-id myjob \
    --task-id task1
```
## <a name="clean-up-deployment"></a>清理部署

运行以下命令以删除资源组及其相关的所有资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| Command | 说明 |
|---|---|
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az batch account create](/cli/batch/account#az-batch-account-create) | 创建批处理帐户。 |
| [az batch account login](/cli/batch/account#az-batch-account-login) | 针对指定的批处理帐户进行身份验证，以便进一步进行 CLI 交互。  |
| [az batch pool create](/cli/batch/pool#az-batch-pool-create) | 创建计算节点池。  |
| [az batch job create](/cli/batch/job#az-batch-job-create) | 创建批处理作业。  |
| [az batch task create](/cli/batch/task#az-batch-task-create) | 将任务添加到指定的批处理作业。  |
| [az batch job set](/cli/batch/job#az-batch-job-set) | 更新批处理作业的属性。  |
| [az batch job show](/cli/batch/job#az-batch-job-show) | 检索指定批处理作业的详细信息。  |
| [az batch task show](/cli/batch/task#az-batch-task-show) | 从指定的批处理作业中检索任务的详细信息。  |
| [az group delete](/cli/group#az-group-delete) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli)。

<!-- Update_Description: link update -->