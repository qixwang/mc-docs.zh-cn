---
title: 在 Azure Stack HCI 中创建卷
description: 如何使用 Windows Admin Center 和 PowerShell 在 Azure Stack HCI 中创建卷。
author: WenJason
ms.author: v-jay
ms.topic: article
origin.date: 02/28/2020
ms.date: 03/23/2020
ms.openlocfilehash: 004b3c28b7cf48b3e2f4783a4fcbc0337a75bcdf
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79547108"
---
# <a name="creating-volumes-in-azure-stack-hci"></a>在 Azure Stack HCI 中创建卷

> 适用于：Windows Server 2019

本主题介绍如何使用 Windows Admin Center 和 Windows PowerShell 在存储空间直通群集中创建卷、如何处理卷中的文件，以及如何在卷中启用重复数据删除和压缩。

## <a name="create-a-three-way-mirror-volume"></a>创建三向镜像卷

若要使用 Windows Admin Center 创建三向镜像卷：

1. 在 Windows Admin Center 中连接到存储空间直通群集，然后在“工具”窗格中选择“卷”。  
2. 在“卷”页上选择“库存”选项卡，然后选择“创建卷”。   
3. 在“创建卷”窗格中输入卷的名称，并将“复原”保留为“三向镜像”。   
4. 在“HDD 上的大小”中指定卷的大小。  例如 5 TB。
5. 选择“创建”  。

创建卷可能需要几分钟时间，具体取决于大小。 卷创建好之后，右上方会显示通知。 新卷将显示在“库存”列表中。

## <a name="create-a-mirror-accelerated-parity-volume"></a>创建镜像加速奇偶校验卷

镜像加速奇偶校验可减少卷在 HDD 上的占用空间。 例如，三向镜像卷意味着每 10 TB 的卷大小需要 30 TB 的占用空间。 为了减少占用空间开销，请创建使用镜像加速奇偶校验的卷。 这样，即使只有 4 台服务器，也可以镜像最活跃的 20% 数据，然后使用空间效率更高的奇偶校验来存储剩余数据，从而将占用空间从 30 TB 减到 22 TB。 可以调整奇偶校验和镜像之比，在性能与容量方面做出最适合工作负荷的取舍。 例如，使用 90% 的奇偶校验和 10% 的镜像所带来的性能效果不太理想，但可以进一步优化占用空间。

若要在 Windows Admin Center 创建使用镜像加速奇偶校验的卷：

1. 在 Windows Admin Center 中连接到存储空间直通群集，然后在“工具”窗格中选择“卷”。  
2. 在“卷”页上选择“库存”选项卡，然后选择“创建卷”。  
3. 在“创建卷”窗格中输入卷的名称。 
4. 在“复原”中选择“镜像加速奇偶校验”。  
5. 在“奇偶校验百分比”中选择奇偶校验的百分比。 
6. 选择“创建”  。

## <a name="open-volume-and-add-files"></a>打开卷并添加文件

若要在 Windows Admin Center 中打开卷并将文件添加到卷：

1. 在 Windows Admin Center 中连接到存储空间直通群集，然后在“工具”窗格中选择“卷”。  
2. 在“卷”页上选择“库存”选项卡。  
2. 在卷列表中，选择要打开的卷的名称。

    在卷详细信息页上，可以看到卷的路径。

4. 在页面顶部选择“打开”。  随即会在 Windows Admin Center 中启动“文件”工具。 
5. 导航到卷的路径。 可在此处浏览卷中的文件。
6. 选择“上传”，然后选择要上传的文件。 
7. 使用浏览器中的“后退”按钮返回到 Windows Admin Center 中的“工具”窗格。  

## <a name="turn-on-deduplication-and-compression"></a>启用重复数据删除和压缩

重复数据删除和压缩是根据每个卷进行管理的。 重复数据删除和压缩使用后处理模型，这意味着，在该功能运行之前，你看不到节省的空间。 该功能在运行时会处理所有文件，甚至包括以前就已存在的文件。

1. 在 Windows Admin Center 中连接到存储空间直通群集，然后在“工具”窗格中选择“卷”。  
2. 在“卷”页上选择“库存”选项卡。  
3. 在卷列表中，选择要管理的卷的名称。
4. 在卷详细信息页上，单击标为“重复数据删除和压缩”的开关。 
5. 在“启用重复数据删除”窗格中选择重复数据删除模式。 

    Windows Admin Center 可让你针对不同的工作负荷选择现成的配置文件，而无需进行复杂的设置。 如果你不确定要如何设置，请使用默认设置。

6. 选择“启用”  。

## <a name="create-volumes-using-windows-powershell"></a>使用 Windows PowerShell 创建卷

首先，从 Windows 的“开始”菜单启动 Windows PowerShell。 建议使用 **New-Volume** cmdlet 来为 Azure Stack HCI 创建卷。 此 cmdlet 提供最快且最直接的体验。 仅凭这一个 cmdlet 就能自动创建虚拟磁盘、分区（并将其格式化），以及创建具有匹配名称的卷，并将其添加到群集共享卷 – 一个简单的步骤即可完成所有这些操作。

**New-Volume** cmdlet 有四个必需的参数：

- **FriendlyName：** 所需的任何字符串，例如“Volume1” 
- **FileSystem：** **CSVFS_ReFS**（建议）或 **CSVFS_NTFS**
- **StoragePoolFriendlyName：** 存储池的名称，例如“S2D on ClusterName” 
- **Size：** 卷的大小，例如“10TB” 

   > [!NOTE]
   > Windows（包括 PowerShell）使用二进制（以 2 为基数）数字进行计数，而驱动器通常是使用十进制（以 10 为基数）数字进行标记。 正因如此，定义为 1,000,000,000,000 字节的“1 TB”驱动器在 Windows 中显示为大约“909 GB”。 这是正常情况。 使用 **New-Volume** 创建卷时，应以二进制（以 2 为基数）数字指定 **Size** 参数。 例如，指定“909GB”或“0.909495TB”会创建大约 1,000,000,000,000 字节的卷。

### <a name="example-with-2-or-3-servers"></a>示例：使用 2 台或 3 台服务器

为简便起见，如果部署只包含两台服务器，则存储空间直通自动使用双向镜像来实现复原功能。 如果部署只包含三台服务器，则存储空间直通会自动使用三向镜像。

```PowerShell
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>示例：使用 4 台或更多服务器

如果你有四台或更多的服务器，可以使用可选的 **ResiliencySettingName** 参数来选择复原类型。

-   **ResiliencySettingName：** **Mirror** 或 **Parity**。

在以下示例中，“Volume2”使用三向镜像，而“Volume3”使用双重奇偶校验（通常称作“擦除编码”）。  

```PowerShell
New-Volume -FriendlyName "Volume2" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Mirror
New-Volume -FriendlyName "Volume3" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Parity
```

### <a name="example-using-storage-tiers"></a>示例：使用存储层

在包含三种类型的驱动器的部署中，一个卷可以跨越 SSD 和 HDD 层，该卷各有一部分驻留在每个驱动器上。 同理，在包含四台或更多服务器的部署中，一个卷可以混合使用镜像和双重奇偶校验，该卷各有一部分驻留在每台服务器上。

为了帮助你创建此类卷，存储空间直通提供称为“性能”和“容量”的默认层模板。   这些模板在较快的容量驱动器（如果适用）上封装三向镜像的定义，并在较慢的容量驱动器（如果适用）上封装双重奇偶校验的定义。

可以运行 **Get-StorageTier** cmdlet 来查看这些层。

```PowerShell
Get-StorageTier | Select FriendlyName, ResiliencySettingName, PhysicalDiskRedundancy
```

![存储层 PowerShell 屏幕截图](media/creating-volumes/storage-tiers-screenshot.png)

若要创建分层卷，请使用 **New-Volume** cmdlet 的 **StorageTierFriendlyNames** 和 **StorageTierSizes** 参数引用这些层模板。 例如，以下 cmdlet 创建一个以 30:70 比例混合三向镜像和双重奇偶校验的卷。

```PowerShell
New-Volume -FriendlyName "Volume4" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 300GB, 700GB
```

大功告成！ 根据需要重复上述步骤创建多个卷。

## <a name="next-steps"></a>后续步骤

若要在存储空间直通中执行其他存储管理任务，另请参阅：

- [存储空间直通概述](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [规划存储空间直通中的卷](https://docs.microsoft.com/windows-server/storage/storage-spaces/plan-volumes)
- [扩展存储空间直通中的卷](https://docs.microsoft.com/windows-server/storage/storage-spaces/resize-volumes)
- [删除存储空间直通中的卷](https://docs.microsoft.com/windows-server/storage/storage-spaces/delete-volumes)