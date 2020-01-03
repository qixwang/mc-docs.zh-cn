---
title: Service Fabric Azure 资源管理器部署准则 | Azure
description: 本文概述通过 Azure 资源管理器部署 Service Fabric 群集时常犯的错误以及如何避免它们。
services: service-fabric
documentationcenter: .net
author: rockboyfor
manager: digimobile
editor: ''
ms.assetid: 803c9c63-373a-4d6a-8ef2-ea97e16e88dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
origin.date: 10/30/2019
ms.date: 12/09/2019
ms.author: v-yeche
ms.openlocfilehash: 2d1fa5d22045b8b7fe312f46caf63a255f26aef3
ms.sourcegitcommit: 4a09701b1cbc1d9ccee46d282e592aec26998bff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348522"
---
# <a name="service-fabric-guardrails"></a>Service Fabric 准则 
部署 Service Fabric 群集时，需遵循相应准则，使用有效的群集配置通过 Azure 资源管理器进行部署，否则就会失败。 以下部分概述了常见的群集配置问题，以及解决这些问题所需的步骤。 

## <a name="durability-mismatch"></a>持久性不匹配
### <a name="overview"></a>概述
Service Fabric 节点类型的持久性值在 Azure 资源管理器模板的两个不同部分定义。 这两个部分是虚拟机规模集资源的虚拟机规模集扩展部分，以及 Service Fabric 群集资源的节点类型部分。 这两个部分中的持久性值必须匹配，否则部署会失败。

以下部分包含一个示例，该示例展示了虚拟机规模集扩展持久性设置和 Service Fabric 节点类型持久性设置之间存在的持久性不匹配情况：  

**虚拟机规模集持久性设置**
```json 
{
  "extensions": [
    {
      "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
      "properties": {
        "type": "ServiceFabricNode",
        "publisher": "Microsoft.Azure.ServiceFabric",
        "settings": {
          "durabilityLevel": "Silver",
        }
      }
    }
  ]
}
```

**Service Fabric 节点类型持久性设置** 
```json
{
  "nodeTypes": [
    {
      "name": "[variables('vmNodeType0Name')]",
      "durabilityLevel": "Bronze",
      "isPrimary": true,
      "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    }
  ]
}
```

### <a name="error-messages"></a>错误消息
* 虚拟机规模集持久性与当前的 Service Fabric 节点类型持久性级别不匹配
* 虚拟机规模集持久性与目标 Service Fabric 节点类型持久性级别不匹配
* 虚拟机规模集持久性与当前的 Service Fabric 持久性级别或目标 Service Fabric 节点类型持久性级别不匹配 

### <a name="mitigation"></a>缓解措施
若要修复上述任何错误消息所指示的持久性不匹配问题，请执行以下操作：
1. 更新 Azure 资源管理器模板的虚拟机规模集扩展或 Service Fabric 节点类型部分中的持久性级别，确保这些值匹配。
2. 使用更新的值重新部署 Azure 资源管理器模板。

## <a name="next-steps"></a>后续步骤
* 在运行 Windows Server 的 VM 或计算机上创建群集：[创建适用于 Windows Server 的 Service Fabric 群集](service-fabric-cluster-creation-for-windows-server.md)
* 在运行 Linux 的 VM 或计算机上创建群集：[创建 Linux 群集](service-fabric-cluster-creation-via-portal.md)
* Service Fabric 故障排除：[故障排除指南](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)

<!-- Update_Description: new article about service fabric azure resource manager guardrails -->
<!--NEW.date: 12/09/2019-->