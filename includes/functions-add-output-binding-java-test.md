---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/03/2020
ms.author: v-junlch
ms.openlocfilehash: 8c4a8e592ecc6bcaffd046d097bc89b1d8a98fe7
ms.sourcegitcommit: f1a76ee3242698123a3d77f44c860db040b48f70
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84563713"
---
## <a name="update-the-tests"></a>更新测试

由于原型还创建一组测试，因此需更新这些测试，以便处理 `run` 方法签名中的新 `msg` 参数。  

浏览到测试代码在 _src/test/java_ 下的位置，打开 *Function.java* 项目文件，将 `//Invoke` 下的代码行替换为以下代码。

```java
@SuppressWarnings("unchecked")
final OutputBinding<String> msg = (OutputBinding<String>)mock(OutputBinding.class);
final HttpResponseMessage ret = new Function().run(req, msg, context);
```

