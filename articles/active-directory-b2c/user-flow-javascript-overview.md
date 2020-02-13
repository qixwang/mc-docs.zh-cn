---
title: JavaScript 和页面布局版本
titleSuffix: Azure AD B2C
description: 了解如何在 Azure Active Directory B2C 中启用 JavaScript 和使用页面布局版本。
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/04/2020
ms.author: v-junlch
ms.subservice: B2C
ms.openlocfilehash: ddfc399365e626bcc98a32d65a0fb5c90cd9bfed
ms.sourcegitcommit: 888cbc10f2348de401d4839a732586cf266883bf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77028104"
---
# <a name="javascript-and-page-layout-versions-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 中的 JavaScript 和页面布局版本

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C 提供一组打包的内容，其中包含的 HTML、CSS 和 JavaScript 适用于用户流中的用户界面元素和自定义策略。 若要为应用程序启用 JavaScript，必须向[自定义策略](custom-policy-overview.md)添加一个元素，或者在门户中为用户流启用它，选择页面布局，并在请求中使用 [b2clogin.cn](b2clogin.md)。

如果你打算启用 [JavaScript](javascript-samples.md) 客户端代码，则需要确保 JavaScript 所基于的元素是不可变的。 否则，任何更改都可能会导致用户页上出现意外行为。 为了防止这些问题，可以强制使用页面布局，并指定页面布局版本。 这样做可以确保 JavaScript 所基于的所有内容定义不可变。 即使不打算启用 JavaScript，也可为页面指定页面布局版本。

## <a name="user-flows"></a>用户流

在用户流**属性**中，可以启用 JavaScript，这样也会强制使用页面布局。 然后就可以设置用户流的页面布局版本，如下一部分所述。

![用户流属性页面，突出显示了“启用 JavaScript”设置](./media/user-flow-javascript-overview/javascript-settings.png)

### <a name="select-a-page-layout-version"></a>选择页面布局版本

不管是否在用户流的属性中启用 JavaScript，都可以为用户流页面指定页面布局版本。 打开用户流，然后选择“页面布局”  。 在“布局名称”下选择一个用户流页面，然后选择“页面布局版本”。  

若要了解不同的页面布局版本，请参阅[版本更改日志](page-layout.md#version-change-log)。

![门户中的页面布局设置，显示了页面布局版本下拉列表](./media/user-flow-javascript-overview/page-layout-version.png)

## <a name="custom-policies"></a>自定义策略

若要在自定义策略中启用 JavaScript，请将 **ScriptExecution** 元素添加到自定义策略文件中的 **RelyingParty** 元素。 有关详细信息，请参阅[在 Azure Active Directory B2C 中使用的 JavaScript 示例](javascript-samples.md)。

不管是否在自定义策略中启用 JavaScript，都可以为页面指定页面布局版本。 有关指定页面布局的详细信息，请参阅[使用自定义策略在 Azure Active Directory B2C 中选择页面布局](page-layout.md)。

## <a name="next-steps"></a>后续步骤

若要了解不同的页面布局版本，请参阅[使用自定义策略在 Azure Active Directory B2C 中选择页面布局](page-layout.md#version-change-log)的“版本更改日志”部分。 

有关 JavaScript 使用的示例，可参阅[在 Azure Active Directory B2C 中使用的 JavaScript 示例](javascript-samples.md)。

<!-- Update_Description: wording update -->