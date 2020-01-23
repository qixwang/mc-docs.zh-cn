---
title: 在虚拟网络中保护试验和推理
titleSuffix: Azure Machine Learning
description: 了解如何在 Azure 虚拟网络中保护 Azure 机器学习中的试验/训练作业和推理/评分作业。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 11/13/2019
ms.openlocfilehash: 8eadb50257bd4fa60367235770f2f5791b1f3a3f
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75599640"
---
# <a name="secure-azure-ml-experimentation-and-inference-jobs-within-an-azure-virtual-network"></a>在 Azure 虚拟网络中保护 Azure ML 试验和推理作业
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

本文介绍如何在 Azure 虚拟网络 (VNet) 中保护 Azure 机器学习中的试验/训练作业和推理/评分作业。

**虚拟网络**充当安全边界，可将 Azure 资源与公共 Internet 相隔离。 你也可以将 Azure 虚拟网络加入本地网络。 加入网络后，可以安全训练模型，以及访问用于推理的已部署模型。

Azure 机器学习依赖于其他 Azure 服务提供计算资源。 计算资源或[计算目标](concept-compute-target.md)用于训练和部署模型。 可以在虚拟网络中创建目标。 例如，可以使用 Microsoft Data Science Virtual Machine 来训练模型，然后将该模型部署到 Azure Kubernetes 服务 (AKS)。 有关虚拟网络的详细信息，请参阅 [Azure 虚拟网络概述](/virtual-network/virtual-networks-overview)。

本文还将提供有关高级安全设置的详细信息（在基本或试验用例中不需要这些信息）。  本文的某些部分提供各种方案的配置信息。 无需按顺序遵循本文中的说明，也不需要遵循所有的说明。

> [!TIP]
> 除非有专门的说明，否则可以在机器学习管道和非管道工作流（例如脚本运行）中使用虚拟网络中的存储帐户或计算目标等资源。

> [!WARNING]
> Microsoft 不支持对虚拟网络中的资源使用 Azure 机器学习设计器或自动化机器学习（通过工作室）。

## <a name="prerequisites"></a>先决条件

+ 一个 Azure 机器学习[工作区](how-to-manage-workspace.md)。

+ 对 [Azure 虚拟网络服务](/virtual-network/virtual-networks-overview)和 [IP 网络](/virtual-network/virtual-network-ip-addresses-overview-arm)具备一般性的实践知识。

+ 预先存在一个要与计算资源配合使用的虚拟网络和子网。

## <a name="use-a-storage-account-for-your-workspace"></a>对工作区使用存储帐户

若要对虚拟网络中的工作区使用 Azure 存储帐户，请执行以下操作：

1. 在虚拟网络的后面创建一个计算资源（例如机器学习计算实例或群集），或者将某个计算资源（例如 HDInsight 群集、虚拟机或 Azure Kubernetes 服务群集）附加到工作区。 该计算资源可用于试验或模型部署。

   有关详细信息，请参阅本文的[使用机器学习计算](#amlcompute)、[使用虚拟机或 HDInsight 群集](#vmorhdi)和[使用 Azure Kubernetes 服务](#aksvnet)部分。

1. 在 Azure 门户中，转到已附加到工作区的存储。

   [![已附加到 Azure 机器学习工作区的存储](./media/how-to-enable-virtual-network/workspace-storage.png)](./media/how-to-enable-virtual-network/workspace-storage.png#lightbox)

1. 在“Azure 存储”页上，选择“防火墙和虚拟网络”。  

   ![Azure 门户中“Azure 存储”页上的“防火墙和虚拟网络”区域](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks.png)

1. 在“防火墙和虚拟网络”页上执行以下操作： 
    - 选择“所选网络”。 
    - 在“虚拟网络”下，选择“添加现有的虚拟网络”链接。   此操作将添加计算资源所在的虚拟网络（参阅步骤 1）。

        > [!IMPORTANT]
        > 存储帐户必须与用于训练或推理的计算实例或群集位于同一虚拟网络中。

    - 选中“允许受信任的 Microsoft 服务访问此存储帐户”复选框。 

    > [!IMPORTANT]
    > 使用 Azure 机器学习 SDK 时，开发环境必须能够连接到 Azure 存储帐户。 当存储帐户位于虚拟网络中时，防火墙必须允许从开发环境的 IP 地址进行访问。
    >
    > 若要启用对存储帐户的访问，请从开发客户端上的 Web 浏览器访问存储帐户的“防火墙和虚拟网络”。   然后选中“添加客户端 IP 地址”复选框，将客户端的 IP 地址添加到“地址范围”。   也可以使用“地址范围”字段手动输入开发环境的 IP 地址。  添加客户端的 IP 地址后，该客户端可以使用 SDK 访问存储帐户。

   [![Azure 门户中的“防火墙和虚拟网络”窗格](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png#lightbox)

> [!IMPORTANT]
> 可将 Azure 机器学习的默认存储帐户或者将非默认存储帐户放在虚拟网络中。  
>
> 创建工作区时，会自动预配默认存储帐户。
>
> 对于非默认存储帐户，可以使用 [`Workspace.create()` 函数](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-)中的 `storage_account` 参数按 Azure 资源 ID 指定自定义的存储帐户。

## <a name="use-a-key-vault-instance-with-your-workspace"></a>将 Key Vault 实例与工作区配合使用

Azure 机器学习使用与工作区关联的 Key Vault 实例来存储以下凭据：
* 关联的存储帐户连接字符串
* Azure 容器存储库实例的密码
* 数据存储的连接字符串

若要在虚拟网络的后面将 Azure 机器学习试验功能与 Azure Key Vault 配合使用，请执行以下操作：
1. 转到与工作区关联的 Key Vault。

   [![与 Azure 机器学习工作区关联的 Key Vault](./media/how-to-enable-virtual-network/workspace-key-vault.png)](./media/how-to-enable-virtual-network/workspace-key-vault.png#lightbox)

1. 在“Key Vault”页上的左侧窗格中，选择“防火墙和虚拟网络”。  

   ![“Key Vault”窗格中的“防火墙和虚拟网络”部分](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks.png)

1. 在“防火墙和虚拟网络”页上执行以下操作： 
    - 在“允许的访问来源”  下，选择“所选网络”  。
    - 在“虚拟网络”下，选择“添加现有的虚拟网络”，以添加试验计算资源所在的虚拟网络。  
    - 在“允许受信任的 Microsoft 服务跳过此防火墙”下选择“是”。  

   [![“Key Vault”窗格中的“防火墙和虚拟网络”部分](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png#lightbox)

<a id="amlcompute"></a>

## <a name="compute-instance"></a>使用机器学习计算

> [!NOTE]
> 计算实例（预览版）目前仅适用于区域为“美国中北部”或“英国南部”的工作区，即将支持其他区域。  
> 可以使用其中的一个区域来创建可添加到虚拟网络的计算实例。

若要在虚拟网络中使用 Azure 机器学习计算实例或计算群集，必须满足以下网络要求：

> [!div class="checklist"]
> * 该虚拟网络必须与 Azure 机器学习工作区位于同一订阅和区域。
> * 为计算实例或群集指定的子网必须具有足够的未分配 IP 地址，以容纳目标 VM 数目。 如果该子网没有足够的未分配 IP 地址，则只会为计算群集分配一部分资源。
> * 检查对虚拟网络的订阅或资源组实施的安全策略或锁定是否限制了管理虚拟网络所需的权限。 如果你打算通过限制流量来保护虚拟网络，请为计算服务保持打开某些端口。 有关详细信息，请参阅[所需的端口](#mlcports)部分。
> * 若要将多个计算实例或群集放入一个虚拟网络，可能需要请求提高一个或多个资源的配额。
> * 如果工作区的 Azure 存储帐户也在虚拟网络中受保护，则它们必须与 Azure 机器学习计算实例或群集位于同一虚拟网络中。 若要在同一个虚拟网络中创建计算实例，需要从虚拟网络分离存储帐户，在虚拟网络中创建计算实例，然后将存储帐户重新附加到虚拟网络。

机器学习计算实例或群集自动在包含虚拟网络的资源组中分配更多网络资源。 对于每个计算实例或群集，该服务将分配以下资源：

* 一个网络安全组
* 一个公共 IP 地址
* 一个负载均衡器

这些资源受订阅的[资源配额](/azure-subscription-service-limits)限制。


### <a id="mlcports"></a> 所需端口

机器学习计算目前使用 Azure Batch 服务在指定的虚拟网络中预配 VM。 子网必须允许来自 Batch 服务的入站通信。 可以使用这种通信在机器学习计算节点上计划运行，以及与 Azure 存储和其他资源进行通信。 Batch 服务在附加到 VM 的网络接口 (NIC) 级别添加网络安全组 (NSG)。 这些 NSG 自动配置允许以下流量的入站和出站规则：

- 端口 29876 和 29877 上的来自 __BatchNodeManagement__ 服务标记的入站 TCP 流量。 

    ![使用 BatchNodeManagement 服务标记的入站规则](./media/how-to-enable-virtual-network/batchnodemanagement-service-tag.png)

- （可选）端口 22 上允许远程访问的入站 TCP 流量。 仅当要在公共 IP 上使用 SSH 进行连接时，才使用此端口。

- 任何端口上通往虚拟网络的出站流量。

- 任何端口上通往 Internet 的出站流量。

- 对于端口 44224 上的来自 __AzureMachineLearning__ 服务标记的计算实例入站 TCP 流量。 

在 Batch 配置的 NSG 中修改或添加入站或出站规则时，请务必小心。 如果 NSG 阻止与计算节点通信，则计算服务会将计算节点的状态设置为不可用。

不需要在子网级别指定 NSG，因为 Azure Batch 会配置自身的 NSG。 但是，如果指定的子网具有关联的 NSG 或防火墙，请如前所述配置入站和出站安全规则。

下图显示了 Azure 门户中的 NSG 规则配置：

[![机器学习计算的入站 NSG 规则](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png#lightbox)

![机器学习计算的出站 NSG 规则](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a id="limiting-outbound-from-vnet"></a> 限制来自虚拟网络的出站连接

如果你不想要使用默认的出站规则，同时想要限制虚拟网络的出站访问，请执行以下操作：

- 使用 NSG 规则拒绝出站 Internet 连接。

- 限制发往以下服务的出站流量：
   - Azure 存储 - 使用 __Storage.Region_Name__ 的服务标记  
   - Azure 容器注册表 - 使用 __AzureContainerRegistry.Region_Name__ 的服务标记  
   - Azure 机器学习 - 使用 __AzureMachineLearning__ 的服务标记 
   - 对于计算实例和 Azure 云，请使用 __AzureCloud.Region_Name__ 的服务标记 

下图显示了 Azure 门户中的 NSG 规则配置：

[![机器学习计算的出站 NSG 规则](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png)](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png#lightbox)

### <a name="user-defined-routes-for-forced-tunneling"></a>用户定义的用于强制隧道的路由

若要在机器学习计算中使用强制隧道，请将[用户定义的路由 (UDR)](/virtual-network/virtual-networks-udr-overview) 添加到包含计算资源的子网。

* 为资源所在区域中的 Azure Batch 服务使用的每个 IP 地址建立一个 UDR。 这些 UDR 使 Batch 服务能够与计算节点通信，以计划任务。 若要获取 Batch 服务的 IP 地址列表，请使用以下方法之一：

    * 下载 [Azure IP 范围和服务标记](https://www.microsoft.com/download/details.aspx?id=56519)，并在文件中搜索 `BatchNodeManagement.<region>`（其中，`<region>` 是你的 Azure 区域）。

    * 使用 [Azure CLI](/cli/install-azure-cli?view=azure-cli-latest) 下载信息。 以下示例下载 IP 地址信息，并筛选出“美国东部 2”区域的信息：

        ```azurecli
        az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'Batch')] | [?properties.region=='chinaeast']"
        ```

* 发往 Azure 存储的出站流量不能被本地网络设备阻止。 具体而言，URL 采用 `<account>.table.core.windows.net`、`<account>.queue.core.windows.net` 和 `<account>.blob.core.windows.net` 格式。

添加 UDR 时，请为每个相关的 Batch IP 地址前缀定义路由，并将“下一跃点类型”设置为“Internet”。   下图显示了 Azure 门户中此 UDR 的示例：

![地址前缀的 UDR 示例](./media/how-to-enable-virtual-network/user-defined-route.png)

有关详细信息，请参阅[在虚拟网络中创建 Azure Batch 池](../batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling)。

### <a name="create-a-compute-cluster-in-a-virtual-network"></a>在虚拟网络中创建计算群集

若要创建机器学习计算群集，请执行以下操作：

1. 在 [Azure 门户](https://portal.azure.cn)中，选择你的 Azure 机器学习工作区。

1. 在“应用程序”部分，依次选择“计算”、“添加计算”。   

1. 若要将此计算资源配置为使用虚拟网络，请执行以下操作：

    a. 对于“网络配置”，请选择“高级”。  

    b. 在“资源组”下拉列表中，选择包含虚拟网络的资源组。 

    c. 在“虚拟网络”下拉列表中，选择包含子网的虚拟网络。 

    d. 在“子网”下拉列表中，选择要使用的子网。 

   ![机器学习计算的虚拟网络设置](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

也可以使用 Azure 机器学习 SDK 创建机器学习计算群集。 以下代码在名为 `mynetwork` 的虚拟网络的 `default` 子网中创建新的机器学习计算群集：

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure virtual network name, subnet, and resource group
vnet_name = 'mynetwork'
subnet_name = 'default'
vnet_resourcegroup_name = 'mygroup'

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print("Found existing cpucluster")
except ComputeTargetException:
    print("Creating new cpucluster")

    # Specify the configuration for the new cluster
    compute_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                           min_nodes=0,
                                                           max_nodes=4,
                                                           vnet_resourcegroup_name=vnet_resourcegroup_name,
                                                           vnet_name=vnet_name,
                                                           subnet_name=subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

    # Wait for the cluster to be completed, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

创建过程完成后，请在试验中使用该群集训练模型。 有关详细信息，请参阅[选择并使用用于训练的计算目标](how-to-set-up-training-targets.md)。

## <a name="use-azure-databricks"></a>使用 Azure Databricks

若要将虚拟网络中的 Azure Databricks 与工作区配合使用，必须满足以下要求：

> [!div class="checklist"]
> * 该虚拟网络必须与 Azure 机器学习工作区位于同一订阅和区域。
> * 如果工作区的 Azure 存储帐户也在虚拟网络中受保护，则它们必须与 Azure Databricks 群集位于同一虚拟网络中。
> * 除了 Azure Databricks 使用的 __databricks-private__ 和 __databricks-public__ 子网以外，还需要为虚拟网络创建 __default__ 子网。

有关在虚拟网络中使用 Azure Databricks 的具体信息，请参阅[在 Azure 虚拟网络中部署 Azure Databricks](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html)。

<a id="vmorhdi"></a>

## <a name="use-a-virtual-machine-or-hdinsight-cluster"></a>使用虚拟机或 HDInsight 群集

> [!IMPORTANT]
> Azure 机器学习仅支持运行 Ubuntu 的虚拟机。

若要将虚拟网络中的虚拟机或 Azure HDInsight 群集与工作区配合使用，请执行以下操作：

1. 使用 Azure 门户或 Azure CLI 创建 VM 或 HDInsight 群集，并将群集放入 Azure 虚拟网络。 有关详细信息，请参阅以下文章：
    * [为 Linux VM 创建和管理 Azure 虚拟网络](/virtual-machines/linux/tutorial-virtual-network)

    * [使用 Azure 虚拟网络扩展 HDInsight](/hdinsight/hdinsight-extend-hadoop-virtual-network)

1. 若要允许 Azure 机器学习与 VM 或群集上的 SSH 端口通信，请为网络安全组配置一个源条目。 SSH 端口通常是端口 22。 若要允许来自此源的流量，请执行以下操作：

    * 在“源”下拉列表中，选择“服务标记”。  

    * 在“源服务标记”下拉列表中，选择“AzureMachineLearning”。  

    * 在“源端口范围”下拉列表中，选择 __*__ 。 

    * 在“目标”下拉列表中，选择“任何”。  

    * 在“目标端口范围”下拉列表中，选择“22”。  

    * 在“协议”下，选择“任何”。  

    * 在“操作”下，选择“允许”。  

   ![用于在虚拟网络中的 VM 或 HDInsight 群集上执行试验的入站规则](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    保留网络安全组的默认出站规则。 有关详细信息，请参阅[安全组](/virtual-network/security-overview#default-security-rules)中的“默认安全规则”。

    如果你不想要使用默认出站规则，同时想要限制虚拟网络的出站访问，请参阅[限制来自虚拟网络的出站连接](#limiting-outbound-from-vnet)部分。

1. 将 VM 或 HDInsight 群集附加到 Azure 机器学习工作区。 有关详细信息，请参阅[设置模型训练的计算目标](how-to-set-up-training-targets.md)。

<a id="aksvnet"></a>

## <a name="use-azure-kubernetes-service-aks"></a>使用 Azure Kubernetes 服务 (AKS)

若要将虚拟网络中的 AKS 添加到工作区，请执行以下操作：

> [!IMPORTANT]
> 在开始执行以下过程之前，请满足[在 Azure Kubernetes 服务 (AKS) 中配置高级网络](/aks/configure-advanced-networking#prerequisites)操作指南中的先决条件，并规划好群集的 IP 地址。
>
> AKS 实例和 Azure 虚拟网络必须位于同一区域。 如果在虚拟网络中保护工作区使用的 Azure 存储帐户，这些帐户必须与 AKS 实例位于同一虚拟网络中。

1. 在 [Azure 门户](https://portal.azure.cn)中，请使用“AzureMachineLearning”作为“源”，确保用于控制虚拟网络的 NSG 包含一条已为 Azure 机器学习启用的入站规则。  

    [![Azure 机器学习“添加计算”窗格](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-aml.png)](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-aml.png#lightbox)

1. 选择你的 Azure 机器学习工作区。

1. 在“应用程序”部分，依次选择“计算”、“添加计算”。   

1. 若要将此计算资源配置为使用虚拟网络，请执行以下操作：

    - 对于“网络配置”，请选择“高级”。  

    - 在“资源组”下拉列表中，选择包含虚拟网络的资源组。 

    - 在“虚拟网络”下拉列表中，选择包含子网的虚拟网络。 

    - 在“子网”下拉列表中选择子网。 

    - 在“Kubernetes 服务地址范围”中，输入 Kubernetes 服务地址范围。  此地址范围使用无类域间路由 (CIDR) 表示法表示的 IP 范围来定义群集可用的 IP 地址。 此范围不得与任何子网 IP 范围重叠（例如 10.0.0.0/16）。

    - 在“Kubernetes DNS 服务 IP 地址”框中，输入 Kubernetes DNS 服务 IP 地址。  此 IP 地址将分配给 Kubernetes DNS 服务。 此 IP 地址必须在 Kubernetes 服务地址范围内（例如 10.0.0.10）。

    - 在“Docker 网桥地址”框中，输入 Docker 网桥地址。  此 IP 地址将分配给 Docker 网桥。 此 IP 地址不得在任何子网 IP 范围或 Kubernetes 服务地址范围内（例如 172.17.0.1/16）。

   ![Azure 机器学习：机器学习计算虚拟网络设置](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. 确保用于控制虚拟网络的 NSG 组包含一条已为评分终结点启用的入站安全规则，以便可以从虚拟网络外部调用此终结点。
   > [!IMPORTANT]
   > 保留 NSG 的默认出站规则。 有关详细信息，请参阅[安全组](/virtual-network/security-overview#default-security-rules)中的“默认安全规则”。

   [![入站安全规则](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png#lightbox)

也可以使用 Azure 机器学习 SDK 在虚拟网络中添加 Azure Kubernetes 服务。 如果虚拟网络中已有一个 AKS 群集，请根据[如何部署到 AKS](how-to-deploy-and-where.md) 中所述，将此群集附加到工作区。 以下代码在名为 `mynetwork` 的虚拟网络的 `default` 子网中创建新的 AKS 实例：

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="chinaeast")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace=ws,
                                  name="myaks",
                                  provisioning_configuration=config)
```

创建过程完成后，可在虚拟网络后面的 AKS 群集上运行推理或模型评分。 有关详细信息，请参阅[如何部署 AKS](how-to-deploy-and-where.md)。

## <a name="use-azure-firewall"></a>使用 Azure 防火墙

使用 Azure 防火墙时，必须将网络规则配置为允许发往或来自以下地址的流量：

- `*.batchai.core.windows.net`
- `ml.azure.com`
- `*.azureml.ms`
- `*.experiments.azureml.net`
- `*.modelmanagement.azureml.net`
- `mlworkspace.azure.ai`
- `*.aether.ms`

添加规则时，请将“协议”设置为“任何”，并将端口设置为 `*`。 

有关配置网络规则的详细信息，请参阅[部署和配置 Azure 防火墙](/firewall/tutorial-firewall-deploy-portal#configure-a-network-rule)。

## <a name="next-steps"></a>后续步骤

* [设置训练环境](how-to-set-up-training-targets.md)
* [模型部署位置](how-to-deploy-and-where.md)
* [使用 SSL 安全地部署模型](how-to-secure-web-service.md)

