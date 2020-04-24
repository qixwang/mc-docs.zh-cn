---
title: 支持将 Azure Site Recovery 与 Azure 备份配合使用
description: 概述如何将 Azure Site Recovery 与 Azure 备份一起使用。
author: rockboyfor
manager: digimobile
ms.service: site-recovery
ms.topic: conceptual
origin.date: 03/18/2019
ms.date: 09/30/2019
ms.author: v-yeche
ms.openlocfilehash: 3a28d7133412f8a5a3a79dbfe3e4a6c9ae8a5c74
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71340801"
---
# <a name="support-for-using-site-recovery-with-azure-backup"></a>支持将 Site Recovery 与 Azure 备份配合使用

本文总结了对将 [Site Recovery 服务](site-recovery-overview.md)与 [Azure 备份服务](/backup/backup-overview)一起使用的支持。

**Action** | **Site Recovery 支持** | **详细信息**
--- | --- | ---
**一起部署服务** | 支持 | 服务具有互操作性，可一起配置。
**文件备份/还原** | 支持 | 为 VM 启用备份和复制并进行备份时，在源端 VM 或 VM 组上还原文件不会出现问题。 复制照常继续，复制运行状况没有任何变化。
**磁盘备份/还原** | 目前不支持 | 如果还原备份的磁盘，则需要再次禁用并重新启用 VM 的复制。
**VM 备份/还原** | 目前不支持 | 如果备份或还原 VM 或 VM 组，则需要禁用并重新启用 VM 的复制。

<!--Update_Description: update meta properties -->
