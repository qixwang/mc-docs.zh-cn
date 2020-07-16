---
title: include 文件
description: include 文件
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 03/23/2020
ms.date: 07/13/2020
ms.testscope: yes
ms.testdate: 07/13/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 247ef2d4d9e9dfa130974f237e90d1c14c2eb95e
ms.sourcegitcommit: 6c9e5b3292ade56d812e7e214eeb66aeb9b8776e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86218836"
---
<!--NOT AVILABLE ON MOONCAKE TILL ON 07/09/2020-->
<!--RELEASE BEFORE CAREFULLY AFTER CONFIRMATION-->
### <a name="portal"></a>门户

如果你是第一次执行此操作，则为磁盘设置客户托管密钥时将要求你按特定顺序创建资源。 首先，需要创建并设置 Azure Key Vault。

#### <a name="setting-up-your-azure-key-vault"></a>设置 Azure Key Vault

1. 登录到 [Azure 门户](https://portal.azure.cn/)。
1. 搜索并选择“Key Vault”。

    [ ![sse-key-vault-portal-search.png](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search.png)](media/virtual-machines-disk-encryption-portal/sse-key-vault-portal-search-expanded.png#lightbox)

    > [!IMPORTANT]
    > Azure Key Vault、磁盘加密集、VM、磁盘和快照必须都位于同一区域和订阅中才能成功部署。

1. 选择“+添加”以创建新的 Key Vault。
1. 创建新的资源组。
1. 输入 Key Vault 名称，选择区域，然后选择定价层。
1. 选择“审阅 + 创建”，验证选择，然后选择“创建” 。

    ![Azure Key Vault 创建体验的屏幕截图。 显示所创建的特定值](media/virtual-machines-disk-encryption-portal/sse-create-a-key-vault.png)

1. Key Vault 部署完成后，请选择它。
1. 在“设置”下，选择“密钥” 。
1. 选择“生成/导入”。

    ![Key Vault 资源设置窗格的屏幕截图。 显示设置中的“生成/导入”按钮。](media/virtual-machines-disk-encryption-portal/sse-key-vault-generate-settings.png)

1. 将“密钥类型”设置为“RSA”，将“RSA 密钥大小”设置为“2048”   。
1. 根据需要填写其余选项，然后选择“创建”。

    ![选择“生成/导入”按钮后出现的“创建密钥”边栏选项卡的屏幕截图](media/virtual-machines-disk-encryption-portal/sse-create-a-key-generate.png)

<!--Not Available on **Disk Encryption Sets** on FOLLOWING FULL SECTION-->
#### <a name="setting-up-your-disk-encryption-set"></a>设置磁盘加密集

1. 搜索“磁盘加密集”并选择它。
1. 在“磁盘加密集”边栏选项卡上，选择“+添加” 。

    ![磁盘加密门户主屏幕的屏幕截图。 突出显示“添加”按钮](media/virtual-machines-disk-encryption-portal/sse-create-disk-encryption-set.png)

1. 选择资源组，命名加密集，然后选择与 Key Vault 相同的区域。
1. 选择“Key Vault 和密钥”。
1. 选择先前创建的 Key Vault 和密钥，以及版本。
1. 按“选择”。
1. 选择“审阅 + 创建”，然后选择“创建” 。

    ![磁盘加密创建边栏选项卡的屏幕截图。 显示订阅、资源组、磁盘加密集名称、区域以及 Key Vault + 密钥选择器。](media/virtual-machines-disk-encryption-portal/sse-disk-enc-set-blade-key.png)

1. 完成创建后，打开磁盘加密集，然后选择弹出的警报。

    ![警报弹出窗口的屏幕截图：“若要将磁盘、映像或快照与磁盘加密设置相关联，必须向 Key Vault 授予权限”。 选择此警报以继续](media/virtual-machines-disk-encryption-portal/sse-disk-enc-alert-fix.png)

应弹出两个通知并成功。 这样做使你能够将磁盘加密集与 Key Vault 协同使用。

![Key Vault 的成功权限和角色分配的屏幕截图。](media/virtual-machines-disk-encryption-portal/disk-enc-notification-success.png)

<!--Not Available on **Encryption at rest with a customer-managed key** on FOLLOWING FULL SECTION-->
#### <a name="deploy-a-vm"></a>部署 VM

创建并设置好 Key Vault 和磁盘加密集之后，接下来即可使用加密来部署 VM。
VM 部署过程与标准部署过程类似，唯一的差别在于，你需要将 VM 部署到与其他资源相同的区域中，并选择使用客户托管密钥。

1. 搜索“虚拟机”，然后选择“+ 添加”以创建 VM 。
1. 在“基本”选项卡上，选择与磁盘加密集和 Azure Key Vault 相同的区域。
1. 根据需要在“基本”选项卡上填写其他值。

    ![VM 创建体验的屏幕截图，其中突出显示了区域值。](media/virtual-machines-disk-encryption-portal/sse-create-a-vm-region.png)

1. 在“磁盘”选项卡上，选择“使用客户托管密钥进行静态加密” 。
1. 在“磁盘加密集”下拉列表中选择磁盘加密集。
1. 根据需要进行剩余选择。

    ![VM 创建体验的屏幕截图，“磁盘”边栏选项卡。 其中突出显示了“磁盘加密集”下拉列表。](media/virtual-machines-disk-encryption-portal/sse-create-vm-select-cmk-encryption-set.png)

<!--Not Available on **Encryption** menu ON Disks setting on FOLLOWING FULL SECTION-->
#### <a name="enable-on-an-existing-disk"></a>在现有磁盘上启用

> [!CAUTION]
> 在附加到 VM 的任何磁盘上启用磁盘加密将需要你停止 VM。

1. 导航到与磁盘加密集位于同一区域中的 VM。
1. 打开 VM 并选择“停止”。

    ![示例 VM 的主覆盖屏幕截图。 其中突出显示了“停止”按钮](media/virtual-machines-disk-encryption-portal/sse-stop-VM-to-encrypt-disk.png)

1. VM 停止后，选择“磁盘”，然后选择要加密的磁盘。

    ![示例 VM 的屏幕截图，其中“磁盘”边栏选项卡处于打开状态。 OS 磁盘突出显示，作为示例磁盘供你选择。](media/virtual-machines-disk-encryption-portal/sse-existing-disk-select.png)

1. 依次选择“加密”、“使用客户托管密钥进行静态加密”，然后在下拉列表中选择“磁盘加密集” 。
1. 选择“保存” 。

    ![示例 OS 磁盘的屏幕截图。 “加密”边栏选项卡处于打开状态，“使用客户托管密钥进行静态加密”处于选中状态以及示例 Azure Key Vault。 完成这些选择后，“保存”按钮处于选中状态。](media/virtual-machines-disk-encryption-portal/sse-encrypt-existing-disk-customer-managed-key.png)

1. 对于附加到你想要加密的 VM 的任何其他磁盘，请重复此过程。
1. 当磁盘完成切换到客户托管密钥后，如果没有其他需要进行加密的附加磁盘，则可以启动 VM。

<!-- Update_Description: new articles of virutal machine disks encryption portal -->