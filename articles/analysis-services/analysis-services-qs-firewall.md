---
title: 快速入门 - 配置 Azure Analysis Services 服务器防火墙 | Azure
description: 此快速入门可帮助你使用Azure 门户为 Azure Analysis Services 服务器配置防火墙。
author: rockboyfor
ms.service: azure-analysis-services
ms.topic: quickstart
origin.date: 05/19/2020
ms.date: 07/13/2020
ms.testscope: no
ms.testdate: 03/23/2020
ms.author: v-yeche
ms.reviewer: minewiskan
ms.openlocfilehash: 1c692d446723e68a1dffc72d2e6534af0c6b2d5f
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226083"
---
<!--Notice: Verified successfully-->
# <a name="quickstart-configure-server-firewall---portal"></a>快速入门：配置服务器防火墙 - 门户

此快速入门可帮助你为 Azure Analysis Services 服务器配置防火墙。 仅为访问服务器的计算机启用防火墙并配置 IP 地址范围是保护服务器和数据的重要部分。

## <a name="prerequisites"></a>先决条件

- 订阅中的 Analysis Services 服务器。 若要了解详细信息，请参阅[快速入门：创建服务器 - 门户](analysis-services-create-server.md)或[快速入门：创建服务器 - PowerShell](analysis-services-create-powershell.md)
- 客户端计算机的一个或多个 IP 地址范围（如果需要）。
- 即使启用了“允许从 Power BI 访问”，当前也不支持 Power BI Premium 连接到 Azure Analysis Services 的某些方案，包括数据导入（刷新）和分页报告。 支持从 Power BI Premium 使用 Live Connect 的更常见方案。 支持所有 Power BI Pro 方案。

## <a name="sign-in-to-the-azure-portal"></a>登录到 Azure 门户 

[登录门户](https://portal.azure.cn)

## <a name="configure-a-firewall"></a>配置防火墙

1. 单击服务器打开“概述”页。 
2. 在“设置” > “防火墙” > “启用防火墙”，单击“开”。
3. 若要允许从 Power BI 服务进行 DirectQuery 访问，请在“允许从Power BI 访问”中，单击“开”。  
4. （可选）指定一个或多个 IP 地址范围。 输入每个范围的名称、起始和结束 IP 地址。 防火墙规则名称应限制为 128 个字符，并且只能包含大写字符、小写字符、数字、下划线和连字符。 不允许使用空格和其他特殊字符。
5. 单击“保存” 。

    ![防火墙设置](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>清理资源

不再需要时，请删除 IP 地址范围或禁用防火墙。

## <a name="next-steps"></a>后续步骤
本快速入门介绍了如何为服务器配置防火墙。 现在你已有服务器，并通过防火墙对其进行了保护，你可以从门户向其添加基本示例数据模型。 拥有一个示例模型有助于了解如何配置模型数据库角色和测试客户端连接。 若要了解更多信息，请继续学习有关添加示例模型的教程。

> [!div class="nextstepaction"]
> [教程：将示例模型添加到服务器](analysis-services-create-sample-model.md)

<!-- Update_Description: update meta properties, wording update, update link -->