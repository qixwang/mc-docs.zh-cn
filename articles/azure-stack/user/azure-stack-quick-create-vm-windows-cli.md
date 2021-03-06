---
title: 使用 Azure CLI 在 Azure Stack Hub 中创建 Windows 虚拟机
description: 使用 Azure CLI 在 Azure Stack Hub 中创建 Windows 虚拟机
author: WenJason
ms.topic: quickstart
origin.date: 1/22/2020
ms.date: 05/18/2020
ms.author: v-jay
ms.lastreviewed: 01/14/2020
ms.openlocfilehash: 38e5b78cdf1fd317618bb8f29454c0c242fea02d
ms.sourcegitcommit: 134afb420381acd8d6ae56b0eea367e376bae3ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/15/2020
ms.locfileid: "83422540"
---
# <a name="quickstart-create-a-windows-server-virtual-machine-using-azure-cli-in-azure-stack-hub"></a>快速入门：使用 Azure CLI 在 Azure Stack Hub 中 创建 Windows Server 虚拟机

可以使用 Azure CLI 创建 Windows Server 2016 虚拟机。 请按照本文中的步骤创建和使用虚拟机。 本文还提供了以下步骤：

* 通过远程客户端连接到虚拟机。
* 安装 IIS Web 服务器并查看默认主页。
* 清理资源。

## <a name="prerequisites"></a>先决条件

* 确保 Azure Stack Hub 操作员已将“Windows Server 2016”  映像添加到 Azure Stack Hub 市场。

* Azure Stack Hub 需要使用特定版本的 Azure CLI 来创建和管理资源。 如果尚未为 Azure Stack Hub 配置 Azure CLI，请按照[安装和配置 Azure CLI](azure-stack-version-profiles-azurecli2.md) 的步骤进行操作。

## <a name="create-a-resource-group"></a>创建资源组

资源组是一个逻辑容器，可以在其中部署和管理 Azure Stack Hub 资源。 在 Azure Stack Hub 环境中，运行 [az group create](/cli/group#az-group-create) 命令以创建资源组。

> [!NOTE]
>  代码示例中为所有变量都分配了值。 但是，如果愿意，也可以分配新值。

以下示例在本地位置创建名为 myResourceGroup 的资源组：

```cli
az group create --name myResourceGroup --location local
```

## <a name="create-a-virtual-machine"></a>创建虚拟机

可以使用 [az vm create](/cli/vm#az-vm-create) 命令创建虚拟机 (VM)。 以下示例创建名为 myVM 的 VM。 此示例使用 Demouser 作为管理员用户名，并使用 Demouser@123 作为管理员密码。 将这些值更改为适合你的环境的值。

```cli
az vm create \
  --resource-group "myResourceGroup" \
  --name "myVM" \
  --image "Win2016Datacenter" \
  --admin-username "Demouser" \
  --admin-password "Demouser@123" \
  --location local
```

创建 VM 时，输出中的 **PublicIPAddress** 参数包含虚拟机的公共 IP 地址。 记下此地址，因为需要使用它来访问虚拟机。

## <a name="open-port-80-for-web-traffic"></a>为 Web 流量打开端口 80

由于此 VM 将运行 IIS Web 服务器，因此需要为 Internet 流量打开端口 80。

使用 [az vm open-port](/cli/vm) 命令打开端口 80：

```cli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="connect-to-the-virtual-machine"></a>连接到虚拟机

使用以下命令创建到虚拟机的远程桌面连接。 将“Public IP Address”替换为你的虚拟机的 IP 地址。 系统询问时，输入用于虚拟机的用户名和密码。

```
mstsc /v <Public IP Address>
```

## <a name="install-iis-using-powershell"></a>使用 PowerShell 安装 IIS

现在，你已登录到虚拟机，可以使用 PowerShell 来安装 IIS 了。 在虚拟机上启动 PowerShell 并运行以下命令：

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a>查看 IIS 欢迎页

可以使用所选的浏览器查看默认 IIS 欢迎页。 请使用前面部分中列出的公共 IP 地址来访问默认页面：

![IIS 默认站点](./media/azure-stack-quick-create-vm-windows-cli/default-iis-website.png)

## <a name="clean-up-resources"></a>清理资源

清理不再需要的资源。 可以使用 [az group delete](/cli/group#az-group-delete) 命令来删除资源组、虚拟机和所有相关的资源。

```cli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

在本快速入门中，你已部署了一台基本的 Windows Server 虚拟机。 若要详细了解 Azure Stack Hub 虚拟机，请继续阅读[有关 Azure Stack Hub 中虚拟机的注意事项](azure-stack-vm-considerations.md)。
