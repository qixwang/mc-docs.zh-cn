---
title: 使用 Azure 资源管理器模板创建并加密虚拟机规模集
description: 本快速入门介绍如何使用 Azure 资源管理器模板来创建和加密虚拟机规模集
author: msmbaldwin
ms.author: v-junlch
ms.service: virtual-machine-scale-sets
ms.topic: quickstart
origin.date: 10/10/2019
ms.date: 10/29/2019
ms.openlocfilehash: cc9ce5a1eabf91f0fde26b28e8123afe1adc65a3
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "73142201"
---
# <a name="encrypt-virtual-machine-scale-sets-with-azure-resource-manager"></a>使用 Azure 资源管理器加密虚拟机规模集

可以使用 Azure 资源管理器模板加密或解密 Linux 虚拟机规模集。

## <a name="deploying-templates"></a>部署模板

首先，选择适合你的方案的模板。

- [在运行的 Linux 虚拟机规模集上启用磁盘加密](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-linux)

- [在运行的 Windows 虚拟机规模集上启用磁盘加密](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-vmss-windows)

  - [使用 jumpbox 部署 Linux VM 的虚拟机规模集，并在 Linux 虚拟机规模集上启用加密](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox)

  - [使用 jumpbox 部署 Windows VM 的虚拟机规模集，并在 Windows 虚拟机规模集上启用加密](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox)

- [在运行的 Linux 虚拟机规模集上禁用磁盘加密](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-linux)

- [在运行的 Windows 虚拟机规模集上禁用磁盘加密](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-vmss-windows)

然后执行以下步骤：

     1. Click **Deploy to Azure**.
     2. Fill in the required fields then agree to the terms and conditions.
     3. Click **Purchase** to deploy the template.

## <a name="next-steps"></a>后续步骤

- [适用于虚拟机规模集的 Azure 磁盘加密](disk-encryption-overview.md)
- [使用 Azure CLI 加密虚拟机规模集](disk-encryption-cli.md)
- [使用 Azure PowerShell 加密虚拟机规模集](disk-encryption-powershell.md)
- [将 Azure 磁盘加密与虚拟机规模集扩展排序配合使用](disk-encryption-extension-sequencing.md)

