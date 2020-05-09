---
title: 通过 Azure SQL 托管实例代理执行 SSIS 包
description: 了解如何通过 Azure SQL 托管实例代理执行 SSIS 包。
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: v-jay
author: WenJason
origin.date: 04/14/2020
ms.date: 05/11/2020
ms.openlocfilehash: 3ac5476406644b3405c3db92f714a2a5e84dc564
ms.sourcegitcommit: f8d6fa25642171d406a1a6ad6e72159810187933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82198454"
---
# <a name="execute-ssis-packages-by-azure-sql-managed-instance-agent"></a>通过 Azure SQL 托管实例代理执行 SSIS 包
本文介绍如何使用 Azure SQL 托管实例代理运行 SQL Server Integration Services (SSIS) 包。 此功能的行为类似于在本地环境中通过 SQL Server 代理计划 SSIS 包。

使用此功能可以运行存储在 Azure SQL 托管实例 SSISDB 或文件系统（例如 Azure 文件存储）中的 SSIS 包。

## <a name="prerequisites"></a>先决条件
若要使用此功能，请下载并安装最新版本的 SSMS（18.5 或更高版本）。 从[此网站](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)下载它。

此外，需要在 Azure 数据工厂中预配一个使用 Azure SQL 托管实例作为终结点服务器的 Azure-SSIS Integration Runtime。 如果尚未预配，请按照[教程](tutorial-create-azure-ssis-runtime-portal.md)中的说明进行预配。 

## <a name="run-ssis-packages-in-ssisdb-by-azure-sql-managed-instance-agent"></a>通过 Azure SQL 托管实例代理运行 SSISDB 中的 SSIS 包
在此步骤中，我们使用 Azure SQL 托管实例代理来调用存储在 Azure SQL 托管实例的 SSISDB 中的 SSIS 包。
1. 在最新版 SSMS 中，连接到 Azure SQL 托管实例。
2. 创建新的代理作业和新的作业步骤。

![新建代理作业](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

3. 在“新建作业步骤”页中，选择“SQL Server Integration Services 包”类型   。

![新建 SSIS 作业步骤](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

4. 在“包”选项卡中，选择“SSIS 目录”作为包源类型   。
5. 由于 SSISDB 位于同一个 Azure SQL 托管实例中，因此不需要指定身份验证。
6. 指定 SSISDB 中的某个 SSIS 包。

![包源类型 - SSIS 目录](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb.png)

7. 在“配置”选项卡中，可以指定“参数”值，替代“连接管理器”中的值，替代“属性”，然后选择“日志记录级别”      。

![包源类型 - SSIS 目录配置](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb-configuration.png)

8. 完成上述所有配置后，单击“确定”保存代理作业配置  。
9. 启动代理作业以执行 SSIS 包。


## <a name="run-ssis-packages-in-file-system-by-azure-sql-managed-instance-agent"></a>通过 Azure SQL 托管实例代理运行文件系统中的 SSIS 包
在此步骤中，我们使用 Azure SQL 托管实例代理来调用存储在文件系统中的要运行的 SSIS 包。
1. 在最新版 SSMS 中，连接到 Azure SQL 托管实例。
2. 创建新的代理作业和新的作业步骤。

   ![新建代理作业](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

3. 在“新建作业步骤”页中，选择“SQL Server Integration Services 包”类型   。

   ![新建 SSIS 作业步骤](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

4. 在“包”选项卡中，选择“文件系统”作为包源类型   。

   ![包源类型 - 文件系统](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-file-system.png)

   1. 如果包已上传到 Azure 文件存储，请选择“Azure 文件共享”作为文件源类型  。
      - 包路径为 \\<storage account name>.file.core.chinacloudapi.cn\<文件共享名称>\<包名>.dtsx 
      - 在“包文件访问凭据”中键入 Azure 文件存储帐户名称和帐户密钥，以访问 Azure 文件存储  。 域设置为 Azure  。
   2. 如果包已上传到网络共享，请选择“网络共享”为文件源类型  。
      - 包路径是带有 dtsx 扩展名的包文件的 UNC 路径  。
      - 键入相应的域、用户名和密码以访问网络共享包文件    。
   3. 如果包文件是使用密码加密的，请选择“加密密码”并键入密码  。

 5. 在“配置”选项卡中，如果需要通过配置文件来执行 SSIS 包，请键入配置文件路径   。
 6. 在“执行选项”选项卡中，可以选择是要使用 Windows 身份验证还是 32 位运行时来执行 SSIS 包    。
 7. 在“日志记录”选项卡中，可以选择日志记录路径和相应的日志记录访问凭据来存储日志文件   。 默认情况下，日志记录路径与包文件夹路径相同，日志记录访问凭据与包访问凭据相同。
 8. 在“设置值”选项卡中，可以键入“属性路径”和“值”来替代包属性    。
 例如，若要替代用户变量的值，请按以下格式输入其路径：\Package.Variables[User::<variable name>].Value  。
 9. 完成上述所有配置后，单击“确定”保存代理作业配置  。
 10. 启动代理作业以执行 SSIS 包。


 ## <a name="cancel-ssis-package-execution"></a>取消 SSIS 包执行
 若要从 Azure SQL 托管代理作业中取消包执行，应执行以下步骤，而不应直接停止代理作业。
 1. 从 msdb.dbo.sysjobs 中找到 SQL 代理的 jobId   。
 2. 通过以下查询基于作业 ID 找到相应的 SSIS executionId  ：
    ```sql
    select * from ssisdb.internal.execution_parameter_values_noncatalog where  parameter_value = 'SQL_Agent_Job_{jobId}' order by execution_id desc
    ```
 3. 选择 SSIS 目录下的“活动操作”  。

    ![包源类型 - 文件系统](./media/how-to-invoke-ssis-package-managed-instance-agent/catalog-active-operations.png)

 4. 基于 executionId 停止相应的操作  。

## <a name="next-steps"></a>后续步骤
 还可以使用 Azure 数据工厂来计划 SSIS 包。 有关分步说明，请参阅 [Azure 数据工厂事件触发器](how-to-create-event-trigger.md)。 