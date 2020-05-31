---
title: 关于 MARS 代理
description: 了解 MARS 代理如何支持备份方案
ms.reviewer: srinathv
ms.topic: conceptual
author: Johnnytechn
ms.date: 05/15/2020
ms.author: v-johya
ms.openlocfilehash: d1b31af809e7ee992ed986b281239ba9116b8757
ms.sourcegitcommit: 08b42258a48d96d754244064d065e4d5703f1cfb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/18/2020
ms.locfileid: "83445252"
---
# <a name="about-the-azure-recovery-services-mars-agent"></a>关于 Azure 恢复服务 (MARS) 代理

本文介绍 Azure 备份服务如何使用 Azure 恢复服务 (MARS) 代理将本地计算机中的文件、文件夹以及卷或系统状态备份和还原到 Azure。

MARS 代理支持以下备份方案：

![MARS 备份方案](./media/backup-try-azure-backup-in-10-mins/backup-scenarios.png)

- **文件和文件夹**：选择性地保护 Windows 文件和文件夹。
- **卷级别**：保护计算机的整个 Windows 卷。
- **系统级别**：保护整个 Windows 系统状态。

MARS 代理支持以下还原方案：

![MARS 恢复方案](./media/backup-try-azure-backup-in-10-mins/restore-scenarios.png)

- **同一服务器**：最初在其上创建备份的服务器。
  - **文件和文件夹**：选择要还原的各个文件和文件夹。
  - **卷级别**：选择要还原的卷和恢复点，然后将其还原到同一计算机上的同一位置或备用位置。  创建现有文件的副本、覆盖现有文件，或跳过现有文件的恢复。
  - **系统级别**：选择要还原到同一计算机上的指定位置的系统状态和恢复点。

- **备用服务器**：在其上创建备份的服务器以外的服务器。
  - **文件和文件夹**：选择要将其恢复点还原到目标计算机的各个文件和文件夹。
  - **卷级别**：选择要还原到另一位置的卷和恢复点。 创建现有文件的副本、覆盖现有文件，或跳过现有文件的恢复。
  - **系统级别**：选择要作为系统状态文件还原到备用计算机的系统状态和恢复点。

## <a name="backup-process"></a>备份过程

1. 在 Azure 门户中创建[恢复服务保管库](install-mars-agent.md#create-a-recovery-services-vault)，并从备份目标中选择文件、文件夹和系统状态。
2. [将恢复服务保管库凭据和代理安装程序下载](/backup/install-mars-agent#download-the-mars-agent)到本地计算机。

    若要通过选择“备份”选项来保护本地计算机，请选择文件、文件夹和系统状态，然后下载 MARS 代理。

3. 准备基础结构：

    a. 运行安装程序以[安装代理](/backup/install-mars-agent#install-and-register-the-agent)。

    b. 使用下载的保管库凭据将计算机注册到恢复服务保管库。
4. 在客户端上的代理控制台中[配置备份](/backup/backup-windows-with-mars-agent#create-a-backup-policy)。 指定备份数据的保留策略以开始保护数据。

![Azure 备份代理示意图](./media/backup-try-azure-backup-in-10-mins/backup-process.png)

### <a name="additional-scenarios"></a>其他方案

- **备份 Azure 虚拟机中的特定文件和文件夹**：备份 Azure 虚拟机 (VM) 的主要方法是在 VM 上使用 Azure 备份扩展。 该扩展可备份整个 VM。 若要备份 VM 中的特定文件和文件夹，可在 Azure VM 中安装 MARS 代理。 有关详细信息，请参阅[体系结构：内置 Azure VM 备份](/backup/backup-architecture#architecture-built-in-azure-vm-backup)。

- **脱机种子设定**：最初在 Azure 中创建完整数据备份时，通常会传输大量的数据，并会占用更多的网络带宽。 后续的备份只会传输差异数量（增量）的数据。 Azure 备份可压缩初始备份。 通过脱机种子设定过程，Azure 备份可以使用磁盘将压缩后的初始备份数据脱机上传到 Azure。

## <a name="next-steps"></a>后续步骤

[MARS 代理支持矩阵](/backup/backup-support-matrix-mars-agent)

[MARS 代理常见问题解答](/backup/backup-azure-file-folder-backup-faq)

