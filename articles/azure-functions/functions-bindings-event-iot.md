---
title: 适用于 Azure Functions 的 Azure IoT 中心绑定
description: 了解如何在 Azure Functions 中使用 IoT 中心绑定。
author: craigshoemaker
ms.topic: reference
ms.date: 12/30/2019
ms.author: v-junlch
ms.openlocfilehash: aea3da5b2dc996b2bfd6ec9940206b4e60f0a40e
ms.sourcegitcommit: 6a8bf63f55c925e0e735e830d67029743d2c7c0a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75623644"
---
# <a name="azure-iot-hub-bindings-for-azure-functions"></a>适用于 Azure Functions 的 Azure IoT 中心绑定

本文介绍如何使用 IoT 中心的 Azure Functions 绑定。 IoT 中心支持基于 [Azure 事件中心绑定](functions-bindings-event-hubs.md)。

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>包 - Functions 1.x

对于 Azure Functions 版本 1.x，[Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet 包 2.x 版中提供了 IoT 中心绑定。 [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs) GitHub 存储库中提供了此包的源代码。

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x-and-higher"></a>包 - Functions 2.x 及更高版本

对于 Functions 2.x 及更高版本，请使用版本 3.x 的 [Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) 包。 [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs) GitHub 存储库中提供了此包的源代码。

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

> [!IMPORTANT]
> 虽然下述代码示例使用事件中心 API，但给定的语法适用于 IoT 中心函数。

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [详细了解 Azure Functions 触发器和绑定](functions-triggers-bindings.md)

<!-- Update_Description: wording update -->