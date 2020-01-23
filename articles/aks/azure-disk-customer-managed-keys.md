---
title: 使用客户管理的密钥加密 Azure Kubernetes Service (AKS) 中的 Azure 磁盘
description: 自带密钥 (BYOK) 来加密 AKS OS 和数据磁盘。
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: article
origin.date: 01/12/2020
ms.date: 01/20/2020
ms.author: v-yeche
ms.openlocfilehash: ef54fb911b43125b4113c730db261954b141d31f
ms.sourcegitcommit: 8de025ca11b62e06ba3762b5d15cc577e0c0f15d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2020
ms.locfileid: "76170664"
---
# <a name="bring-your-own-keys-byok-with-azure-disks-in-azure-kubernetes-service-aks"></a>对 Azure Kubernetes Service (AKS) 中的 Azure 磁盘使用自带密钥 (BYOK)

Azure 存储对静态存储帐户中的所有数据进行加密。 默认情况下，数据使用 Azure 管理的密钥进行加密。 为了更进一步控制加密密钥，你可以提供[客户管理的密钥][customer-managed-keys]，以用于对 AKS 群集的 OS 和数据磁盘进行加密。

<!--Not Available on > Linux and Windows based AKS clusters are both supported.-->

## <a name="before-you-begin"></a>准备阶段

* 本文假定你要创建*新的 AKS 群集*。  你还需要使用或创建 Azure 密钥保管库的实例来存储加密密钥。

* 使用密钥保管库加密托管磁盘时，必须为 *Azure 密钥保管库*启用软删除和清除保护。

* 需要 Azure CLI 2.0.79 或更高版本，以及 aks-preview 0.4.26 扩展

> [!IMPORTANT]
> AKS 预览功能是自助式选择加入功能。 预览版“按原样”提供，并且仅在“可用情况下”提供，不包含在服务级别协议和有限保障中。 AKS 预览版的内容部分包含在客户支持中，我们只能尽力提供支持。 因此，这些功能不应用于生产。 有关其他信息，请参阅以下支持文章：
>
> * [AKS 支持策略](support-policies.md)
> * [Azure 支持常见问题](faq.md)

## <a name="install-latest-aks-cli-preview-extension"></a>安装最新的 AKS CLI 预览版扩展

若要使用客户管理的密钥，需要具有 *aks-preview* CLI 扩展版本 0.4.26 或更高版本。 使用 [az extension add][az-extension-add] 命令安装 *aks-preview* Azure CLI 扩展，然后使用 [az extension update][az-extension-update] 命令检查是否有任何可用的更新：

```azurecli
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="create-an-azure-key-vault-instance-to-store-your-keys"></a>创建 Azure 密钥保管库实例来存储密钥

可以通过 Azure 门户[使用 Azure Key Vault 配置客户管理的密钥][byok-azure-portal]

创建一个新的*资源组*，然后创建一个新的*密钥保管库*实例，并启用软删除和清除保护。

```azurecli
# Optionally retrieve Azure region short names for use on upcoming commands
az account list-locations

# Create new resource group in a supported Azure region
az group create -l myAzureRegionName -n myResourceGroup

# Create an Azure Key Vault resource in a supported Azure region
az keyvault create -n myKeyVaultName -g myResourceGroup -l myAzureRegionName  --enable-purge-protection true --enable-soft-delete true
```

## <a name="create-an-instance-of-a-diskencryptionset"></a>创建 DiskEncryptionSet 的实例

要完成以下步骤，需要具有存储在 Azure 密钥保管库中的“密钥”  。  将现有密钥存储在你创建的密钥保管库中，或者[生成一个密钥][key-vault-generate]

```azurecli
# Retrieve the Key Vault Id and store it in a variable
keyVaultId=$(az keyvault show --name myKeyVaultName --query [id] -o tsv)

# Retrieve the Key Vault key URL and store it in a variable
keyVaultKeyUrl=$(az keyvault key show --vault-name myKeyVaultName  --name myKeyName  --query [key.kid] -o tsv)

# Create a DiskEncryptionSet
az disk-encryption-set create -n myDiskEncryptionSetName  -l myAzureRegionName  -g myResourceGroup --source-vault $keyVaultId --key-url $keyVaultKeyUrl 
```

## <a name="grant-the-diskencryptionset-resource-access-to-the-key-vault"></a>向 DiskEncryptionSet 资源授予对密钥保管库的访问权限

使用在前面的步骤中创建的 DiskEncryptionSet 和资源组，并授予 DiskEncryptionSet 资源对 Azure 密钥保管库的访问权限。

```azurecli
# Retrieve the DiskEncryptionSet value and set a variable
desIdentity=$(az disk-encryption-set show -n myDiskEncryptionSetName  -g myResourceGroup --query [identity.principalId] -o tsv)

# Update security policy settings
az keyvault set-policy -n myKeyVaultName -g myResourceGroup --object-id $desIdentity --key-permissions wrapkey unwrapkey get

# Assign the reader role
az role assignment create --assignee $desIdentity --role Reader --scope $keyVaultId
```

## <a name="create-a-new-aks-cluster-and-encrypt-the-os-disk-with-a-customer-manged-key"></a>创建一个新的 AKS 群集，并使用客户管理的密钥对 OS 磁盘进行加密

创建一个新的资源组和 AKS 群集，然后使用密钥对 OS 磁盘进行加密。 只有高于 1.17 的 kubernetes 版本才支持客户管理的密钥

```azurecli
# Retrieve the DiskEncryptionSet value and set a variable
diskEncryptionSetId=$(az resource show -n diskEncryptionSetName -g myResourceGroup --resource-type "Microsoft.Compute/diskEncryptionSets" --query [id] -o tsv)

# Create a resource group for the AKS cluster
az group create -n myResourceGroup-l myAzureRegionName

# Create the AKS cluster
az aks create -n myAKSCluster -g myResourceGroup --node-osdisk-diskencryptionset-id $diskEncryptionSetId --kubernetes-version 1.17.0
```

向上面创建的群集添加新的节点池时，会使用在创建过程中提供的客户管理的密钥对 OS 磁盘进行加密

## <a name="encrypt-your-aks-cluster-data-disk-with-a-customer-managed-key"></a>使用客户管理的密钥对 AKS 群集数据磁盘进行加密

还可以使用你自己的密钥对 AKS 数据磁盘进行加密。  将 myResourceGroup 和 myDiskEncryptionSetName 替换为实际值并应用 yaml。

确保你具有正确的 AKS 凭据。 服务主体将需要对 diskencryptionset 所在的资源组具有参与者访问权限。 否则，你将收到一条错误消息，指出服务主体没有权限。

创建一个名为 **byok-azure-disk.yaml** 的文件，在其中包含以下信息。  将 myResourceGroup 和 myDiskEncrptionSetName 替换为你的值。

```
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: hdd
provisioner: kubernetes.io/azure-disk
parameters:
  skuname: Standard_LRS
  kind: managed
  diskEncryptionSetID: "/subscriptions/{subs-id}/resourceGroups/{myResourceGroup}/providers/Microsoft.Compute/diskEncryptionSets/{myDiskEncryptionSetName}"
```
接下来，在 AKS 群集中运行此部署：
```azurecli
kubectl apply -f byok-azure-disk.yaml
```

## <a name="limitations"></a>限制

* Kubernetes 版本 1.17 及更高版本支持 OS 磁盘加密   
* 仅适用于支持 BYOK 的区域
* 此功能当前仅适用于新的 AKS 群集，无法升级现有群集
* 使用虚拟机规模集的 AKS 群集是必需的，不支持虚拟机可用性集

## <a name="next-steps"></a>后续步骤

查看 [AKS 群集安全性最佳做法][best-practices-security]

<!-- LINKS - external -->

<!-- LINKS - internal -->

[az-extension-add]: https://docs.azure.cn/cli/extension?view=azure-cli-latest#az-extension-add
[az-extension-update]: https://docs.azure.cn/cli/extension?view=azure-cli-latest#az-extension-update
[best-practices-security]: /aks/operator-best-practices-cluster-security
[byok-azure-portal]: /storage/common/storage-encryption-keys-portal
[customer-managed-keys]: /virtual-machines/windows/disk-encryption#customer-managed-keys-public-preview
[key-vault-generate]: /key-vault/key-vault-manage-with-cli2

<!-- Update_Description: new article about azure disk customer managed keys -->
<!--NEW.date: 01/20/2020-->