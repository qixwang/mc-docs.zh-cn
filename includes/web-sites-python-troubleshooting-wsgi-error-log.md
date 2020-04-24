---
title: include 文件
description: include 文件
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 06/11/2018
ms.date: 03/16/2020
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: d409c28b68a84dab92460c34a209e40ecc2a0eb0
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79549608"
---
如果 Python 在启动应用程序时遇到错误，只只会返回简单的错误页（例如“由于发生内部服务器错误，无法显示该页。”）。

捕获 Python 应用程序错误：

1. 在 Azure 门户中的 Web 应用中，选择“设置”  。
2. 在“设置”  选项卡上，选择“应用程序设置”  。
3. 在“应用设置”  下，输入以下键/值对：
    * 键：WSGI_LOG
    * 值：D:\home\site\wwwroot\logs.txt（输入所选文件名）

现在应可在 wwwroot 文件夹中的 logs.txt 文件中看到错误。
