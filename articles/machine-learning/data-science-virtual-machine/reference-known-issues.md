---
title: 已知问题与故障排除
titleSuffix: Azure Data Science Virtual  Machine
description: 获取 Azure Data Science Virtual Machine 的已知问题、解决方法和故障排除的列表。
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: reference
ms.date: 10/10/2019
ms.openlocfilehash: b7d1763f647f5d01cd97b43b62d9367ebb3a8c3e
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75599585"
---
# <a name="known-issues-and-troubleshooting-the-azure-data-science-virtual-machine"></a>Azure Data Science Virtual Machine 的已知问题和故障排除方法

本文可帮助你查找和更正使用 Azure Data Science Virtual Machine 时遇到的错误或失败。

## <a name="python-package-installation-issues"></a>Python 包安装问题

### <a name="installing-packages-with-pip-breaks-dependencies-on-linux"></a>在 Linux 上安装具有 pip 中断依赖项的包

安装包时，请使用 `sudo pip install` 而不是 `pip install`。

## <a name="disk-encryption-issues"></a>磁盘加密问题

### <a name="disk-encryption-fails-on-the-ubuntu-dsvm"></a>Ubuntu DSVM 上的磁盘加密失败

Ubuntu DSVM 目前不支持 Azure 磁盘加密 (ADE)。 若要解决问题，请考虑配置[使用客户托管密钥的 Azure 存储加密](../../storage/common/storage-encryption-keys-portal.md)。

## <a name="tool-appears-disabled"></a>工具显示为已禁用

### <a name="hyper-v-does-not-work-on-the-windows-dsvm"></a>Hyper-V 在 Windows DSVM 上不起作用

这是预期的行为，因为对于启动性能，我们已禁用某些服务。 若要重新启用，请打开 Windows DSVM 上的搜索栏，键入“服务”，将所有 Hyper-V 服务设置为“手动”，并将“Hyper-V 虚拟机管理”设置为“自动”。

最终屏幕应如下所示：

   ![启用 Hyper-V](./media/workaround/hyperv-enable-dsvm.png)

