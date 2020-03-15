---
title: 在 Azure 存储中托管静态网站
description: 了解如何直接从 Azure 存储 GPv2 帐户中的容器提供静态内容（HTML、CSS、JavaScript 和图像文件）。
author: WenJason
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.author: v-jay
origin.date: 05/28/2019
ms.date: 03/09/2020
ms.openlocfilehash: 2354e6dfa8fdc466013c51451dc3227a51ee49fd
ms.sourcegitcommit: fbc7584f403417d3af7bd6bbbaed7c13a78c57b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2020
ms.locfileid: "78411453"
---
# <a name="host-a-static-website-in-azure-storage"></a>在 Azure 存储中托管静态网站

可以直接从 Azure 存储 GPv2 帐户中的容器提供静态内容（HTML、CSS、JavaScript 和图像文件）。 若要了解详细信息，请参阅 [Azure 存储中的静态网站托管](storage-blob-static-website.md)。

本文将展示如何使用 Azure 门户、Azure CLI 或 PowerShell 启用静态网站托管。

<a id="portal" />

## <a name="portal"></a>[门户](#tab/azure-portal)

有关分步教程，请参阅[教程：在 Blob 存储上托管静态网站](/storage/blobs/storage-blob-static-website-host)。

启用静态网站托管后，可以在浏览器中使用网站的公共 URL 来查看你的站点的页面。

<a id="portal-find-url" />

### <a name="find-the-website-url-by-using-the-azure-portal"></a>使用 Azure 门户查找网站 URL

在存储帐户的帐户概述页旁边显示的窗格中，选择“静态网站”  。 你的站点的 URL 显示在“主终结点”  字段中。

![Azure 存储静态网站指标 - 指标](./media/storage-blob-static-website/storage-blob-static-website-url.png)

<a id="cli" />

## <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

可以使用 [Azure 命令行接口 (CLI)](/cli/?view=azure-cli-latest) 启用静态网站托管。

1. 如果在本地[安装了](/cli/install-azure-cli?view=azure-cli-latest) Azure CLI，请打开命令控制台应用程序，例如 Windows PowerShell。

2. 如果你的标识已关联到多个订阅，请将活动订阅设置为将托管你的静态网站的存储帐户的订阅。

   ```azurecli
   az account set --subscription <subscription-id>
   ```

   将 `<subscription-id>` 占位符值替换为你的订阅 ID。

3. 启用静态网站托管。

   ```azurecli
   az storage blob service-properties update --account-name <storage-account-name> --static-website --404-document <error-document-name> --index-document <index-document-name>
   ```

   * 将 `<storage-account-name>` 占位符值替换为存储帐户的名称。

   * 将 `<error-document-name>` 占位符替换为当浏览器请求站点上不存在的页面时向用户显示的错误文档的名称。

   * 将 `<index-document-name>` 占位符替换为索引文档的名称。 此文档通常是“index.html”。

4. 将对象从源目录上传到 $web 容器  。

   此示例假设你从 Azure Cloud Shell 会话中运行命令。

   ```azurecli
   az storage blob upload-batch -s <source-path> -d $web --account-name <storage-account-name> --content-type 'text/html; charset=utf-8'
   ```

   * 将 `<storage-account-name>` 占位符值替换为存储帐户的名称。

   * 将 `<source-path>` 占位符替换为要上传的文件所在位置的路径。

   > [!NOTE]
   > 如果你使用 Azure CLI 的本地安装，则可以使用本地计算机上的任何位置的路径（例如：`C:\myFolder`）。

<a id="cli-find-url" />

### <a name="find-the-website-url-by-using-the-azure-cli"></a>使用 Azure CLI 查找网站 URL

可以在浏览器中使用网站的公共 URL 来查看内容。

可以使用以下命令查找 URL：

```azurecli
az storage account show -n <storage-account-name> -g <resource-group-name> --query "primaryEndpoints.web" --output tsv
```

* 将 `<storage-account-name>` 占位符值替换为存储帐户的名称。

* 将 `<resource-group-name>` 占位符值替换为资源组的名称。

<a id="powershell" />

## <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

可以使用 Azure PowerShell 模块启用静态网站托管。

1. 打开 Windows PowerShell 命令窗口。

2. 验证你是否具有 Azure PowerShell 模块 Az 版本 0.7 或更高版本。

   ```powershell
   Get-InstalledModule -Name Az -AllVersions | select Name,Version
   ```

   如果需要进行安装或升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-Az-ps)。

3. 运行 `Connect-AzAccount` 命令以登录 Azure 订阅，并按照屏幕上的说明操作。

   ```powershell
   Connect-AzAccount -Environment AzureChinaCloud
   ```

4. 如果你的标识已关联到多个订阅，请将活动订阅设置为将托管你的静态网站的存储帐户的订阅。

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

   将 `<subscription-id>` 占位符值替换为你的订阅 ID。

5. 获取定义了要使用的存储帐户的存储帐户上下文。

   ```powershell
   $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
   $ctx = $storageAccount.Context
   ```

   * 将 `<resource-group-name>` 占位符值替换为资源组的名称。

   * 将 `<storage-account-name>` 占位符值替换为存储帐户的名称。

6. 启用静态网站托管。

   ```powershell
   Enable-AzStorageStaticWebsite -Context $ctx -IndexDocument <index-document-name> -ErrorDocument404Path <error-document-name>
   ```

   * 将 `<error-document-name>` 占位符替换为当浏览器请求站点上不存在的页面时向用户显示的错误文档的名称。

   * 将 `<index-document-name>` 占位符替换为索引文档的名称。 此文档通常是“index.html”。

7. 将对象从源目录上传到 $web 容器  。

    ```powershell
    # upload a file
    set-AzStorageblobcontent -File "<path-to-file>" `
    -Properties @{ ContentType = "text/html; charset=utf-8";} `
    -Container `$web `
    -Blob "<blob-name>" `
    -Context $ctx
     ```

   * 将 `<path-to-file>` 占位符值替换为要上传的文件的完全限定路径（例如：`C:\temp\index.html`）。

   * 将 `<blob-name>` 占位符值替换为要为生成的 blob 提供的名称（例如：`index.html`）。

<a id="powershell-find-url" />

### <a name="find-the-website-url-by-using-powershell"></a>使用 PowerShell 查找网站 URL

可以在浏览器中使用网站的公共 URL 来查看内容。

可以使用以下命令查找 URL：

```powershell
 $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -Name "<storage-account-name>"
Write-Output $storageAccount.PrimaryEndpoints.Web
```

* 将 `<resource-group-name>` 占位符值替换为资源组的名称。

* 将 `<storage-account-name>` 占位符值替换为存储帐户的名称。

<a id="metrics" />

---

## <a name="enable-metrics-on-static-website-pages"></a>在静态网站页面上启用指标

启用指标后，指标仪表板会报告有关 $web 容器中的文件的流量统计信息  。

1. 单击“设置” > “监视” > “指标”。   

   将通过挂接到不同的指标 API 来生成指标数据。 门户只会显示在给定时间范围内使用的 API 成员，以便重点关注可返回数据的成员。 为确保能够选择所需的 API 成员，第一步是展开时间范围。

2. 单击时间范围按钮，选择“过去 24 小时”，然后单击“应用”   。

   ![Azure 存储静态网站指标 - 时间范围](./media/storage-blob-static-website/storage-blob-static-website-metrics-time-range.png)

3. 从“命名空间”下拉列表中选择“Blob”。  

   ![Azure 存储静态网站指标 - 命名空间](./media/storage-blob-static-website/storage-blob-static-website-metrics-namespace.png)

4. 然后选择“传出”指标。 

   ![Azure 存储静态网站指标 - 指标](./media/storage-blob-static-website/storage-blob-static-website-metrics-metric.png)

5. 从“聚合”选择器中选择“总和”。  

   ![Azure 存储静态网站指标 - 聚合](./media/storage-blob-static-website/storage-blob-static-website-metrics-aggregation.png)

6. 单击“添加筛选器”按钮，并从“属性”选择器中选择“API 名称”。   

   ![Azure 存储静态网站指标 - API 名称](./media/storage-blob-static-website/storage-blob-static-website-metrics-api-name.png)

7. 在“值”选择器中选中“GetWebContent”旁边的复选框，以填充指标报告。  

   ![Azure 存储静态网站指标 - GetWebContent](./media/storage-blob-static-website/storage-blob-static-website-metrics-getwebcontent.png)

## <a name="next-steps"></a>后续步骤

* 了解如何为静态网站配置自定义域。 请参阅[将自定义域映射到 Azure Blob 存储终结点](storage-custom-domain-name.md)。

