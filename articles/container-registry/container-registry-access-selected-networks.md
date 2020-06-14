---
title: 配置服务防火墙规则
description: 配置 IP 规则以允许从选定的公共 IP 地址或地址范围访问 Azure 容器注册表。
ms.topic: article
origin.date: 05/04/2020
ms.date: 06/08/2020
ms.author: v-yeche
ms.openlocfilehash: 5fbb78ec05d14ca83721503792b2e01872b8c076
ms.sourcegitcommit: c4fc01b7451951ef7a9616fca494e1baf29db714
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84564362"
---
<!--Verified successfully on Portal and CLI-->
# <a name="configure-public-ip-network-rules"></a>配置公共 IP 网络规则

默认情况下，Azure 容器注册表会接受来自任何网络上的主机的 Internet 连接。 本文介绍如何将容器注册表配置为仅允许从特定的公共 IP 地址或地址范围进行访问。 提供了使用 Azure CLI 和 Azure 门户的等效步骤。

IP 网络规则在公共注册表终结点上进行配置。 
<!--Not Available on [Private Link](container-registry-private-link.md)-->

配置 IP 访问规则功能在“高级”容器注册表服务层级中可用。 有关注册表服务层级和限制的信息，请参阅 [Azure 容器注册表层级](container-registry-skus.md)。

## <a name="access-from-selected-public-network---cli"></a>从所选公共网络进行访问 - CLI

### <a name="change-default-network-access-to-registry"></a>更改对注册表的默认网络访问权限

若要将访问限制为所选公共网络，请首先将默认操作更改为“拒绝访问”。 请将以下 [az acr update][az-acr-update] 命令中的占位符替换为你的注册表名称：

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

### <a name="add-network-rule-to-registry"></a>向注册表添加网络规则

使用 [az acr network-rule add][az-acr-network-rule-add] 命令将允许从公共 IP 地址或范围进行访问的网络规则添加到注册表中。 例如，替换为虚拟网络中 VM 的容器注册表名和公共 IP 地址。

```azurecli
az acr network-rule add \
  --name mycontainerregistry \
  --ip-address <public-IP-address>
```

> [!NOTE]
> 添加规则后，规则需要几分钟才能生效。

## <a name="access-from-selected-public-network---portal"></a>从所选公共网络进行访问 - 门户

1. 在门户中，导航到容器注册表。
1. 在“设置”下选择“网络” 。
1. 在“公共访问”选项卡上，选择“所选网络”以允许从所选网络进行公共访问。
1. 在“防火墙”下输入公共 IP 地址，例如虚拟网络中 VM 的公共 IP 地址。 或者，以 CIDR 表示法输入包含 VM IP 地址的地址范围。
1. 选择“保存” 。

    ![为容器注册表配置防火墙规则][acr-access-selected-networks]

> [!NOTE]
> 添加规则后，规则需要几分钟才能生效。

> [!TIP]
> 或者，启用从本地客户端计算机或 IP 地址范围进行的注册表访问。 若要允许此访问，需要计算机的公共 IPv4 地址。 可以通过在 Internet 浏览器中搜索“我的 IP 地址是什么”来找到此地址。 当你在门户的“网络”页上配置防火墙设置时，当前客户端 IPv4 地址也会自动显示。

## <a name="disable-public-network-access"></a>禁用公共网络访问

<!--Not Available on  [Private Link](container-registry-private-link.md)-->

### <a name="disable-public-access---portal"></a>禁用公共访问 - 门户

1. 在门户中，导航到容器注册表，然后选择“设置”>“网络”。
1. 在“公共访问”选项卡上的“允许公共访问”中，选择“禁用”。 再选择“保存”。

    ![禁用公共访问][acr-access-disabled]

## <a name="restore-default-registry-access"></a>还原默认注册表访问

若要将注册表还原为默认的允许访问，请更新默认操作。 

### <a name="restore-default-registry-access---portal"></a>还原默认注册表访问 - 门户

1. 在门户中，导航到容器注册表，然后选择“设置”>“网络”。
1. 在“防火墙”下，选择每个地址范围，然后选择“删除”图标。
1. 在“公共访问”选项卡上的“允许公共访问”中，选择“所有网络”。 再选择“保存”。

![所有网络的公共访问][acr-access-all-networks]

## <a name="next-steps"></a>后续步骤

<!--Not Available on [Configure Azure Private Link for an Azure container registry](container-registry-private-link.md)-->

* 如果需要从客户端防火墙后面设置注册表访问规则，请参阅[配置规则以访问防火墙后面的 Azure 容器注册表](container-registry-firewall-access-rules.md)。

[az-acr-login]: https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-login
[az-acr-network-rule-add]: https://docs.azure.cn/cli/acr/network-rule/?view=azure-cli-latest#az-acr-network-rule-add
[az-acr-network-rule-remove]: https://docs.azure.cn/cli/acr/network-rule/?view=azure-cli-latest#az-acr-network-rule-remove
[az-acr-network-rule-list]: https://docs.azure.cn/cli/acr/network-rule/?view=azure-cli-latest#az-acr-network-rule-list
[az-acr-run]: https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-run
[az-acr-update]: https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-update
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.cn

[acr-access-selected-networks]: ./media/container-registry-access-selected-networks/acr-access-selected-networks.png
[acr-access-disabled]: ./media/container-registry-access-selected-networks/acr-access-disabled.png
[acr-access-all-networks]: ./media/container-registry-access-selected-networks/acr-access-all-networks.png

<!-- Update_Description: update meta properties, wording update, update link -->