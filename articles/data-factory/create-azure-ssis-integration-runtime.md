---
title: 在 Azure 数据工厂中创建 Azure-SSIS 集成运行时
description: 了解如何在 Azure 数据工厂中创建 Azure-SSIS 集成运行时，以便能够在 Azure 中部署和运行 SSIS 包。
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
origin.date: 12/23/2019
ms.date: 01/06/2020
author: WenJason
ms.author: v-jay
ms.reviewer: douglasl
manager: digimobile
ms.openlocfilehash: a51e2e6bbe793a1b548eeaa60be9e0e7fdfa2bc9
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75624076"
---
# <a name="create-an-azure-ssis-integration-runtime-in-azure-data-factory"></a>在 Azure 数据工厂中创建 Azure-SSIS 集成运行时

本文提供在 Azure 数据工厂中预配 Azure-SQL Server Integration Services (SSIS) 集成运行时 (IR) 的步骤。 Azure-SSIS IR 支持：

- 运行部署在由 Azure SQL 数据库服务器承载的 SSIS 目录 (SSISDB) 中的包。
- 运行部署在文件系统、文件共享或 Azure 文件存储中的包（包部署模型）。 

预配 Azure-SSIS IR 后，可以使用熟悉的工具在 Azure 中部署和运行包。 这些工具包括 SQL Server Data Tools (SSDT)、SQL Server Management Studio (SSMS) 和命令行工具（例如 `dtinstall`、`dtutil` 和 `dtexec`）。

[预配 Azure-SSIS IR](tutorial-create-azure-ssis-runtime-portal.md) 教程介绍了如何通过 Azure 门户或数据工厂应用创建 Azure-SSIS IR。 本教程还将介绍如何选择性地使用 Azure SQL 数据库服务器。 本文对该教程的基础上有所延伸，介绍如何执行以下可选任务：

- 使用包含 IP 防火墙规则/虚拟网络服务终结点的 Azure SQL 数据库服务器来承载 SSISDB。 作为先决条件，需要配置虚拟网络权限和设置，才能让 Azure-SSIS IR 加入虚拟网络。

- 对数据工厂的托管标识使用 Azure Active Directory (Azure AD) 身份验证，以连接到 Azure SQL 数据库服务器。 作为先决条件，需要将数据工厂的托管标识添加为可创建 SSISDB 实例的数据库用户。

- 将 Azure-SSIS IR 加入虚拟网络，或将自承载 IR 配置为代理，使 Azure-SSIS IR 能够访问本地数据。

本文将介绍如何使用 Azure 门户、Azure PowerShell 和 Azure 资源管理器模板来预配 Azure-SSIS IR。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure 订阅**。 如果没有订阅，可以创建一个 [1 元试用](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)帐户。

- **Azure SQL 数据库服务器（可选）** 。 如果还没有数据库服务器，请在启动之前在 Azure 门户中创建一个。 数据工厂进而会在此数据库服务器上创建一个 SSISDB 实例。 

  建议在集成运行时所在的同一 Azure 区域中创建数据库服务器。 此配置允许集成运行时将执行日志写入 SSISDB 而无需跨 Azure 区域。

  请记住以下几点：

  - 根据所选的数据库服务器，系统可以代表你创建 SSISDB 实例作为弹性池中的单一数据库。 可以在公用网络中访问或者通过加入虚拟网络来访问该实例。
  
    如果使用包含 IP 防火墙规则/虚拟网络服务终结点的 Azure SQL 数据库服务器来承载 SSISDB，或者需要在未配置自承载 IR 的情况下访问本地数据，则需要将 Azure-SSIS IR 加入虚拟网络。 有关详细信息，请参阅[将 Azure-SSIS IR 加入虚拟网络](/data-factory/join-azure-ssis-integration-runtime-virtual-network)。

  - 确认为数据库服务器启用了“允许访问 Azure 服务”设置。  使用包含 IP 防火墙规则/虚拟网络服务终结点的 Azure SQL 数据库服务器来承载 SSISDB 时，此设置不适用。 有关详细信息，请参阅[保护 Azure SQL 数据库](../sql-database/sql-database-security-tutorial.md#create-firewall-rules)。 若要通过 PowerShell 来启用此设置，请参阅 [New-AzSqlServerFirewallRule](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserverfirewallrule)。

  - 将客户端计算机的 IP 地址或一系列包括客户端计算机 IP 地址的 IP 地址添加到数据库服务器的防火墙设置中的客户端 IP 地址列表。 有关详细信息，请参阅 [Azure SQL 数据库服务器级和数据库级防火墙规则](../sql-database/sql-database-firewall-configure.md)。

  - 若要连接到数据库服务器，可以结合服务器管理员凭据使用 SQL 身份验证，或者结合数据工厂的托管标识使用 Azure AD 身份验证。 对于后者，需将数据工厂的托管标识添加到有权访问数据库服务器的 Azure AD 组中。 有关详细信息，请参阅[为 Azure-SSIS IR 启用 Azure AD 身份验证](/data-factory/enable-aad-authentication-azure-ssis-ir)。

  - 确认你的数据库服务器还没有 SSISDB 实例。 预配 Azure-SSIS IR 时不支持使用现有的 SSISDB 实例。

- **Azure 资源管理器虚拟网络（可选）** 。 如果下列条件中至少有一个属实，则必须配置 Azure 资源管理器虚拟网络：
  - 在包含 IP 防火墙规则/虚拟网络服务终结点的 Azure SQL 数据库服务器上承载 SSISDB。
  - 需要从 SSIS 包连接到本地据存储，该包运行在没有配置自承载 IR 的 Azure-SSIS IR 上。

- **Azure PowerShell（可选）** 。 若要运行 PowerShell 脚本来预配 Azure-SSIS IR，请按照[如何安装和配置 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps) 中的说明进行操作。

### <a name="regional-support"></a>区域支持

有关提供数据工厂和 Azure-SSIS IR 的 Azure 区域列表，请参阅[数据工厂和 SSIS IR 在各区域的上市情况](https://azure.microsoft.com/global-infrastructure/services/?regions=china-non-regional,china-east,china-east-2,china-north,china-north-2&products=data-factory)。

## <a name="use-the-azure-portal-to-create-an-integration-runtime"></a>使用 Azure 门户创建集成运行时

在本部分，你将使用 Azure 门户（具体而言，使用数据工厂用户界面 (UI) 或应用）创建 Azure-SSIS IR。

### <a name="create-a-data-factory"></a>创建数据工厂

若要通过 Azure 门户创建数据工厂，请按照[通过 UI 创建数据工厂](/data-factory/quickstart-create-data-factory-portal#create-a-data-factory)中的分步说明操作。 执行此操作时，请选择“固定到仪表板”，以便在创建后能够快速访问数据工厂。  

创建数据工厂后，在 Azure 门户中打开其概述页。 选择“创建和监视”磁贴，在单独的选项卡中打开该数据工厂的“开始”页。   然后可以继续创建 Azure-SSIS IR。   

### <a name="provision-an-azure-ssis-integration-runtime"></a>预配 Azure-SSIS 集成运行时

1. 在“入门”页中，选择“配置 SSIS 集成运行时”磁贴。  

   ![“配置 Azure SSIS 集成运行时”磁贴](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. 在“集成运行时安装”面板的“常规设置”部分完成以下步骤。  

   ![常规设置](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   1. 对于“名称”，请输入集成运行时的名称。 

   1. 对于“说明”，请输入集成运行时的说明。 

   1. 对于“位置”，请选择集成运行时的位置。  界面上仅显示支持的位置。 建议选择承载 SSISDB 所需的数据库服务器的位置。

   1. 对于“节点大小”，请选择集成运行时群集中的节点大小  。 仅显示支持的节点大小。 如果需要运行多个计算密集型或内存密集型包，请选择较大的节点大小（纵向扩展）。

   1. 对于“节点数”，请选择集成运行时群集中的节点数  。 仅显示支持的节点数。 如果需要并行运行多个包，请选择包含许多节点的大型群集（横向扩展）。

   1. 对于“版本/许可证”，请选择集成运行时的 SQL Server 版本：  “标准”或“企业”。 如果需要在集成运行时上使用高级功能，请选择“Enterprise”。

   1. 对于“节省资金”，请选择适用于集成运行时的“Azure 混合权益”选项：  “是”或“否”。   如果需要自带具有软件保障的 SQL Server 许可证，以便充分利用使用混合权益带来的成本节省，请选择“是”。 

   1. 选择“**下一步**”。

1. 在“SQL 设置”部分完成以下步骤。 

   ![SQL 设置](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

   1. 选中“创建 SSIS 目录...”复选框，选择要在 Azure-SSIS IR 中运行的包的部署模型。  选择“项目部署模型”（其中的包将部署到数据库服务器承载的 SSISDB）或“包部署模型”（其中的包将部署到文件系统、文件共享或 Azure 文件）。 
    
      如果选中该复选框，需要提供自己的数据库服务器来承载我们将代表你创建和管理的 SSISDB 实例。
   
      1. 对于“订阅”，请选择使用数据库服务器来托管 SSISDB 的 Azure 订阅。  

      1. 对于“位置”，请选择用于托管 SSISDB 的数据库服务器的位置。  建议选择集成运行时的位置。 

      1. 对于“目录数据库服务器终结点”，请选择用于承载 SSISDB 的数据库服务器的终结点。  
    
         根据所选的数据库服务器，系统可以代表你创建 SSISDB 实例作为弹性池中的单一数据库。 可以在公用网络中访问或者通过加入虚拟网络来访问该实例。
    
         如果选择包含 IP 防火墙规则/虚拟网络服务终结点的 Azure SQL 数据库服务器来承载 SSISDB，或者需要在未配置自承载 IR 的情况下访问本地数据，则需要将 Azure-SSIS IR 加入虚拟网络。 有关详细信息，请参阅[将 Azure-SSIS IR 加入虚拟网络](/data-factory/join-azure-ssis-integration-runtime-virtual-network)。 

      1. 选中“结合 ADF 的托管标识使用 AAD 身份验证”复选框，选择数据库服务器用来承载 SSISDB 的身份验证方法。  选择使用数据工厂的托管标识进行 SQL 身份验证或 Azure AD 身份验证。 
    
         如果选中该复选框，需将数据工厂的托管标识添加到有权访问数据库服务器的 Azure AD 组中。 有关详细信息，请参阅[为 Azure-SSIS IR 启用 Azure AD 身份验证](/data-factory/enable-aad-authentication-azure-ssis-ir)。 

      1. 对于“管理员用户名”，请输入用于承载 SSISDB 的数据库服务器的 SQL 身份验证用户名。  

      1. 对于“管理员密码”，请输入用于承载 SSISDB 的数据库服务器的 SQL 身份验证密码。  

      1. 对于“目录数据库服务层级”，请选择用于承载 SSISDB 的数据库服务器的服务层级。  选择“基本”、“标准”或“高级”层级，或选择弹性池名称。 

      1. 选择“测试连接”  。 如果测试成功，请选择“下一步”。  

1. 在“高级设置”部分完成以下步骤。 

   ![高级设置](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   1. 对于“每个节点的最大并行执行数”，请选择要在集成运行时群集中并发运行的最大包数（按节点）。  仅显示支持的包数。 如果需要使用多个核心来运行单个计算密集型或内存密集型的大型包，请选择较小的数字。 如果需要在单个核心中运行一个或多个小型包，请选择较大的数字。

   1. 选中“使用其他系统配置/组件安装自定义 Azure-SSIS Integration Runtime”复选框，以选择是否要在 Azure-SSIS IR 中添加标准/快速自定义安装。  有关详细信息，请参阅 [Azure-SSIS IR 的自定义安装](/data-factory/how-to-configure-azure-ssis-ir-custom-setup)。

      如果选中该复选框，请完成以下步骤。

      ![包含自定义安装的高级设置](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-custom.png)
   
      1. 对于“自定义安装容器 SAS URI”，请输入用于存储标准自定义安装脚本和关联文件的容器的 SAS URI。 

      1. 对于“快速自定义安装”，请选择“新建”打开“添加快速自定义安装”面板，然后在“快速自定义安装类型”下拉菜单中选择任何类型，例如“运行 cmdkey 命令”、“添加环境变量”、“安装许可的组件”等。       

         如果选择“安装许可的组件”类型，则可以在“组件名称”下拉菜单中选择我们的 ISV 合作伙伴提供的任何集成组件；如果需要，请在“许可密钥”字段中输入从合作伙伴购买的产品许可密钥。   
  
         添加的快速自定义安装将显示在“高级设置”部分  。 若要删除它们，可以选中其复选框，然后选择“删除”  。

   1. 选中“选择 Azure-SSIS 集成运行时要加入到的 VNet，允许 ADF 创建特定的网络资源，并提供自己的静态公共 IP 地址(可选)”复选框，选择是否要将集成运行时加入虚拟网络。  

      如果使用包含 IP 防火墙规则/虚拟网络服务终结点的 Azure SQL 数据库服务器来承载 SSISDB，或者需要在不配置自承载 IR 的情况下访问本地数据（即，SSIS 包中包含本地数据源或目标），请选择此项。 有关详细信息，请参阅[将 Azure-SSIS IR 加入虚拟网络](/data-factory/join-azure-ssis-integration-runtime-virtual-network)。 

      如果选中该复选框，请完成以下步骤。

      ![使用虚拟网络的高级设置](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

      1. 对于“订阅”，请选择包含你的虚拟网络的 Azure 订阅。 

      1. 对于“位置”，系统已选择集成运行时所在的位置。 

      1. 对于“类型”，请选择虚拟网络的类型：“经典”或“Azure 资源管理器”。  我们建议选择 Azure 资源管理器虚拟网络，因为经典虚拟网络在不久后将被弃用。

      1. 对于“VNet 名称”，请选择虚拟网络的名称。  它应该包含用于承载 SSISDB 的虚拟网络服务终结点的 Azure SQL 数据库服务器所用的虚拟网络相同。 或者，它应该是连接到本地网络的同一个虚拟网络。 否则，它可以是为 Azure-SSIS IR 提供你自己的静态公共 IP 地址的任何虚拟网络。

      1. 对于“子网名称”，请选择虚拟网络的子网名称。  它应该包含用于承载 SSISDB 的虚拟网络服务终结点的 Azure SQL 数据库服务器所用的虚拟网络相同。 否则，它可以是为 Azure-SSIS IR 提供你自己的静态公共 IP 地址的任何子网。

      1. 选中“为 Azure-SSIS Integration Runtime 提供静态公共 IP 地址”复选框，以选择是否要为 Azure-SSIS IR 提供自己的静态公共 IP 地址，以便可以在数据源的防火墙中允许这些地址。 

         如果选中该复选框，请完成以下步骤。

         1. 对于“第一个静态公共 IP 地址”，请选择符合 Azure-SSIS IR 的要求的第一个静态公共 IP 地址。  如果没有任何符合要求的 IP 地址，请单击“新建”链接以在 Azure 门户中创建静态公共 IP 地址，然后单击此处的刷新按钮，以便可以选择创建的地址。 
      
         1. 对于“第二个静态公共 IP 地址”，请选择符合 Azure-SSIS IR 的要求的第二个静态公共 IP 地址。  如果没有任何符合要求的 IP 地址，请单击“新建”链接以在 Azure 门户中创建静态公共 IP 地址，然后单击此处的刷新按钮，以便可以选择创建的地址。 

   1. 选择“VNet 验证” > “继续”。   

1. 在“摘要”部分检查所有预配设置，将建议的文档链接添加为书签，然后选择“完成”开始创建集成运行时。  

   > [!NOTE]
   > 此过程应在 5 分钟内完成（不包括任何自定义安装时间）。 但是，Azure-SSIS IR 可能需要 20-30 分钟才能加入虚拟网络。
   >
   > 如果使用 SSISDB，数据工厂服务将连接到数据库服务器以准备 SSISDB。 它还会配置虚拟网络的权限和设置（如果已指定），并将 Azure-SSIS IR 加入虚拟网络中。
   > 
   > 预配 Azure-SSIS IR 时，还会安装 Access Redistributable 和 Azure Feature Pack for SSIS。 除了内置组件已支持的数据源外，这些组件还提供与 Excel 文件、Access 文件和各种 Azure 数据源的连接。 有关可安装的其他组件的信息，请参阅 [Azure-SSIS IR 的自定义安装](how-to-configure-azure-ssis-ir-custom-setup.md)。

1. 在“连接”选项卡中，根据需要切换到“集成运行时”。   选择“刷新”可刷新状态。 

   ![创建状态](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

1. 使用“操作”列中的链接可以停止/启动、编辑或删除集成运行时。  使用最后一个链接可以查看集成运行时的 JSON 代码。 仅当 IR 已停止时，才会启用编辑和删除按钮。

   ![Azure SSIS IR 操作](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>门户中的 Azure SSIS 集成运行时

1. 在 Azure 数据工厂 UI 中切换到“编辑”选项卡，选择“连接”。   然后切换到“集成运行时”选项卡以查看数据工厂中的现有集成运行时。 

   ![查看现有 IR](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. 选择“新建”创建新的 Azure-SSIS IR。 

   ![通过菜单创建集成运行时](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. 在“集成运行时安装”面板中选择“直接迁移现有的 SSIS 包以在 Azure 中执行”磁贴，然后选择“下一步”。   

   ![指定集成运行时的类型](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. 请参阅[预配 Azure SSIS 集成运行时](#provision-an-azure-ssis-integration-runtime)部分，了解用于设置 Azure-SSIS IR 的剩余步骤。

## <a name="use-azure-powershell-to-create-an-integration-runtime"></a>使用 Azure PowerShell 创建集成运行时

在本部分，你将使用 Azure PowerShell 来创建 Azure-SSIS IR。

### <a name="create-variables"></a>创建变量

复制并粘贴以下脚本。 指定变量的值。 

```powershell
### Azure Data Factory information
# If your input contains a PSH special character like "$", precede it with the escape character "`" - for example, "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?regions=china-non-regional,china-east,china-east-2,china-north,china-north-2&products=data-factory
$DataFactoryLocation = "ChinaEast2"

### Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages.
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?regions=china-non-regional,china-east,china-east-2,china-north,china-north-2&products=data-factory
$AzureSSISLocation = "ChinaEast2"
# For supported node sizes, see https://azure.cn/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, whereas Enterprise lets you use advanced features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, whereas BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported. For other nodes, up to (2 x number of cores) are currently supported.
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide the SAS URI of the blob container where your custom setup script and its associated files are stored
# Virtual network info: classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use an Azure SQL Database server with virtual network service endpoints, or if you require on-premises data without configuring a self-hosted IR. We recommend an Azure Resource Manager virtual network, because classic virtual networks will be deprecated soon.
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Use the same subnet as the one used for your Azure SQL Database server with virtual network service endpoints

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.chinacloudapi.cn or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, ensure that there's no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure AD
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for Azure AD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for Azure AD authentication]"
# For the basic pricing tier, specify "Basic," not "B." For standard, premium, and elastic pool tiers, specify "S0," "S1," "S2," "S3," etc. See https://docs.azure.cn/sql-database/sql-database-resource-limits-database-server.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server]"
```

### <a name="sign-in-and-select-a-subscription"></a>登录并选择订阅

添加以下脚本，以登录并选择 Azure 订阅。

```powershell
Connect-AzAccount -Environment AzureChinaCloud
Select-AzSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database-server"></a>验证与数据库服务器的连接

添加以下脚本来验证 Azure SQL 数据库服务器。

```powershell
# Validate only if you use SSISDB and you don't use virtual network or Azure AD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
    {
        if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
        {
            $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword
            $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
            Try
            {
                $sqlConnection.Open();
            }
            Catch [System.Data.SqlClient.SqlException]
            {
                Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
                Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
                $yn = Read-Host
                if(!($yn -ieq "Y"))
                {
                    Return;
                }
            }
        }
    }
}
```

### <a name="configure-the-virtual-network"></a>配置虚拟网络

添加以下脚本以自动配置加入 Azure-SSIS 集成运行时的虚拟网络权限和设置。

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>创建资源组

使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 命令创建 [Azure 资源组](../azure-resource-manager/resource-group-overview.md)。 资源组是在其中以组的形式部署和管理 Azure 资源的逻辑容器。

如果资源组已存在，请不要将此代码复制到脚本中。 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>创建数据工厂

运行以下命令创建数据工厂。

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>创建集成运行时

运行以下命令，在 Azure 中创建运行 SSIS 包的 Azure-SSIS 集成运行时：

如果不使用 SSISDB，则可以省略 `CatalogServerEndpoint`、`CatalogPricingTier` 和 `CatalogAdminCredential` 参数。

如果不使用包含 IP 防火墙规则/虚拟网络服务终结点的 Azure SQL 数据库服务器来承载 SSISDB，或者需要访问本地数据，则可以省略 `VNetId` 和 `Subnet` 参数，或传递这些参数的空值。 否则不能省略这些参数，且必须传递虚拟网络配置中的有效值。 有关详细信息，请参阅[将 Azure-SSIS IR 加入虚拟网络](/data-factory/join-azure-ssis-integration-runtime-virtual-network)。

不能省略此参数，且必须传递 Azure SQL 数据库支持的定价层列表中的有效值。 有关详细信息，请参阅 [SQL 数据库资源限制](../sql-database/sql-database-resource-limits.md)。

如果对数据工厂的托管标识使用 Azure AD 身份验证以连接到数据库服务器，则可以省略 `CatalogAdminCredential` 参数。 但必须将数据工厂的托管标识添加到有权访问数据库服务器的 Azure AD 组中。 有关详细信息，请参阅[为 Azure-SSIS IR 启用 Azure AD 身份验证](/data-factory/enable-aad-authentication-azure-ssis-ir)。 否则，不能忽略此参数，且必须传递由 SQL 身份验证的服务器管理员用户名和密码构成的有效对象。

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add the CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add the CatalogAdminCredential parameter if you don't use Azure AD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add the SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

### <a name="start-the-integration-runtime"></a>启动集成运行时

运行以下命令以启动 Azure-SSIS Integration Runtime。

```powershell
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

> [!NOTE]
> 此过程应在 5 分钟内完成（不包括任何自定义安装时间）。 但是，Azure-SSIS IR 可能需要 20-30 分钟才能加入虚拟网络。
>
> 如果使用 SSISDB，数据工厂服务将连接到数据库服务器以准备 SSISDB。 它还会配置虚拟网络的权限和设置（如果已指定），并将 Azure-SSIS IR 加入虚拟网络中。
> 
> 预配 Azure-SSIS IR 时，还会安装 Access Redistributable 和 Azure Feature Pack for SSIS。 除了内置组件已支持的数据源外，这些组件还提供与 Excel 文件、Access 文件和各种 Azure 数据源的连接。 有关可安装的其他组件的信息，请参阅 [Azure-SSIS IR 的自定义安装](how-to-configure-azure-ssis-ir-custom-setup.md)。

### <a name="full-script"></a>完整脚本

下面是创建 Azure-SSIS 集成运行时的完整脚本。

```powershell
### Azure Data Factory information
# If your input contains a PSH special character like "$", precede it with the escape character "`" - for example, "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?regions=china-non-regional,china-east,china-east-2,china-north,china-north-2&products=data-factory
$DataFactoryLocation = "ChinaEast2"

### Azure-SSIS integration runtime information. This is a Data Factory compute resource for running SSIS packages.
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?regions=china-non-regional,china-east,china-east-2,china-north,china-north-2&products=data-factory
$AzureSSISLocation = "ChinaEast2"
# For supported node sizes, see https://azure.cn/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, whereas Enterprise lets you use advanced features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, whereas BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from the Azure Hybrid Benefit option
# For a Standard_D1_v2 node, up to four parallel executions per node are supported. For other nodes, up to (2 x number of cores) are currently supported.
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide the SAS URI of the blob container where your custom setup script and its associated files are stored
# Virtual network info: classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use an Azure SQL Database server with virtual network service endpoints, or if you require on-premises data without configuring a self-hosted IR. We recommend an Azure Resource Manager virtual network, because classic virtual networks will be deprecated soon.
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Use the same subnet as the one used for your Azure SQL Database server with virtual network service endpoints

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.chinacloudapi.cn or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, ensure that there's no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure AD
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for Azure AD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for Azure AD authentication]"
# For the basic pricing tier, specify "Basic," not "B." For standard, premium, and elastic pool tiers, specify "S0," "S1," "S2," "S3," etc. See https://docs.azure.cn/sql-database/sql-database-resource-limits-database-server.
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server]"

### Sign in and select a subscription
Connect-AzAccount -Environment AzureChinaCloud
Select-AzSubscription -SubscriptionName $SubscriptionName

### Validate the connection to the database server
# Validate only if you use SSISDB and don't use a virtual network or Azure AD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
    {
        if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
        {
            $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword
            $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
            Try
            {
                $sqlConnection.Open();
            }
            Catch [System.Data.SqlClient.SqlException]
            {
                Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
                Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
                $yn = Read-Host
                if(!($yn -ieq "Y"))
                {
                    Return;
                }
            }
        }
    }
}

### Configure a virtual network
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}

### Create a data factory
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add the CatalogAdminCredential parameter if you don't use Azure AD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add the SetupScriptContainerSasUri parameter when you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

### Start the integration runtime
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="use-an-azure-resource-manager-template-to-create-an-integration-runtime"></a>使用 Azure 资源管理器模板创建集成运行时

在本部分，你将使用 Azure 资源管理器模板创建 Azure-SSIS 集成运行时。 下面是示例演练：

1. 使用以下 Azure 资源管理器模板创建一个 JSON 文件。 请将尖括号中的值（占位符）替换为自己的值。

    ```json
    {
        "contentVersion": "1.0.0.0",
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2018-06-01",
            "type": "Microsoft.DataFactory/factories",
            "location": "China East 2",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for your Azure-SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "China East 2",
                            "nodeSize": "Standard_D8_v3",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 8
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL Database server name>.database.chinacloudapi.cn",
                                "catalogAdminUserName": "<Azure SQL Database server admin username>",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Database server admin password>"
                                },
                                "catalogPricingTier": "Basic"
                            }
                        }
                    }
                }
            }]
        }]
    }
    ```

2. 若要部署 Azure 资源管理器模板，请按以下示例所示运行 `New-AzResourceGroupDeployment` 命令。 在此示例中，`ADFTutorialResourceGroup` 是资源组的名称。 `ADFTutorialARM.json` 是包含数据工厂和 Azure-SSIS IR 的 JSON 定义的文件。

    ```powershell
    New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    此命令将创建数据工厂并在其中创建 Azure-SSIS IR，但不会启动 IR。

3. 若要启动 Azure-SSIS IR，请运行 `Start-AzDataFactoryV2IntegrationRuntime` 命令：

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ```

> [!NOTE]
> 此过程应在 5 分钟内完成（不包括任何自定义安装时间）。 但是，Azure-SSIS IR 可能需要 20-30 分钟才能加入虚拟网络。
>
> 如果使用 SSISDB，数据工厂服务将连接到数据库服务器以准备 SSISDB。 它还会配置虚拟网络的权限和设置（如果已指定），并将 Azure-SSIS IR 加入虚拟网络中。
> 
> 预配 Azure-SSIS IR 时，还会安装 Access Redistributable 和 Azure Feature Pack for SSIS。 除了内置组件已支持的数据源外，这些组件还提供与 Excel 文件、Access 文件和各种 Azure 数据源的连接。 有关可安装的其他组件的信息，请参阅 [Azure-SSIS IR 的自定义安装](how-to-configure-azure-ssis-ir-custom-setup.md)。

## <a name="deploy-ssis-packages"></a>部署 SSIS 包

如果使用 SSISDB，可将包部署到其中，并使用 SQL Server Data Tools (SSDT) 或 SQL Server Management Studio (SSMS) 工具在 Azure-SSIS IR 上运行这些包。 这些工具通过数据库服务器的服务器终结点来与该服务器建立连接： 

- 对于 Azure SQL 数据库服务器，服务器终结点格式为 `<server name>.database.chinacloudapi.cn`。

如果不使用 SSISDB，则可以将包部署到文件系统、文件共享或 Azure 文件存储中，并使用 `dtinstall`、`dtutil` 和 `dtexec` 命令行工具在 Azure-SSIS IR 上运行它们。 有关详细信息，请参阅[部署 SSIS 包](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server)。 

在这两种情况下，还可以使用数据工厂管道中的“执行 SSIS 包”活动在 Azure-SSIS IR 上运行已部署的包。 有关详细信息，请参阅[以第一类数据工厂活动的形式调用 SSIS 包执行](/data-factory/how-to-invoke-ssis-package-ssis-activity)。

## <a name="next-steps"></a>后续步骤

请参阅本文档中的其他 Azure-SSIS IR 主题：

- [Azure-SSIS 集成运行时](concepts-integration-runtime.md#azure-ssis-integration-runtime)。 此文提供有关集成运行时（包括 Azure-SSIS IR）的一般信息。
- [监视 Azure-SSIS IR](monitor-integration-runtime.md#azure-ssis-integration-runtime)。 此文介绍如何检索并了解有关 Azure-SSIS IR 的信息。
- [管理 Azure-SSIS IR](manage-azure-ssis-integration-runtime.md)。 此文介绍如何停止、启动或删除 Azure-SSIS IR。 此外，介绍如何通过添加更多节点来横向扩展 Azure-SSIS IR。
- [将 Azure-SSIS IR 加入虚拟网络](join-azure-ssis-integration-runtime-virtual-network.md)。 此文介绍如何将 Azure-SSIS IR 加入虚拟网络。
