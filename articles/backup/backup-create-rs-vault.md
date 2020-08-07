---
title: 创建恢复服务保管库
description: 本文介绍如何创建用于存储备份和恢复点的恢复服务保管库。
author: Johnnytechn
ms.topic: conceptual
origin.date: 08/30/2019
ms.date: 07/31/2020
ms.author: v-johya
ms.openlocfilehash: aa2db64041b27f9bcea75f26cc1c75bc7e9194ef
ms.sourcegitcommit: b5794af488a336d84ee586965dabd6f45fd5ec6d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2020
ms.locfileid: "87508446"
---
# <a name="create-a-recovery-services-vault"></a>创建恢复服务保管库

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="set-storage-redundancy"></a>设置存储冗余

Azure 备份会自动处理保管库的存储。 需要指定如何复制该存储。

<!--Not available in MC: Azure file share-->
1. 从“恢复服务保管库”边栏选项卡中，单击新保管库  。 在“设置”部分下，单击“属性” 。
2. 在“属性”中，在“备份配置”下，单击“更新”。  

3. 选择存储复制类型，然后单击“保存”。

     ![设置新保管库的存储配置](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

   - 如果使用 Azure 作为主要备份存储终结点，则我们建议继续使用默认的“异地冗余”设置。
   - 如果不使用 Azure 作为主要的备份存储终结点，则请选择“本地冗余”，减少 Azure 存储费用。
   - 详细了解[异地冗余](../storage/common/storage-redundancy-grs.md)和[本地冗余](../storage/common/storage-redundancy-lrs.md)。

> [!NOTE]
> 在保管库中配置备份之前，必须更改恢复服务保管库的存储复制类型（本地冗余/异地冗余）。 配置备份后，将禁用修改选项。
>
>- 如果尚未配置备份，请遵循[这些步骤](/backup/backup-create-rs-vault#set-storage-redundancy)检查和修改设置。
>- 如果已配置备份，并且必须从 GRS 迁移到 LRS，请查看 [解决方法](backup-azure-backup-faq.md#can-i-change-from-grs-to-lrs-after-a-backup)。

<!--Not available in MC: ## Set Cross Region Restore-->
## <a name="modifying-default-settings"></a>修改默认设置

在保管库中配置备份之前，我们强烈建议检查“存储复制类型”和“安全设置”的默认设置。 

- “存储复制类型”默认设置为“异地冗余”(GRS) 。 配置备份后，将禁用修改选项。
  - 如果尚未配置备份，请遵循[这些步骤](/backup/backup-create-rs-vault#set-storage-redundancy)检查和修改设置。
  - 如果已配置备份，并且必须从 GRS 迁移到 LRS，请查看 [解决方法](backup-azure-backup-faq.md#can-i-change-from-grs-to-lrs-after-a-backup)。

- “软删除”对新建的保管库默认为“已启用”，旨在防止意外或恶意删除备份数据。  请遵循[这些步骤](/backup/backup-azure-security-feature-cloud#enabling-and-disabling-soft-delete)检查和修改设置。

## <a name="next-steps"></a>后续步骤

[了解](backup-azure-recovery-services-vault-overview.md)恢复服务保管库。
[了解](backup-azure-delete-vault.md)如何删除恢复服务保管库。

