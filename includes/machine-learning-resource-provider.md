---
author: blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/09/2020
ms.author: larryfr
ms.openlocfilehash: 5ffeb3f2dad402bc8fbd363308116f67082ac9b9
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155012"
---
创建 Azure 机器学习工作区或工作区使用的资源时，可能会收到类似于以下消息的错误：

* `No registered resource provider found for location {location}`
* `The subscription is not registered to use namespace {resource-provider-namespace}`

大多数资源提供程序均已自动注册，但并非全部。 如果收到此消息，则需要注册提到的提供程序。

有关注册资源提供程序的信息，请参阅[解决资源提供程序注册的错误](../articles/azure-resource-manager/templates/error-register-resource-provider.md)。