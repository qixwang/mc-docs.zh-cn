---
title: 配置 Power BI 报表
description: 使用恢复服务保管库为 Azure 备份配置 Power BI 报表。
author: lingliw
manager: digimobile
ms.topic: conceptual
origin.date: 09/18/2019
ms.date: 11/20/2019
ms.author: v-lingwu
ms.openlocfilehash: 1501a6ccf3eb173291d8a34f5296c15f3fd362f3
ms.sourcegitcommit: 27eaabd82b12ad6a6840f30763034a6360977186
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2020
ms.locfileid: "77497358"
---
# <a name="configure-azure-backup-reports"></a>配置 Azure 备份报表

本文介绍使用恢复服务保管库为 Azure 备份配置报表所需执行的步骤。 另外还介绍如何通过 Power BI 访问报表。 完成这些步骤后，可直接转到 Power BI，以便查看、自定义和创建报表。

> [!IMPORTANT]
> 从 2018 年 11 月 1 日起，某些客户可能会在 Power BI 的 Azure 备份应用中加载数据时发现问题，其消息指出“我们在 JSON 输入末尾发现额外字符。 此异常由 IDataReader 接口引起。”
这是由于在将数据加载到存储帐户中时，其格式发生了变化。
请下载最新的应用（版本 1.8）以避免此问题。
>
>

## <a name="supported-scenarios"></a>支持的方案
- 使用 Azure 恢复服务代理进行 Azure 虚拟机备份以及将文件和文件夹备份到云时，支持 Azure 备份报表。
- 目前，Azure SQL 数据库、Azure 文件共享、Data Protection Manager 和 Azure 备份服务器不支持报表。
- 如果为每个保管库配置同一存储帐户，可以跨保管库和订阅查看报表。 所选存储帐户必须位于恢复服务保管库所在的区域。
- 在 Power BI 中，报表按计划每 24 小时刷新一次。 也可在 Power BI 中按需刷新报表。 在这种情况下，会使用客户存储帐户中的最新数据来呈现报表。

## <a name="prerequisites"></a>必备条件

- 创建 [Azure 存储帐户](../storage/common/storage-account-create.md)，以便为报表配置此帐户。 此存储帐户用于存储与报表相关的数据。
- [创建 Power BI 帐户](https://powerbi.microsoft.com/landing/signin/)，以便可以使用 Power BI 门户查看、自定义并创建自己的报表。
- 注册资源提供程序 Microsoft.insights  （如果尚未注册）。 将订阅用于存储帐户和恢复服务保管库，以便报表数据可以流向存储帐户。 要执行此步骤，请转到 Azure 门户，选择“订阅”   > “资源提供程序”  ，并找到此提供程序以进行注册。

## <a name="configure-storage-account-for-reports"></a>配置报表的存储帐户
请按以下步骤操作，使用 Azure 门户配置恢复服务保管库的存储帐户。 这是一次性配置。 配置存储帐户后，可以直接转到 Power BI 来查看模板应用和使用报表。

1. 如果已打开恢复服务保管库，请转到下一步。 如果未打开恢复服务保管库，则请在 Azure 门户中选择“所有服务”  。

   - 在资源列表中，输入“恢复服务”  。
   - 开始键入时，会根据输入筛选该列表。 出现“恢复服务保管库”时，请选择它。 
   - 此时显示恢复服务保管库列表。 在恢复服务保管库列表中选择一个保管库。

     此时会打开选定的保管库仪表板。
2. 在保管库下显示的项列表中，选择“监视和报表”部分下的“备份报表”   ，以配置报表的存储帐户。

      ![选择“备份报表”菜单项 - 第 2 步](./media/backup-azure-configure-reports/backup-reports-settings.PNG)
3. 在“备份报表”边栏选项卡上，选择“诊断设置”   链接。 此链接将打开用于将数据推送到客户存储帐户的**诊断设置** UI。

      ![启用诊断 - 第 3 步](./media/backup-azure-configure-reports/backup-azure-configure-reports.png)
4. 选择“启用诊断”，打开用于配置存储帐户的 UI。 

      ![启用诊断 - 第 4 步](./media/backup-azure-configure-reports/enable-diagnostics.png)
5. 在“名称”框中，输入设置名称  。  选择“存档到存储帐户”复选框，这样报表数据就可以开始流向存储帐户。

      ![启用诊断 - 第 5 步](./media/backup-azure-configure-reports/select-setting-name.png)
6. 选择“存储帐户”，接着从列表中选择用于存储报表数据的相关订阅和存储帐户，再选择“确定”   。

      ![选择存储帐户 - 第 6 步](./media/backup-azure-configure-reports/select-subscription-sa.png)
7. 在“日志”部分选中“AzureBackupReport”复选框。   移动滑块，选择此报表数据的保持期。 存储帐户中报表数据的保持期即为使用此滑块选择的时间段。

      ![保存存储帐户 - 第 7 步](./media/backup-azure-configure-reports/save-diagnostic-settings.png)
8. 查看所有更改，然后选择“保存”。  此操作可确保保存所有更改，现在用于存储报表数据的存储帐户已配置完成。

9. “诊断设置”表现在显示为保管库启用的新设置。  如果未显示，请刷新该表以查看更新的设置。

      ![查看诊断设置 - 第 9 步](./media/backup-azure-configure-reports/diagnostic-setting-row.png)

> [!NOTE]
> 通过保存存储帐户完成对报表的配置后，请等待 24 小时，完成初始数据推送  。 仅在此之后才将 Azure 备份应用导入 Power BI。 
>
>




## <a name="troubleshooting-errors"></a>排查错误
| 错误详细信息 | 解决方法 |
| --- | --- |
| 为备份报表设置存储帐户后，“存储帐户”  仍显示“未配置”  。 | 如果已成功配置存储帐户，则即使存在此问题，报表数据仍会流入该帐户。 若要解决此问题，请转到 Azure 门户并选择“所有服务”   > “诊断设置”   > “恢复服务保管库”   > “编辑设置”  。 删除以前配置的设置，然后在同一边栏选项卡中创建新设置。 这次请在“名称”  框中选择“服务”  。 现在，应会显示配置的存储帐户。 |
|在 Power BI 中导入 Azure 备份模板应用后，会显示“404 - 未找到容器”错误消息。 | 如上所述，在恢复服务保管库中配置报表后，必须等待 24 小时，Power BI 中才会正确显示报表。 如果在 24 小时内尝试访问报表，会显示此错误消息，因为尚不存在完整的数据，无法显示有效报表。 |

## <a name="next-steps"></a>后续步骤
配置存储帐户并导入 Azure 备份模板应用后，需在后续步骤中自定义报表，并使用报表数据模型创建报表。 有关详细信息，请参阅以下文章。

* [使用 Azure 备份报表数据模型](backup-azure-reports-data-model.md)
* [在 Power BI 中筛选报表](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
* [在 Power BI 中创建报表](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
