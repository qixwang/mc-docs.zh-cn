---
title: Azure Service Fabric 中心机密存储
description: 本文介绍如何使用 Azure Service Fabric 中的中心机密存储。
ms.topic: conceptual
origin.date: 07/25/2019
ms.date: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: b376d6e1b237da7398830e07565262a2d3259a8c
ms.sourcegitcommit: afe972418a883551e36ede8deae32ba6528fb8dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2020
ms.locfileid: "77540492"
---
# <a name="central-secrets-store-in-azure-service-fabric"></a>Azure Service Fabric 中的中心机密存储 
本文介绍如何使用 Azure Service Fabric 中的中心机密存储 (CSS) 在 Service Fabric 应用程序中创建机密。 CSS 是一个本地机密存储缓存，用于保存敏感数据，例如，已在内存中加密的密码、令牌和密钥。

## <a name="enable-central-secrets-store"></a>启用中心机密存储
将以下脚本添加到群集配置中的 `fabricSettings` 下即可启用 CSS。 对于 CSS，我们建议使用除群集证书以外的证书。 确保在所有节点上安装加密证书，并且 `NetworkService` 对证书的私钥拥有读取权限。
```json
"fabricSettings": 
[
    ...
    {
        "name":  "CentralSecretService",
        "parameters":  [         
            {
                "name":  "IsEnabled",
                "value":  "true"
            },
            {
                "name":  "MinReplicaSetSize",
                "value":  "3"
            },
            {
                "name":  "TargetReplicaSetSize",
                "value":  "3"
            },
            {
                "name" : "EncryptionCertificateThumbprint",
                "value": "<thumbprint>"
            }
         ]
    }
    ...
]
```
## <a name="declare-a-secret-resource"></a>声明机密资源
可以使用 Azure 资源管理器模板或 REST API 创建机密资源。

### <a name="use-resource-manager"></a>使用资源管理器

使用以下资源管理器模板创建机密资源。 该模板将创建 `supersecret` 机密资源，但不会为机密资源设置任何值。

```json
"resources": [
    {
        "apiVersion": "2018-07-01-preview",
        "name": "supersecret",
        "type": "Microsoft.ServiceFabricMesh/secrets",
        "location": "[parameters('location')]", 
        "dependsOn": [],
        "properties": {
          "kind": "inlinedValue",
            "description": "Application Secret",
            "contentType": "text/plain",
          }
    }
]
```

### <a name="use-the-rest-api"></a>使用 REST API

若要使用 REST API 创建 `supersecret` 机密资源，请向 `https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview` 发出 PUT 请求。 需要提供群集证书或管理客户端证书来创建机密资源。

```powershell
Invoke-WebRequest  -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview -Method PUT -CertificateThumbprint <CertThumbprint>
```

## <a name="set-the-secret-value"></a>设置机密值

### <a name="use-the-resource-manager-template"></a>使用资源管理器模板

使用以下资源管理器模板创建和设置机密值。 此模板将 `supersecret` 机密资源的机密值设置为版本 `ver1`。
```json
  {
  "parameters": {
  "supersecret": {
      "type": "string",
      "metadata": {
        "description": "supersecret value"
      }
   }
  },
  "resources": [
    {
      "apiVersion": "2018-07-01-preview",
        "name": "supersecret",
        "type": "Microsoft.ServiceFabricMesh/secrets",
        "location": "[parameters('location')]", 
        "dependsOn": [],
        "properties": {
          "kind": "inlinedValue",
            "description": "Application Secret",
            "contentType": "text/plain",
        }
    },
    {
      "apiVersion": "2018-07-01-preview",
      "name": "supersecret/ver1",
      "type": "Microsoft.ServiceFabricMesh/secrets/values",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/secrets/supersecret"
      ],
      "properties": {
        "value": "[parameters('supersecret')]"
      }
    }
  ],
```
### <a name="use-the-rest-api"></a>使用 REST API

使用以下 REST API 脚本设置机密值。
```powershell
$Params = @{"properties": {"value": "mysecretpassword"}}
Invoke-WebRequest -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret/values/ver1?api-version=6.4-preview -Method PUT -Body $Params -CertificateThumbprint <ClusterCertThumbprint>
```
## <a name="use-the-secret-in-your-application"></a>在应用程序中使用机密

遵循以下步骤在 Service Fabric 应用程序中使用机密。

1. 在 **settings.xml** 文件中添加包含以下代码片段的节。 请注意，此处的值采用 {`secretname:version`} 格式。

    ```xml
     <Section Name="testsecrets">
      <Parameter Name="TopSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/
     </Section>
    ```

1. 将该节导入到 **ApplicationManifest.xml** 中。
    ```xml
     <ServiceManifestImport>
       <ServiceManifestRef ServiceManifestName="testservicePkg" ServiceManifestVersion="1.0.0" />
       <ConfigOverrides />
       <Policies>
         <ConfigPackagePolicies CodePackageRef="Code">
           <ConfigPackage Name="Config" SectionName="testsecrets" EnvironmentVariableName="SecretPath" />
           </ConfigPackagePolicies>
       </Policies>
     </ServiceManifestImport>
    ```

    环境变量 `SecretPath` 将指向存储所有机密的目录。 `testsecrets` 节下列出的每个参数存储在单独的文件中。 现在，应用程序可以使用该机密，如下所示：
    ```C#
    secretValue = IO.ReadFile(Path.Join(Environment.GetEnvironmentVariable("SecretPath"),  "TopSecret"))
    ```
1. 将机密装载到容器。 使机密在容器中可用而要做出的唯一更改就是在 `<ConfigPackage>` 中指定 (`specify`) 一个装入点。
    以下代码片段是修改后的 **ApplicationManifest.xml**。  

    ```xml
    <ServiceManifestImport>
       <ServiceManifestRef ServiceManifestName="testservicePkg" ServiceManifestVersion="1.0.0" />
       <ConfigOverrides />
       <Policies>
         <ConfigPackagePolicies CodePackageRef="Code">
           <ConfigPackage Name="Config" SectionName="testsecrets" MountPoint="C:\secrets" EnvironmentVariableName="SecretPath" />
           <!-- Linux Container
            <ConfigPackage Name="Config" SectionName="testsecrets" MountPoint="/mnt/secrets" EnvironmentVariableName="SecretPath" />
           -->

         </ConfigPackagePolicies>
       </Policies>
     </ServiceManifestImport>
    ```
    可在容器中的装入点下使用机密。

1. 可以通过指定 `Type='SecretsStoreRef` 将机密绑定到进程环境变量。 以下示例代码片段演示如何将 `supersecret` 版本 `ver1` 绑定到 **ServiceManifest.xml** 中的环境变量 `MySuperSecret`。

    ```xml
    <EnvironmentVariables>
     <EnvironmentVariable Name="MySuperSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/>
    </EnvironmentVariables>
    ```

## <a name="next-steps"></a>后续步骤
详细了解[应用程序和服务安全性](service-fabric-application-and-service-security.md)。

<!-- Update_Description: update meta properties, wording update, update link -->