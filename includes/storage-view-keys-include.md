---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 11/06/2019
ms.date: 01/06/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 13f1ab2340c82d794ae2c1195ab47cadeb33c4ea
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75623700"
---
若要从 Azure 门户查看和复制存储帐户访问密钥或连接字符串，请执行以下操作：

1. 导航到 [Azure 门户](https://portal.azure.cn)。
2. 找到自己的存储帐户。
3. 在存储帐户概述的“设置”部分，选择“访问密钥”。   此时会显示帐户访问密钥，以及每个密钥的完整连接字符串。
4. 找到“key1”下面的“密钥”值，单击“复制”按钮复制该帐户密钥。   
5. 或者，可复制整个连接字符串。 找到“密钥 1”下面的“连接字符串”值，单击“复制”按钮复制该连接字符串。   

    ![显示如何在 Azure 门户中查看访问密钥的屏幕截图](media/storage-view-keys-include/portal-connection-string.png)

可以使用任一密钥来访问 Azure 存储，但通常情况下，使用第一个密钥是很好的做法，并将第二个密钥保留到轮换密钥时使用。
