---
title: 有助于保护云工作负荷的安全功能
description: 了解如何在 Azure 备份中使用安全功能，使备份更加安全。
ms.topic: conceptual
author: lingliw
origin.date: 09/13/2019
ms.date: 11/14/2019
ms.author: v-lingwu
ms.openlocfilehash: 0cfff1c4a0518ce749398d2099fd78cc3c2a08bd
ms.sourcegitcommit: 27eaabd82b12ad6a6840f30763034a6360977186
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77497633"
---
# <a name="security-features-to-help-protect-cloud-workloads-that-use-azure-backup"></a>有助于保护使用 Azure 备份的云工作负荷的安全功能

对安全问题（例如恶意软件、勒索软件、入侵）的关注在逐渐上升。 这些安全问题可能会代价高昂（就金钱和数据来说）。 为了防范此类攻击，Azure 备份现提供可帮助保护备份数据（即使数据已删除）的安全功能。

其中的一项功能是软删除。 在使用软删除的情况下，即使恶意行动者删除了 VM 的备份（或用户意外删除了备份数据），备份数据也仍会保留 14 天，因此可以恢复该备份项，而不会丢失数据。 以“软删除”状态将备份数据额外保留 14 天不会向客户收取任何费用。 Azure 还使用[存储服务加密](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)来加密所有已备份的静态数据，以进一步保护数据。

Azure 虚拟机的软删除保护功能已推出正式版。

>[!NOTE]
>针对 Azure VM 中 SQL 服务器的软删除以及针对 Azure VM 工作负荷中 SAP HANA 的软删除现已推出预览版。<br>
> [!NOTE]
> 软删除只是保护已删除的备份数据。 如果删除未备份的 VM，则软删除功能不会保留数据。 应使用 Azure 备份保护所有资源，以确保能够完全复原。
>

## <a name="soft-delete"></a>软删除


### <a name="soft-delete-for-vms-using-azure-portal"></a>使用 Azure 门户对 VM 进行软删除

1. 若要删除 VM 的备份数据，必须停止备份。 在 Azure 门户中转到你的恢复服务保管库，右键单击备份项，然后选择“停止备份”。 

   ![Azure 门户中备份项的屏幕截图](./media/backup-azure-security-feature-cloud/backup-stopped.png)

2. 在接下来的窗口中，会提供用于删除或保留备份数据的选项。 如果依次选择“删除备份数据”、“停止备份”，则不会永久删除 VM 备份，   而是将备份数据以软删除状态保留 14 天。 如果选择“删除备份数据”，则会将一封“删除”警报电子邮件发送到配置的电子邮件 ID，告知用户备份数据将延期保留 14 天。  在第 12 天时也会发送一封警报电子邮件，告知还有两天时间可以恢复已删除的数据。 删除会推迟到第 15 天，到时数据将永久删除，同时系统会发送最后一封警报电子邮件，告知数据已永久删除。

   ![Azure 门户中“停止备份”屏幕的屏幕截图](./media/backup-azure-security-feature-cloud/delete-backup-data.png)

3. 在这 14 天内，恢复服务保管库中已软删除的 VM 旁边会显示一个红色的“软删除”图标。

   ![Azure 门户中处于软删除状态的 VM 的屏幕截图](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

   > [!NOTE]
   > 如果保管库中存在任何已软删除的备份项，此时无法删除该保管库。 请在永久删除了备份项，并且保管库中未留下任何处于软删除状态的项之后，尝试删除保管库。

4. 若要还原软删除的 VM，必须先将其取消删除。 若要取消删除，请选择软删除的 VM，然后选择“取消删除”选项。 

   ![Azure 门户中“取消删除 VM”的屏幕截图](./media/backup-azure-security-feature-cloud/choose-undelete.png)

   窗口中会显示一条警告，指出如果选择取消删除，将取消删除 VM 的所有还原点，这些还原点可用于执行还原操作。 VM 将保留为“停止保护并保留数据”状态，备份将会暂停，备份数据将永久保留，且没有任何生效的备份策略。

   ![Azure 门户中“确认取消删除 VM”的屏幕截图](./media/backup-azure-security-feature-cloud/undelete-vm.png)

   此时，还可以通过从选定还原点中选择“还原 VM”来还原 VM。   

   ![Azure 门户中“还原 VM”选项的屏幕截图](./media/backup-azure-security-feature-cloud/restore-vm.png)

   > [!NOTE]
   > 只有在用户执行“恢复备份”操作后，垃圾回收器才会运行并清理已过期的恢复点。 

5. 取消删除操作完成后，状态将恢复为“停止备份并保留数据”，然后，你可以选择“恢复备份”。  “恢复备份”操作会恢复处于活动状态的、与定义备份和保留计划的用户所选的备份策略相关联的备份项。 

   ![Azure 门户中“恢复备份”选项的屏幕截图](./media/backup-azure-security-feature-cloud/resume-backup.png)

此流程图显示了启用软删除时备份项的不同步骤和状态：

![软删除的备份项的生命周期](./media/backup-azure-security-feature-cloud/lifecycle.png)

有关详细信息，请参阅下面的[常见问题解答](backup-azure-security-feature-cloud.md#frequently-asked-questions)部分。

### <a name="soft-delete-for-vms-using-azure-powershell"></a>使用 Azure Powershell 对 VM 进行软删除

> [!IMPORTANT]
> 使用 Azure PS 软删除所需的 Az.RecoveryServices 版本最低为 2.2.0。 可使用 ```Install-Module -Name Az.RecoveryServices -Force``` 获取最新版本。

如上所述 Azure 门户，使用 Azure Powershell 时，步骤顺序是相同的。

#### <a name="delete-the-backup-item-using-azure-powershell"></a>使用 Azure Powershell 删除备份项

使用 [Disable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) PS cmdlet 删除备份项。

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

备份项的“DeleteState”将从“NotDeleted”更改为“ToBeDeleted”。 备份数据会保留 14 天。 如果要还原删除操作，则应执行撤消-删除操作。

#### <a name="undoing-the-deletion-operation-using-azure-powershell"></a>使用 Azure Powershell 撤销删除操作

首先，获取处于软删除状态（即将删除）的相关备份项。

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

然后，使用 [Undo-AzRecoveryServicesBackupItemDeletion](https://docs.microsoft.com/powershell/module/az.recoveryservices/undo-azrecoveryservicesbackupitemdeletion?view=azps-3.1.0) PS cmdlet 执行撤消-删除操作。

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

备份项的“DeleteState”将还原为“NotDeleted”。 但保护仍处于停止状态。 需要[恢复备份](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#change-policy-for-backup-items)以重新启用保护。

### <a name="soft-delete-for-vms-using-rest-api"></a>使用 REST API 对 VM 进行软删除

- 如[此处](backup-azure-arm-userestapi-backupazurevms.md#stop-protection-and-delete-data)所述，使用 REST API 删除备份。
- 如果用户想要撤消这些删除操作，请参阅[此处](backup-azure-arm-userestapi-backupazurevms.md#undo-the-stop-protection-and-delete-data)所述的步骤。

## <a name="disabling-soft-delete"></a>禁用软删除

软删除在新创建的保管库上默认启用，目的是防止意外或恶意删除备份数据。  建议不要禁用此功能。 唯一应该考虑禁用软删除的情况是，你打算将受保护的项移到新保管库，需要在删除后重新进行保护，因此等不及要求的 14 天（例如在测试环境中）。仅备份管理员可以禁用此功能。 如果禁用此功能，则只要删除受保护的项，就会导致即时删除，不可还原。 如果在禁用此功能之前备份数据处于软删除状态，则会保持软删除状态。 若要立即永久删除这些项，则需先取消删除，然后再次将其删除，这样就可以永久删除它们。

### <a name="disabling-soft-delete-using-azure-portal"></a>使用 Azure 门户禁用软删除

若要禁用软删除，请执行以下步骤：

1. 在 Azure 门户中转到保管库，然后转到“设置” -> “属性”。  
2. 在“属性”窗格中选择“安全设置” -> “更新”。    
3. 在“安全设置”窗格的“软删除”下，选择“禁用”。  

![禁用软删除](./media/backup-azure-security-feature-cloud/disable-soft-delete.png)

### <a name="disabling-soft-delete-using-azure-powershell"></a>使用 Azure Powershell 禁用软删除

> [!IMPORTANT]
> 使用 Azure PS 软删除所需的 Az.RecoveryServices 版本最低为 2.2.0。 可使用 ```Install-Module -Name Az.RecoveryServices -Force``` 获取最新版本。

若要禁用，请使用 [AzRecoveryServicesVaultBackupProperty](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty?view=azps-3.1.0) PS cmdlet。

```powershell
Set-AzRecoveryServicesVaultProperty -VaultId $myVaultID -SoftDeleteFeatureState Disable


StorageModelType       :
StorageType            :
StorageTypeState       :
EnhancedSecurityState  : Enabled
SoftDeleteFeatureState : Disabled
```

### <a name="disabling-soft-delete-using-rest-api"></a>使用 REST API 禁用软删除

若要使用 REST API 禁用软删除功能，请参阅[此处](use-restapi-update-vault-properties.md)所述的步骤。

## <a name="permanently-deleting-soft-deleted-backup-items"></a>永久删除软删除的备份项

如果在禁用此功能之前备份数据处于软删除状态，则会保持软删除状态。 若要立即永久删除这些项，请取消删除，然后再次将其删除，这样就可以永久删除它们。

### <a name="using-azure-portal"></a>使用 Azure 门户

执行以下步骤：

1. 按照步骤[禁用软删除](#disabling-soft-delete)。
2. 在 Azure 门户中，请切换到保管库，转到“备份项”并选择已软删除的 VM。 

![选择已软删除的 VM](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

3. 选择“撤消删除”选项  。

![选择“撤消删除”](./media/backup-azure-security-feature-cloud/choose-undelete.png)


4. 此时会出现一个窗口。 选择“撤消删除”  。

![选择“撤消删除”](./media/backup-azure-security-feature-cloud/undelete-vm.png)

5. 选择“删除备份数据”，永久删除备份数据  。

![选择“删除备份数据”](https://docs.microsoft.com/azure/backup/media/backup-azure-manage-vms/delete-backup-buttom.png)

6. 键入备份项的名称以确认你要删除恢复点。

![键入备份项的名称](https://docs.microsoft.com/azure/backup/media/backup-azure-manage-vms/delete-backup-data1.png)

7. 若要删除项的备份数据，请选择“删除”  。 一条通知消息将让你获悉备份数据已删除。

### <a name="using-azure-powershell"></a>使用 Azure PowerShell

如果在禁用软删除之前删除了项，则它们将处于已软删除状态。 若要立即删除它们，需要反转删除操作，然后再次执行。

确定处于已软删除状态的项。

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

然后反转启用软删除时执行的删除操作。

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

由于软删除现在已禁用，删除操作将导致立即删除备份数据。

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

### <a name="using-rest-api"></a>使用 REST API

如果在禁用软删除之前删除了项，则它们将处于已软删除状态。 若要立即删除它们，需要反转删除操作，然后再次执行。

1. 首先，使用[此处](backup-azure-arm-userestapi-backupazurevms.md#undo-the-stop-protection-and-delete-data)提到的步骤撤消删除操作。
2. 然后遵循[此处](use-restapi-update-vault-properties.md#update-soft-delete-state-using-rest-api)所述的步骤，使用 REST API 禁用软删除功能。
3. 然后，使用[此处](backup-azure-arm-userestapi-backupazurevms.md#stop-protection-and-delete-data)所述的 REST API 删除备份。

## <a name="encryption"></a>Encryption

### <a name="encryption-of-backup-data-using-microsoft-managed-keys"></a>使用 Microsoft 托管密钥加密备份数据

备份数据将使用 Azure 存储加密自动加密。 加密可以保护数据，并帮助组织履行在安全性与合规性方面做出的承诺。 数据将使用 256 位 AES 加密法（可用的最强大块加密法之一）以透明方式进行加密和解密，并符合 FIPS 140-2 规范。 Azure 存储加密法类似于 Windows 上的 BitLocker 加密法。

在 Azure 中，Azure 存储与保管库之间传输的数据受 HTTPS 保护。 此数据保留在 Azure 主干网络上。

有关详细信息，请参阅[静态数据的 Azure 存储加密](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)。 请参阅 [Azure 备份常见问题解答](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq#encryption)，其中解答了有关加密的任何问题。

### <a name="encryption-of-backup-data-using-customer-managed-keys"></a>使用客户管理的密钥加密备份数据

备份 Azure 虚拟机时，还可以选择使用 Azure Key Vault 中存储的加密密钥来加密恢复服务保管库中的备份数据。

>[!NOTE]
>此功能目前已推出尝鲜版。 如果你想要使用客户管理的密钥加密备份数据，请填写[此调查表](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapURE9TTDRIUEUyNFhNT1lZS1BNVDdZVllHWi4u)。 请注意，能否使用此功能取决于 Azure 备份服务的审批结果。

### <a name="backup-of-managed-disk-vm-encrypted-using-customer-managed-keys"></a>备份使用客户管理的密钥加密的托管磁盘 VM

借助 Azure 备份可以备份包含使用客户管理密钥加密的磁盘的 Azure 虚拟机。 有关详细信息，请参阅[使用客户管理的密钥加密托管磁盘](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#customer-managed-keys)。

### <a name="backup-of-encrypted-vms"></a>备份加密的 VM

可以使用 Azure 备份服务来备份和还原包含已加密磁盘的 Windows 或 Linux Azure 虚拟机 (VM)。 有关说明，请参阅[使用 Azure 备份来备份和还原已加密的虚拟机](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption)。

## <a name="other-security-features"></a>其他安全功能

### <a name="protection-of-azure-backup-recovery-points"></a>保护 Azure 备份恢复点

恢复服务保管库使用的存储帐户是隔离的，恶意用户无法对其进行访问。 仅允许通过 Azure 备份管理操作（例如还原）进行访问。 可通过基于角色的访问控制 (RBAC) 来控制这些管理操作。

有关详细信息，请参阅[使用基于角色的访问控制管理 Azure 备份恢复点](/backup/backup-rbac-rs-vault)。

## <a name="frequently-asked-questions"></a>常见问题

### <a name="for-soft-delete"></a>对于软删除

#### <a name="do-i-need-to-enable-the-soft-delete-feature-on-every-vault"></a>是否需要对每个保管库启用软删除功能？

否。此功能是为所有恢复服务保管库设计的，默认已启用。

#### <a name="can-i-configure-the-number-of-days-for-which-my-data-will-be-retained-in-soft-deleted-state-after-delete-operation-is-complete"></a>是否可以配置在完成删除操作后，以软删除状态保留数据的天数？

否。删除操作完成后，数据将额外保留固定的 14 天。

#### <a name="do-i-need-to-pay-the-cost-for-this-additional-14-day-retention"></a>是否需要支付这额外 14 天数据保留的费用？

否。14 天额外保留是软删除功能免费附送的。

#### <a name="can-i-perform-a-restore-operation-when-my-data-is-in-soft-delete-state"></a>如果数据处于软删除状态，是否可以执行还原操作？

否。需要取消删除已软删除的资源才能还原。 取消删除操作会将资源恢复到“停止保护并保留数据”状态，然后，你可以还原到任意时间点。  在此状态下，垃圾回收器将保持暂停状态。

#### <a name="will-my-snapshots-follow-the-same-lifecycle-as-my-recovery-points-in-the-vault"></a>快照的生命周期是否与保管库中恢复点的生命周期相同？

是的。

#### <a name="how-can-i-trigger-the-scheduled-backups-again-for-a-soft-deleted-resource"></a>如何针对软删除的资源再次触发计划的备份？

先取消删除，然后执行恢复操作，即可再次保护资源。 恢复操作将关联某个备份策略，以触发具有选定保留期的计划备份。 此外，在恢复操作完成后，垃圾回收器会立即运行。 若要从超出过期日期的恢复点执行还原，我们建议在触发恢复操作之前执行此还原操作。

#### <a name="can-i-delete-my-vault-if-there-are-soft-deleted-items-in-the-vault"></a>如果保管库中存在软删除的项，我是否可以删除该保管库？

如果保管库中存在处于软删除状态的备份项，则无法删除恢复服务保管库。 完成删除操作 14 天后，软删除的项将永久删除。 如果不能等 14 天，则请[禁用软删除](#disabling-soft-delete)，接着取消删除软删除的项，然后再次将其删除，这样就可以永久删除它们。 在确保没有受保护项和软删除项以后，可以删除保管库。  

#### <a name="can-i-delete-the-data-earlier-than-the-14-days-soft-delete-period-after-deletion"></a>是否可以在删除后的 14 天软删除期之前删除数据？

否。 无法强制删除软删除项，14 天后会自动删除这些项。 启用此安全功能是为了保护备份的数据不被意外删除或恶意删除。  你应等待 14 天，然后再在 VM 上执行任何其他操作。  不会对软删除项收费。  如果需要将 14 天内标记为软删除的 VM 重新保护到新保管库，请联系 Microsoft 支持部门。

#### <a name="can-soft-delete-operations-be-performed-in-powershell-or-cli"></a>是否可以在 PowerShell 或 CLI 中执行软删除操作？

可以使用 [Powershell](#soft-delete-for-vms-using-azure-powershell) 执行软删除操作。 目前不支持 CLI。

#### <a name="is-soft-delete-supported-for-other-cloud-workloads-like-sql-server-in-azure-vms-and-sap-hana-in-azure-vms"></a>其他云工作负荷（例如 Azure VM 中的 SQL Server，以及 Azure VM 中的 SAP HANA）是否支持软删除？

否。 目前只有 Azure 虚拟机支持软删除。

## <a name="next-steps"></a>后续步骤

- 了解 [Azure 备份的安全控制](backup-security-controls.md)。
