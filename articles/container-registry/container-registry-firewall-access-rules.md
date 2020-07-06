---
title: 防火墙访问规则
description: 通过允许访问（“加入允许列表”）REST API 和数据终结点域名或特定于服务的 IP 地址范围，来配置规则以访问防火墙后的 Azure 容器注册表。
ms.topic: article
ms.date: 06/08/2020
ms.author: v-yeche
ms.openlocfilehash: ab438cc940c4bb33c9db7ab30a5b825dae61f4bb
ms.sourcegitcommit: 8dae792aefbe44e8388f961b813e3da6564423ec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84654899"
---
<!--Verified successfully-->
# <a name="configure-rules-to-access-an-azure-container-registry-behind-a-firewall"></a>配置规则以访问防火墙后的 Azure 容器注册表

本文介绍如何在防火墙上配置规则，以允许访问 Azure 容器注册表。 例如，防火墙或代理服务器后面的 Azure IoT Edge 设备可能需要访问容器注册表以拉取容器映像。 或者，本地网络中的锁定服务器可能需要访问权限以推送映像。

<!--Not Available on  [Configure Azure Private Link for an Azure container registry](container-registry-private-link.md)-->

## <a name="about-registry-endpoints"></a>关于注册表终结点

若要将映像或其他项目请求或推送到 Azure 容器注册表，客户端（例如 Docker 后台程序）需要通过 HTTPS 与两个不同的终结点进行交互。 对于访问防火墙后注册表的客户端，需要为这两个终结点配置访问规则。

* **注册表 REST API 终结点** - 通过注册表的公共 REST API 终结点处理身份验证和注册表管理操作。 此终结点是注册表的登录服务器名称。 示例： `myregistry.azurecr.cn`

* **存储（数据）终结点** - Azure 代表每个注册表在 Azure 存储帐户中[分配 blob 存储](container-registry-storage.md)，以管理容器映像和其他项目的数据。 当客户端访问 Azure 容器注册表中的映像层时，它会使用注册表提供的存储帐户终结点发出请求。

如果你的注册表是[异地复制](container-registry-geo-replication.md)的，则客户端可能需要与特定区域或多个已复制区域中的数据终结点交互。

## <a name="allow-access-to-rest-and-data-endpoints"></a>允许访问 REST 和数据终结点

* **REST 终结点** - 允许访问完全限定的注册表登录服务器名称、`<registry-name>.azurecr.cn` 或关联的 IP 地址范围
* **存储（数据）终结点** - 允许使用通配符 `*.blob.core.chinacloudapi.cn` 或关联的 IP 地址范围访问所有 Azure blob 存储帐户。

> [!NOTE]
> Azure 容器注册表即将引入[专用数据终结点](#enable-dedicated-data-endpoints-preview)（预览版），可以通过它为注册表存储严格限制客户端防火墙规则的作用域。 还可以选择使用 `<registry-name>.<region>.data.azurecr.cn` 的形式，启用注册表所在或复制到的所有区域中的数据终结点。

## <a name="allow-access-by-ip-address-range"></a>允许按 IP 地址范围进行访问

如果你的组织具有仅允许访问特定 IP 地址或地址范围的策略，请下载 [Azure IP 范围和服务标记 - 中国云](https://www.microsoft.com/download/confirmation.aspx?id=57062)。

若要查找需要允许访问的 ACR REST 终结点 IP 范围，请在 JSON 文件中搜索 AzureContainerRegistry。

> [!IMPORTANT]
> Azure 服务的 IP 地址范围可以更改，每周发布一次更新。 定期下载 JSON 文件，并在访问规则中进行必要的更新。 如果你的方案涉及在 Azure 虚拟网络中配置网络安全组规则，或者使用 Azure 防火墙，请改为使用 AzureContainerRegistry [服务标记](#allow-access-by-service-tag)。
>

### <a name="rest-ip-addresses-for-all-regions"></a>所有区域的 REST IP 地址

```json
{
  "name": "AzureContainerRegistry",
  "id": "AzureContainerRegistry",
  "properties": {
    "changeNumber": 10,
    "region": "",
    "platform": "Azure",
    "systemService": "AzureContainerRegistry",
    "addressPrefixes": [
      "40.73.136.24/29",
    [...]
```

<!--CORRECT ON 40.73.136.24/29-->


### <a name="rest-ip-addresses-for-a-specific-region"></a>特定区域的 REST IP 地址

搜索特定的区域，例如 AzureContainerRegistry.ChinaNorth。

```json
{
  "name": "AzureContainerRegistry.ChinaNorth",
  "id": "AzureContainerRegistry.ChinaNorth",
  "properties": {
    "changeNumber": 1,
    "region": "chinanorth",
    "platform": "Azure",
    "systemService": "AzureContainerRegistry",
    "addressPrefixes": [
      "139.217.48.104/29",
    [...]
```
<!--CORRECT ON 139.217.48.104/29-->

### <a name="storage-ip-addresses-for-all-regions"></a>所有区域的存储 IP 地址

```json
{
  "name": "Storage",
  "id": "Storage",
  "properties": {
    "changeNumber": 19,
    "region": "",
    "platform": "Azure",
    "systemService": "AzureStorage",
    "addressPrefixes": [
      "40.72.64.0/24",
    [...]
```

<!--CORRECT ON 40.72.64.0/24-->

### <a name="storage-ip-addresses-for-specific-regions"></a>特定区域的存储 IP 地址

搜索特定的区域，例如 Storage.ChinaNorth。

```json
{
  "name": "Storage.ChinaNorth",
  "id": "Storage.ChinaNorth",
  "properties": {
    "changeNumber": 1,
    "region": "chinanorth",
    "platform": "Azure",
    "systemService": "AzureStorage",
    "addressPrefixes": [
      "40.72.64.0/24"
    [...]
```

<!--CORRECT ON 40.72.64.0/24-->

## <a name="allow-access-by-service-tag"></a>允许通过服务标记访问

在 Azure 虚拟网络中，使用网络安全规则筛选从虚拟机等资源到容器注册表的流量。 若要简化 Azure 网络规则的创建，请使用 AzureContainerRegistry [服务标记](../virtual-network/security-overview.md#service-tags)。 服务标记代表一组用于全局或每个 Azure 区域访问 Azure 服务的 IP 地址前缀。 当地址更改时，将自动更新标记。 

例如，创建包含目标 AzureContainerRegistry 的出站网络安全组规则，以允许流量流向 Azure 容器注册表。 若要仅允许在特定区域中访问服务标记，请按以下格式指定区域：AzureContainerRegistry.[区域名称]。

## <a name="enable-dedicated-data-endpoints-preview"></a>启用专用数据终结点（预览）

<!--Verified successfully-->

> [!WARNING]
> 如果以前配置了对现有 `*.blob.core.chinacloudapi.cn` 终结点的客户端防火墙访问，则切换到专用数据终结点会影响客户端连接，从而导致拉取失败。 若要确保客户端具有一致的访问权限，请将新的数据终结点规则添加到客户端防火墙规则。 完成后，使用 Azure CLI 或其他工具为你的注册表启用专用数据终结点。

专用数据终结点是高级容器注册表服务层的一项可选功能。 有关注册表服务层级和限制的信息，请参阅 [Azure 容器注册表层](container-registry-skus.md)。 若要使用 Azure CLI 启用数据终结点，请使用 Azure CLI 版本 2.4.0 或更高版本。 如果需要进行安装或升级，请参阅[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli?view=azure-cli-latest)。

下面的 [az acr update][az-acr-update] 命令启用注册表 myregistry 上的专用数据终结点。 出于演示目的，假设在两个区域复制了注册表：

```azurecli
az acr update --name myregistry --data-endpoint-enabled
```

数据终结点使用区域模式 `<registry-name>.<region>.data.azurecr.cn`。 若要查看数据终结点，请使用 [az acr show-endpoints][az-acr-show-endpoints] 命令：

```azurecli
az acr show-endpoints --name myregistry
```

输出：

```
{
    "loginServer": "myregistry.azurecr.cn",
    "dataEndpoints": [
        {
            "region": "chinanorth",
            "endpoint": "myregistry.chinanorth.data.azurecr.cn",
        },
        {
            "region": "chinanorth",
            "endpoint": "myregistry.chinanorth.data.azurecr.cn",
        }
    ]
}
```

为注册表设置专用数据终结点后，可以为数据终结点启用客户端防火墙访问规则。 为所有必需的注册表区域启用数据终结点访问规则。

## <a name="configure-client-firewall-rules-for-mcr"></a>配置 MCR 的客户端防火墙规则

如果需要访问防火墙后的 Microsoft 容器注册表 (MCR)，请参阅配置 [MCR 客户端防火墙规则](https://github.com/microsoft/containerregistry/blob/master/client-firewall-rules.md)的指南。 MCR 是 Microsoft 发布的所有 docker 映像的主注册表，例如 Windows Server 映像。

<!--CORRECT ON Microsoft Container Registry (MCR)-->

## <a name="next-steps"></a>后续步骤

* 了解 [Azure 网络安全最佳做法](../security/fundamentals/network-best-practices.md)

* 详细了解 Azure 虚拟网络中的[安全组](/virtual-network/security-overview)

* 详细了解 Azure 容器注册表的[专用数据终结点](https://azure.microsoft.com/blog/azure-container-registry-mitigating-data-exfiltration-with-dedicated-data-endpoints/)

<!-- IMAGES -->

<!-- LINKS - External -->

<!-- LINKS - Internal -->

[az-acr-update]: https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-update
[az-acr-show-endpoints]: https://docs.azure.cn/cli/acr?view=azure-cli-latest#az-acr-show-endpoints

<!-- Update_Description: update meta properties, wording update, update link -->