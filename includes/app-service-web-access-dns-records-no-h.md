---
author: cephalin
ms.service: app-service-web
ms.topic: include
origin.date: 11/03/2016
ms.date: 03/16/2020
ms.author: v-tawe
ms.openlocfilehash: 20f9de4de0b3d36e0053bade366ad5885e84aa5d
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79546987"
---
> [!NOTE]
> 可以使用 Azure DNS 为 Azure 应用服务配置自定义 DNS 名称。 有关详细信息，请参阅[使用 Azure DNS 为 Azure 服务提供自定义域设置](../articles/dns/dns-custom-domain.md#app-service-web-apps)。
>
>

请登录到域提供商的网站。

查找管理 DNS 记录的页面。 每个域提供商都有自己的 DNS 记录界面，因此请查阅提供商的文档。 查找站点中标记为“域名”、“DNS”或“名称服务器管理”的区域。    

通常通过查看帐户信息，然后查找如“我的域”之类的链接，便可以找到 DNS 记录页面  。 转到该页面，然后查找名称类似于“区域文件”、“DNS 记录”或“高级配置”的链接    。

以下屏幕截图是 DNS 记录页的一个示例：

![示例 DNS 记录页](./media/app-service-web-access-dns-records-no-h/example-record-ui.png)

在示例屏幕截图中，选择“添加”以创建记录  。 某些提供商提供了不同的链接来添加不同的记录类型。 同样，请查阅提供商的文档。

> [!NOTE]
> 对于某些提供商（例如 GoDaddy），在你选择单独的“保存更改”链接之前，这些 DNS 记录不会生效  。 
