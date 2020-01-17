---
title: 创建群集错误字典
description: 了解如何创建群集错误字典。
ms.reviewer: hrasheed
author: karkrish
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: troubleshooting
origin.date: 11/19/2019
ms.date: 01/13/2020
ms.author: v-yiso
ms.openlocfilehash: bf95f450bff11223fa4dcb33147506e5391c34f4
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75631299"
---
# <a name="hdinsight-cluster-creation-errors"></a>HDInsight：群集创建错误

本文介绍在创建群集时遇到的错误的解决方法。 

> [!NOTE]
> 以下列表中的前三个错误是当 HDInsight 产品使用 CsmDocument_2_0 类时发生的验证错误造成的。



## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>错误代码： DeploymentDocument 'CsmDocument_2_0' 验证失败

### <a name="error-script-action-location-cannot-be-accessed-uriscript-action-url"></a>**错误**：无法通过 URI:\<脚本操作 URL\> 访问脚本操作位置

**错误消息：“远程服务器返回错误:(404)未找到。”**

### <a name="cause"></a>原因
无法从 HDInsight 服务访问作为“创建群集”请求的一部分提供的脚本操作 URL。 尝试访问脚本操作时收到了“ErrorMessage”。

### <a name="resolution"></a>解决方法 
  - 对于 http/https URL，可以尝试从 Incognito 浏览器窗口打开该 URL 进行验证。 
  - 对于 WASB URL，请确保该脚本在请求中指定的存储帐户中存在。 确保此存储帐户的存储密钥准确。 
  - 对于 ADLS URL，请确保该脚本在存储帐户中存在。

---

## <a name="error-codedeploymentdocument-csmdocument_2_0-failed-the-validation"></a>错误代码： DeploymentDocument 'CsmDocument_2_0' 验证失败

### <a name="error-script-action-location-cannot-be-accessed-uri-script-action-url"></a>**错误**：无法通过 URI:\<脚本操作 URL\> 访问脚本操作位置

**错误消息：给定的脚本 URI \<脚本 URI\> 在 ADLS 中，但此群集没有 Data Lake Storage 主体**

### <a name="cause"></a>原因
无法从 HDInsight 服务访问作为“创建群集”请求的一部分提供的脚本操作 URL。 尝试访问脚本操作时收到了“ErrorMessage”。 

### <a name="resolution"></a>解决方法

确保将相应的 Azure Data Lake Store Gen 1 帐户添加到群集。 此外，将用于访问 Azure Data Lake Store Gen 1 帐户的服务主体添加到群集。 

---

## <a name="error-code-deploymentdocument-csmdocument_2_0-failed-the-validation"></a>错误代码：DeploymentDocument 'CsmDocument_2_0' 验证失败

### <a name="error-vm-size-customer-specified-vm-size-provided-in-the-request-is-invalid-or-not-supported-for-role-role-valid-values-are-valid-vm-size-for-role"></a>**错误**：请求中提供的 VM 大小“\<客户指定的 VM 大小\>”无效或不受角色“\<角色\>”的支持。 有效值是：\<角色的有效 VM 大小\>。

### <a name="cause"></a>原因
该角色不允许客户指定的 VM 大小。 之所以出现这种情况，是因为 VM 大小值不可按预期方式使用，或者不适合该计算机角色。 

### <a name="resolution"></a>解决方法
错误消息列出了 VM 大小的有效值。 请选择其中的一个有效值，然后重试“创建群集”请求。 

---

## <a name="error-codeinvalidvirtualnetworkid"></a>错误代码： InvalidVirtualNetworkId  

### <a name="error-the-virtualnetworkid-is-not-valid-virtualnetworkid-user_virtualnetworkid"></a>**错误**：VirtualNetworkId 无效。 VirtualNetworkId '\<USER_VIRTUALNETWORKID\>'*

### <a name="cause"></a>原因
在群集创建过程中指定的 **VirtualNetworkId** 值的格式不正确。 

### <a name="resolution"></a>解决方法
确保 **VirtualNetworkId** 和子网采用正确的格式。 若要获取 **VirtualnetworkId** 值，请转到 Azure 门户，选择你的虚拟网络，然后在菜单中选择“属性”。  **ResourceID** 属性为 **VirtualNetworkId** 值。 

虚拟网络 ID 的示例： 

“/subscriptions/c15fd9b8-e2b8-1d4e-aa85-2e668040233b/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/myvnet” 

---

## <a name="error-code-customizationfailederrorcode"></a>错误代码：CustomizationFailedErrorCode

### <a name="error-cluster-deployment-failed-due-to-an-error-in-the-custom-script-action-failed-actions-script_name-please-go-to-ambari-ui-to-further-debug-the-failure"></a>**错误**：群集部署因自定义脚本操作出错而失败。 失败的操作:\<SCRIPT_NAME\>，请转到 Ambari UI 以进一步调试错误。

### <a name="cause"></a>原因
成功部署群集后，将执行“创建群集”请求期间提供的用户自定义脚本。 此错误代码指示在执行此名为 \<SCRIPT_NAME\> 的自定义脚本时遇到错误。   

### <a name="resolution"></a>解决方法
由于这是用户的自定义脚本，因此应该由用户排查问题，并根据需要重新运行脚本。 若要排查脚本错误，请检查 /var/lib/ambari-agent/* 文件夹中的日志。 或者，在 Ambari UI 中打开“操作”页，然后选择“run_customscriptionaction”操作查看错误详细信息。  

---

## <a name="error-codeinvaliddocumenterrorcode"></a>错误代码： InvalidDocumentErrorCode

### <a name="error-the-meta_store_type-metastore-schema-version-metastore_major_version-in-database-database_name-is-incompatible-with-cluster-version-cluster_version"></a>**错误**：数据库 \<DATABASE_NAME\> 中的 \<META_STORE_TYPE\> 元存储架构版本 \<METASTORE_MAJOR_VERSION\> 与群集版本 \<CLUSTER_VERSION\> 不兼容

### <a name="cause"></a>原因
自定义元存储与所选的 HDInsight 群集版本不兼容。 目前，HDInsight 4.0 群集仅支持元存储版本 3.*x*，HDInsight 3.6 不支持元存储版本 3.*x* 或更高版本。 

### <a name="resolution"></a>解决方法
确保仅使用每个 HDInsight 群集版本支持的元存储版本。 请注意，如果未指定自定义元存储，HDInsight 将在内部创建元存储。 但是，在删除群集时，会自动删除此元存储。 

---

## <a name="error-code-failedtoconnectwithclustererrorcode"></a>错误代码：FailedToConnectWithClusterErrorCode 

### <a name="error-unable-to-connect-to-cluster-management-endpoint-to-perform-scaling-operation-verify-that-network-security-rules-are-not-blocking-external-access-to-the-cluster-and-that-the-cluster-manager-ambari-ui-can-be-successfully-accessed"></a>**错误**：无法连接到群集管理终结点来执行缩放操作。 验证网络安全规则是否未阻止从外部访问群集，以及是否可以成功访问群集管理器 (Ambari) UI。

### <a name="cause"></a>原因
网络安全组 (NSG) 中的某个防火墙规则阻止了群集与关键 Azure 运行状况和管理服务的通信。 

### <a name="resolution"></a>解决方法
如果你打算使用 **网络安全组**来控制网络流量，请在安装 HDInsight 之前执行以下操作： 
  - 确定计划用于 HDInsight 的 Azure 区域。 
  - 确定 HDInsight 所需的 IP 地址。 有关详细信息，请参阅 [HDInsight 管理 IP 地址](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)。 
    - 为计划将 HDInsight 安装到其中的子网创建或修改网络安全组。 
    - **网络安全组：** 允许端口  **443** 上的来自该 IP 地址的 **入站**流量。 这将确保 HDInsight 管理服务可以从虚拟网络外部访问群集。 

---

## <a name="error-code-storagepermissionsblockedformsi"></a>错误代码：StoragePermissionsBlockedForMsi  

### <a name="error-the-managed-identity-does-not-have-permissions-on-the-storage-account-please-verify-that-storage-blob-data-owner-role-is-assigned-to-the-managed-identity-for-the-storage-account-storage-subscriptions-subscription-id-resourcegroups-resource-group-name-providersmicrosoftstoragestorageaccounts-storage-account-name-managed-identity-subscriptions-subscription-id-resourcegroups--resource-group-name-providersmicrosoftmanagedidentityuserassignedidentities-user-managed-identity-name"></a>**错误**：托管标识对存储帐户没有权限。 请验证是否已将“存储 Blob 数据所有者”角色分配给存储帐户的托管标识。 存储：/subscriptions/\<订阅 ID\>/resourceGroups/\<资源组名称\>/providers/Microsoft.Storage/storageAccounts/\<存储帐户名称\>；托管标识：/subscriptions/\<订阅 ID\>/resourceGroups//\<资源组名称\>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/\<用户托管标识名称\>

### <a name="cause"></a>原因
未将所需的权限提供给**托管标识**。 **用户分配的托管标识**没有 ADLS Gen2 存储帐户的 Blob 存储参与者角色。 

### <a name="resolution"></a>解决方法

打开 Azure 门户，转到你的存储帐户，查看“访问控制(IAM)”，确保“存储 Blob 数据参与者”或“存储 Blob 数据所有者”角色已经为该订阅的“用户分配的托管标识”分配了访问权限。   **** 有关详细信息，请参阅[在 Data Lake Storage Gen2 帐户中设置托管标识的权限](hdinsight-hadoop-use-data-lake-storage-gen2.md)。 

---

## <a name="error-code-invalidnetworksecuritygroupsecurityrules"></a>错误代码：InvalidNetworkSecurityGroupSecurityRules  

### <a name="error-the-security-rules-in-the-network-security-group-subscriptionssubscriptionidresourcegroupsresource-group-name-defaultprovidersmicrosoftnetworknetworksecuritygroupsnetwork-security-group-name-configured-with-subnet-subscriptionssubscriptionidresourcegroupsresource-group-name-rg-westeurope-vnet-tomtom-defaultprovidersmicrosoftnetworkvirtualnetworksvirtual-network-namesubnetssubnet-name"></a>**错误**：配置了子网 /subscriptions/\<订阅 ID>\/resourceGroups/\<资源组名称> RG-westeurope-vnet-tomtom-default\/providers/Microsoft.Network/virtualNetworks/\<虚拟网络名称>\/subnets/\<子网名称\> 的网络安全组 /subscriptions/\<订阅 ID>\/resourceGroups/<Resource Group name>default/providers/Microsoft.Network/networkSecurityGroups/\<网络安全组名称\> 中的安全规则 
不允许建立所需的入站和/或出站连接。 有关详细信息，请访问[规划 Azure HDInsight 的虚拟网络](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment)，或联系支持人员。**

### <a name="cause"></a>原因
如果使用网络安全组 (NSG) 或用户定义的路由 (UDR) 来控制流向 HDInsight 群集的入站流量，则必须确保群集能够与关键的 Azure 运行状况和管理服务通信。

### <a name="resolution"></a>解决方法
如果你打算使用 **网络安全组**来控制网络流量，请在安装 HDInsight 之前执行以下操作： 
  - 确定要用于 HDInsight 的 Azure 区域，并为该区域创建安全的 IP 地址列表：[运行状况和管理服务：特定区域](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses#health-and-management-services-specific-regions)。
  - 确定 HDInsight 所需的 IP 地址。 有关详细信息，请参阅  [HDInsight 管理 IP 地址](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)。 
  - 为计划将 HDInsight 安装到其中的子网创建或修改网络安全组。 **网络安全组**：允许端口  **443** 上的来自 IP 地址的入站流量。 这将确保 HDInsight 管理服务可以从虚拟网络外部访问群集。
  
---

## <a name="error-code-cluster-setup-failed-to-install-components-on-one-or-more-hosts"></a>错误代码：群集安装程序无法在一个或多个主机上安装组件

###  <a name="errorcluster-setup-failed-to-install-components-on-one-or-more-hosts-please-retry-your-request"></a>**错误**： 群集安装程序无法在一个或多个主机上安装组件。 请重试请求。 

### <a name="cause"></a>原因 
通常，此错误是出现暂时性问题或 Azure 服务中断时生成的 

### <a name="resolution"></a>解决方法

有关可能影响群集部署的任何潜在 Azure 服务中断，请查看 [Azure 状态](https://status.azure.com/status)页。 如果未发生服务中断，请重试部署群集。 

## <a name="next-steps"></a>后续步骤

若要更深入地了解如何排查群集创建问题，请参阅[排查 Azure HDInsight 中的群集创建失败问题](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-troubleshoot-cluster-creation-fails)。
