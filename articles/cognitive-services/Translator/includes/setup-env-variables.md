---
author: Johnnytechn
ms.service: cognitive-services
ms.topic: include
ms.date: 08/07/2020
ms.author: v-johya
ms.openlocfilehash: 00e79b39e74a626dc91dcea1d7f09b4b72f518e6
ms.sourcegitcommit: caa18677adb51b5321ad32ae62afcf92ac00b40b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2020
ms.locfileid: "88024219"
---
## <a name="set-up"></a>设置

### <a name="create-a-translator-resource"></a>创建“翻译”资源

Azure 认知服务由你订阅的 Azure 资源表示。 在本地计算机上使用 [Azure 门户](/cognitive-services/cognitive-services-apis-create-account)或 [Azure CLI](/cognitive-services/cognitive-services-apis-create-account-cli) 创建用于“翻译”的资源。 你还可以：

* 查看 [Azure 门户](https://portal.azure.cn/)中的现有资源。

从试用订阅或资源获取密钥后，创建两个[环境变量](/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)：

* `TRANSLATOR_TEXT_SUBSCRIPTION_KEY` - “翻译”资源的订阅密钥。
* `TRANSLATOR_TEXT_ENDPOINT` - 翻译器的终结点。 改用 `https://api.translator.azure.cn/`

