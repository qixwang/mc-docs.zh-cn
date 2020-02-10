---
title: 添加和管理 SSL 证书
description: 在 Azure 应用服务中创建免费的证书、导入应用服务证书、导入 Key Vault 证书或购买应用服务证书。
tags: buy-ssl-certificates
ms.topic: tutorial
origin.date: 10/25/2019
ms.date: 02/17/2020
ms.author: v-tawe
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: 3be6f3bb02320b6affa18a43e84e2779388fe704
ms.sourcegitcommit: ee2a3063185cd4c5dc24901366dbb726119d045d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2020
ms.locfileid: "76979344"
---
# <a name="add-an-ssl-certificate-in-azure-app-service"></a>在 Azure 应用服务中添加 SSL 证书

[Azure 应用服务](overview.md)提供高度可缩放、自修复的 Web 托管服务。 本文介绍如何创建私有证书或公用证书，或将其上传或导入到应用服务中。 

将证书添加到应用服务应用或[函数应用](https://docs.azure.cn/azure-functions/)后，即可[使用它来保护自定义 DNS 名称](configure-ssl-bindings.md)或[在应用程序代码中使用它](configure-ssl-certificate-in-code.md)。

下表列出了用于在应用服务中添加证书的选项：

|选项|说明|
|-|-|
| 创建免费应用服务托管证书（预览版） | 如果只需保护 `www` [自定义域](app-service-web-tutorial-custom-domain.md)或应用服务中的任何非裸域，则可以轻松使用私有证书。 |
| 购买应用服务证书 | 由 Azure 管理的私有证书。 它结合了自动化证书管理的简单性以及续订和导出选项的灵活性。 |
| 导入来自 Key Vault 的证书 | 这在使用 [Azure Key Vault](https://docs.azure.cn/key-vault/) 管理 [PKCS12 证书](https://wikipedia.org/wiki/PKCS_12)时很有用。 请参阅[私有证书要求](#private-certificate-requirements)。 |
| 上传私有证书 | 如果你已有第三方提供商提供的私有证书，则可以上传它。 请参阅[私有证书要求](#private-certificate-requirements)。 |
| 上传公用证书 | 公用证书不用于保护自定义域，但可以将其加载到代码中（如果需要它们来访问远程资源）。 |

## <a name="prerequisites"></a>必备条件

按照本操作方法指南操作：

- [创建应用服务应用](/app-service/)。
- 仅限免费证书：使用 [CNAME 记录](app-service-web-tutorial-custom-domain.md#map-a-cname-record)将子域（例如 `www.contoso.com`）映射到应用服务。

## <a name="private-certificate-requirements"></a>私有证书要求

[免费应用服务托管证书](#create-a-free-certificate-preview)或[应用服务证书](#import-an-app-service-certificate)已满足应用服务的要求。 如果选择将私有证书上传或导入到应用服务，则证书必须满足以下要求：

* 已导出为[受密码保护的 PFX 文件](https://wikipedia.org/w/index.php?title=X.509&section=4#Certificate_filename_extensions)
* 包含长度至少为 2048 位的私钥
* 包含证书链中的所有中间证书

若要保护 SSL 绑定中的自定义域，证书还有其他要求：

* 包含用于服务器身份验证的[扩展密钥用法](https://wikipedia.org/w/index.php?title=X.509&section=4#Extensions_informing_a_specific_usage_of_a_certificate) (OID = 1.3.6.1.5.5.7.3.1)
* 已由受信任的证书颁发机构签名

> [!NOTE]
> **椭圆曲线加密 (ECC) 证书**可用于应用服务，但本文不予讨论。 请咨询证书颁发机构，了解有关创建 ECC 证书的确切步骤。

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

<!-- ## Create a free certificate (Preview) -->

<!-- ## Import an App Service Certificate -->

<!-- ## Import a certificate from Key Vault -->

## <a name="upload-a-private-certificate"></a>上传私有证书

从证书提供者处获得证书以后，请执行此部分的步骤，使证书可供应用服务使用。

### <a name="merge-intermediate-certificates"></a>合并中间证书

如果证书颁发机构在证书链中提供了多个证书，则需按顺序合并证书。

若要执行此操作，请在文本编辑器中打开收到的每个证书。

创建名为 mergedcertificate.crt  的合并证书文件。 在文本编辑器中，将每个证书的内容复制到此文件。 证书的顺序应遵循证书链中的顺序，以你的证书开头，以根证书结尾， 如以下示例所示：

```
-----BEGIN CERTIFICATE-----
<your entire Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-to-pfx"></a>将证书导出为 PFX

用生成证书请求时所用的私钥导出合并的 SSL 证书。

如果使用 OpenSSL 生成证书请求，则已创建私钥文件。 若要将证书导出为 PFX，请运行以下命令。 将占位符 _&lt;private-key-file>_ 和 _&lt;merged-certificate-file>_ 分别替换为私钥和合并证书文件的路径。

```bash
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

出现提示时，定义导出密码。 稍后将 SSL 证书上传到应用服务时需使用此密码。

如果使用了 IIS 或 _Certreq.exe_ 来生成证书请求，请将证书安装到你的本地计算机，然后[将证书导出为 PFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx)。

### <a name="upload-certificate-to-app-service"></a>将证书上传到应用服务

现在可以将证书上传到应用服务了。

在 <a href="https://portal.azure.cn" target="_blank">Azure 门户</a>的左侧菜单中，选择“应用程序服务” > “\<app-name>”   。

在应用的左侧导航窗格中，选择“TLS/SSL 设置” > “私钥证书(.pfx)” > “上载证书”    。

![将私有证书上传到应用服务中](./media/configure-ssl-certificate/upload-private-cert.png)

在“PFX 证书文件”中，选择你的 PFX 文件。  在“证书密码”中，键入导出 PFX 文件时创建的密码。  完成后，单击“上传”  。 

操作完成后，会在“私钥证书”列表中看到该证书  。

![上传证书文件已完成](./media/configure-ssl-certificate/create-free-cert-finished.png)

> [!IMPORTANT] 
> 若要使用此证书保护自定义域，仍需要创建证书绑定。 按照[创建绑定](configure-ssl-bindings.md#create-binding)中的步骤操作。
>

## <a name="upload-a-public-certificate"></a>上传公用证书

公共证书可以使用 *.cer* 格式。 

在 <a href="https://portal.azure.cn" target="_blank">Azure 门户</a>的左侧菜单中，选择“应用程序服务” > “\<app-name>”   。

在应用的左侧导航窗格中，单击“TLS/SSL 设置” > “公用证书(.cer)” > “上传公钥证书”    。

在“名称”  中，键入证书的名称。 在“CER 证书文件”中  ，选择你的 CER 文件。

单击“上传”。 

![将公用证书上传到应用服务中](./media/configure-ssl-certificate/upload-public-cert.png)

上传证书以后，请复制证书指纹并参阅[使证书可供访问](configure-ssl-certificate-in-code.md#make-the-certificate-accessible)。

<!-- ## Manage App Service certificates -->

## <a name="automate-with-scripts"></a>使用脚本自动执行

### <a name="azure-cli"></a>Azure CLI

```azurecli
#!/bin/bash

fqdn=<replace-with-www.{yourdomain}>
pfxPath=<replace-with-path-to-your-.PFX-file>
pfxPassword=<replace-with-your=.PFX-password>
resourceGroup=myResourceGroup
webappname=mywebapp$RANDOM

# Create a resource group.
az group create --location chinaeast --name $resourceGroup

# Create an App Service plan in Basic tier (minimum required by custom domains).
az appservice plan create --name $webappname --resource-group $resourceGroup --sku B1

# Create a web app.
az webapp create --name $webappname --resource-group $resourceGroup \
--plan $webappname

echo "Configure a CNAME record that maps $fqdn to $webappname.chinacloudsites.cn"
read -p "Press [Enter] key when ready ..."

# Before continuing, go to your DNS configuration UI for your custom domain and follow the 
# instructions at https://aka.ms/appservicecustomdns to configure a CNAME record for the 
# hostname "www" and point it your web app's default domain name.

# Map your prepared custom domain name to the web app.
az webapp config hostname add --webapp-name $webappname --resource-group $resourceGroup \
--hostname $fqdn

# Upload the SSL certificate and get the thumbprint.
thumbprint=$(az webapp config ssl upload --certificate-file $pfxPath \
--certificate-password $pfxPassword --name $webappname --resource-group $resourceGroup \
--query thumbprint --output tsv)

# Binds the uploaded SSL certificate to the web app.
az webapp config ssl bind --certificate-thumbprint $thumbprint --ssl-type SNI \
--name $webappname --resource-group $resourceGroup

echo "You can now browse to https://$fqdn"
```

### <a name="powershell"></a>PowerShell

```powershell
$fqdn="<Replace with your custom domain name>"
$pfxPath="<Replace with path to your .PFX file>"
$pfxPassword="<Replace with your .PFX password>"
$webappname="mywebapp$(Get-Random)"
$location="China East"

# Create a resource group.
New-AzResourceGroup -Name $webappname -Location $location

# Create an App Service plan in Free tier.
New-AzAppServicePlan -Name $webappname -Location $location `
-ResourceGroupName $webappname -Tier Free

# Create a web app.
New-AzWebApp -Name $webappname -Location $location -AppServicePlan $webappname `
-ResourceGroupName $webappname

Write-Host "Configure a CNAME record that maps $fqdn to $webappname.chinacloudsites.cn"
Read-Host "Press [Enter] key when ready ..."

# Before continuing, go to your DNS configuration UI for your custom domain and follow the 
# instructions at https://aka.ms/appservicecustomdns to configure a CNAME record for the 
# hostname "www" and point it your web app's default domain name.

# Upgrade App Service plan to Basic tier (minimum required by custom SSL certificates)
Set-AzAppServicePlan -Name $webappname -ResourceGroupName $webappname `
-Tier Basic

# Add a custom domain name to the web app. 
Set-AzWebApp -Name $webappname -ResourceGroupName $webappname `
-HostNames @($fqdn,"$webappname.chinacloudsites.cn")

# Upload and bind the SSL certificate to the web app.
New-AzWebAppSSLBinding -WebAppName $webappname -ResourceGroupName $webappname -Name $fqdn `
-CertificateFilePath $pfxPath -CertificatePassword $pfxPassword -SslState SniEnabled
```

## <a name="more-resources"></a>更多资源

* [使用 SSL 绑定保护自定义 DNS 名称](configure-ssl-bindings.md)
* [实施 HTTPS](configure-ssl-bindings.md#enforce-https)
* [实施 TLS 1.1/1.2](configure-ssl-bindings.md#enforce-tls-versions)
* [在应用程序代码中使用 SSL 证书](configure-ssl-certificate-in-code.md)
* [常见问题解答：应用服务证书](https://docs.azure.cn/app-service/faq-configuration-and-management/)
