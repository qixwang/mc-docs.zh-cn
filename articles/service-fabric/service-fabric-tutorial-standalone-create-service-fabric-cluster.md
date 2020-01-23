---
title: 教程：安装 Service Fabric 独立客户端 - Azure Service Fabric
description: 本教程介绍如何在上一教程文章中创建的群集上安装 Service Fabric 独立客户端。
author: rockboyfor
ms.topic: tutorial
origin.date: 07/22/2019
ms.date: 01/13/2020
ms.author: v-yeche
ms.custom: mvc
ms.openlocfilehash: dbf6bc1854a513b86dc1940b827b615639c66586
ms.sourcegitcommit: 713136bd0b1df6d9da98eb1da7b9c3cee7fd0cee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75741973"
---
# <a name="tutorial-install-and-create-service-fabric-cluster"></a>教程：安装并创建 Service Fabric 群集

Service Fabric 独立群集提供相应的选项让我们选择自己的环境，并创建群集作为 Service Fabric 所采用的“任意 OS、任意云”方案的一部分。 在本系列教程中，将创建一个托管在 AWS 或 Azure 上的独立群集，并将应用程序安装到其中。

本教程是一个系列中的第二部分。 本教程将引导你完成创建 Service Fabric 独立群集的步骤。

此系列的第二部分介绍如何：

> [!div class="checklist"]
> * 下载并安装 Service Fabric 独立包
> * 创建 Service Fabric 群集
> * 连接到 Service Fabric 群集

## <a name="download-the-service-fabric-for-windows-server-package"></a>下载用于 Windows Server 的 Service Fabric 包

Service Fabric 提供了一个安装程序包，用于创建独立的 Service Fabric 群集。  在本地计算机上[下载安装程序包](https://go.microsoft.com/fwlink/?LinkId=730690)。  成功下载后，将其通过 RDP 连接复制到 VM，并将其粘贴到桌面上。

选择 zip 文件并打开上下文菜单，然后选择“全部提取” > “提取”。    提取文件时，将在桌面上生成一个与 zip 文件名相同的文件夹。

如果想要获取更多详细信息，请参阅[安装程序包的内容](service-fabric-cluster-standalone-package-contents.md)。

## <a name="set-up-your-configuration-file"></a>设置配置文件

由于要构建三节点 Windows 群集，因此需要修改 `ClusterConfig.Unsecure.MultiMachine.json` 文件。

接下来，将该文件中第 8、15 和 22 行中出现的三个 ipAddress 行更新为每个实例的 IP 地址。

更新节点后，它们将显示如下：

```json
{
    "nodeName": "vm0",
    "ipAddress": "172.31.27.1",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD0"
}
```

然后，需要更新几个属性。  在 34 行中，需要修改诊断存储的连接字符串，它应如下所示：`"connectionstring": "C:\\ProgramData\\SF\\DiagnosticsStore"`

最后，请在配置的 `nodeTypes` 节中添加一个新节来映射 windows 将使用的临时端口。  该配置文件应该如下所示：

```json
"applicationPorts": {
    "startPort": "20001",
    "endPort": "20031"
},
"ephemeralPorts": {
    "startPort": "20606",
    "endPort": "20861"
},
"isPrimary": true
```

## <a name="validate-the-environment"></a>验证环境

独立包中的 *TestConfiguration.ps1* 脚本可以用作最佳做法分析器，验证群集能否在给定环境中部署。 [部署准备](service-fabric-cluster-standalone-deployment-preparation.md)列出了先决条件和环境要求。 请运行以下脚本，验证你能否创建开发群集：

```powershell
cd .\Desktop\Microsoft.Azure.ServiceFabric.WindowsServer.6.2.274.9494\
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
```

可看到如下输出： 如果底部字段“Passed”的返回值为 `True`，那么已通过完整性检查，并且群集看似可以根据输入配置进行部署。

```powershell
Trace folder already exists. Traces will be written to existing trace folder: C:\Users\Administrator\Desktop\Microsoft.Azure.ServiceFabric.WindowsServer.6.2.274.9494\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.

LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 :
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
DataDrivesAvailable        : True
NoDomainController         : True
Passed                     : True
```

## <a name="create-the-cluster"></a>创建群集

成功验证群集配置后，运行 *CreateServiceFabricCluster.ps1* 脚本，将 Service Fabric 群集部署到配置文件中的虚拟机。

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA
```

如果一切正常，会得到如下所示的输出：

```powershell
Your cluster is successfully created! You can connect and manage your cluster using Azure Service Fabric Explorer or PowerShell. To connect through PowerShell, run 'Connect-ServiceFabricCluster [ClusterConnectionEndpoint]'.
```

> [!NOTE]
> 部署跟踪已写入运行 CreateServiceFabricCluster.ps1 PowerShell 脚本的 VM/计算机。 可在运行脚本的目录中的子文件夹 DeploymentTraces 中找到这些信息。 要确定是否已将 Service Fabric 正确部署到计算机，请根据群集配置文件 FabricSettings 部分中的详述找到 FabricDataRoot 目录（默认为 c:\ProgramData\SF）中安装的文件。 在任务管理器中也可以看到 FabricHost.exe 和 Fabric.exe 进程正在运行。
>
>

### <a name="bring-up-service-fabric-explorer"></a>打开 Service Fabric Explorer

现在可以通过 Service Fabric Explorer 连接到群集，既可以直接使用 http:\//localhost:19080/Explorer/index.html 从某台计算机进行连接，也可以使用 http:\//<*IPAddressofaMachine*>:19080/Explorer/index.html 进行远程连接。

## <a name="add-and-remove-nodes"></a>添加和删除节点

当业务需要改变时，可以向独立 Service Fabric 群集添加或删除节点。 参阅[向 Service Fabric 独立群集添加或删节点](service-fabric-cluster-windows-server-add-remove-nodes.md)以了解详细步骤。

## <a name="next-steps"></a>后续步骤

本系列的第二部分介绍了以并行方式将大量随机数据上传到存储帐户的方法，例如如何：

> [!div class="checklist"]
> * 配置连接字符串
> * 构建应用程序
> * 运行应用程序
> * 验证连接数

请继续学习本系列教程的第三部分，将应用程序安装到已创建的群集中。

> [!div class="nextstepaction"]
> [将应用程序安装到 Service Fabric 群集中](service-fabric-tutorial-standalone-install-an-application.md)

<!--Image references-->

[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png

<!-- Update_Description: update meta properties -->
