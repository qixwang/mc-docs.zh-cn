---
title: Service Fabric 机密存储
description: 本文介绍如何使用 Service Fabric 机密存储。
ms.topic: conceptual
origin.date: 07/25/2019
ms.author: v-yeche
ms.date: 01/06/2020
ms.openlocfilehash: af3b296d4947e10e2bbb6ebfc066f6b1a4b3abcc
ms.sourcegitcommit: 713136bd0b1df6d9da98eb1da7b9c3cee7fd0cee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75742279"
---
# <a name="service-fabric-secrets-store"></a>Service Fabric 机密存储
本文介绍如何使用 Service Fabric 机密存储 (CSS) 在 Service Fabric 应用程序中创建和使用机密。 CSS 是一个本地机密存储缓存，用于保存敏感数据，例如，已在内存中加密的密码、令牌和密钥。

## <a name="enabling-secrets-store"></a>启用机密存储
将以下内容添加到群集配置中的 `fabricSettings` 下即可启用 CSS。 建议对 CSS 使用不同于群集证书的证书。 确保在所有节点上安装加密证书，并且 `NetworkService` 对证书的私钥拥有读取权限。
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
## <a name="declare-secret-resource"></a>声明机密资源
可以使用资源管理器模板或 REST API 创建机密资源。

* 使用 Resource Manager 模板
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
上述模板将创建 `supersecret` 机密资源，但不会为机密资源设置任何值。

* 使用 REST API

为了创建机密资源，`supersecret` 将向 `https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview` 发出 PUT 请求。 需要提供群集证书或管理客户端证书来创建机密。

```powershell
Invoke-WebRequest  -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview -Method PUT -CertificateThumbprint <CertThumbprint>
```

## <a name="set-secret-value"></a>设置机密值
* 使用 Resource Manager 模板

以下资源管理器模板创建版本为 `ver1` 的机密 `supersecret` 并设置其值。
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
* 使用 REST API

```powershell
$Params = @{"properties": {"value": "mysecretpassword"}}
Invoke-WebRequest -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret/values/ver1?api-version=6.4-preview -Method PUT -Body $Params -CertificateThumbprint <ClusterCertThumbprint>
```
## <a name="using-the-secret-in-your-application"></a>在应用程序中使用机密

1. 在 settings.xml 文件中添加包含以下内容的节。 请注意，此处的 Value 的格式为 {`secretname:version`}

```xml
  <Section Name="testsecrets">
   <Parameter Name="TopSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/
  </Section>
```
2. 现在，将该节导入到 ApplicationManifest.xml 中
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

环境变量“SecretPath”将指向存储所有机密的目录。 `testsecrets` 节下列出的每个参数将存储在单独的文件中。 现在，应用程序可以使用机密，如下所示

```C#
secretValue = IO.ReadFile(Path.Join(Environment.GetEnvironmentVariable("SecretPath"),  "TopSecret"))
```
3. 将机密装载到容器

使机密在容器中可用而要做出的唯一更改就是在 `<ConfigPackage>` 中指定一个装入点。
下面是修改后的 ApplicationManifest.xml  

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

4. 将机密绑定到环境变量 

可以通过指定 Type='SecretsStoreRef' 将机密绑定到进程环境变量。 以下示例演示如何将 `supersecret` 版本 `ver1` 绑定到 ServiceManifest.xml 中的环境变量 `MySuperSecret`。

```xml
<EnvironmentVariables>
  <EnvironmentVariable Name="MySuperSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/>
</EnvironmentVariables>
```
## <a name="next-steps"></a>后续步骤
深入了解[应用程序和服务安全性](service-fabric-application-and-service-security.md)

<!-- Update_Description: new article about service fabric application secret store -->
<!--NEW.date: 12/09/2019-->