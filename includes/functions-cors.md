---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/01/2020
ms.author: v-junlch
ms.openlocfilehash: e2b35fd464126e1ce8751d003d7ea475a9d6c810
ms.sourcegitcommit: 1008ad28745709e8d666f07a90e02a79dbbe2be5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2020
ms.locfileid: "85945335"
---
Azure Functions 支持跨域资源共享 (CORS)。 CORS 在[门户中](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#cors)以及通过 [Azure CLI](/cli/functionapp/cors) 进行配置。 CORS 允许的来源列表应用于函数应用级别。 启用 CORS 后，响应包含 `Access-Control-Allow-Origin` 标头。 有关详细信息，请参阅 [跨域资源共享](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#cors)。 

