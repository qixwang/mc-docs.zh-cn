---
title: include 文件
description: include 文件
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 02/17/2020
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: 0d130cf531aa6790d7ca5ca7eec1ed76d6e05613
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77494429"
---
#### <a name="azure-storage-sdk-version-in-functions-1x"></a>Functions 1.x 中的 Azure 存储 SDK 版本

在 Functions 1.x 中，存储触发器和绑定使用 Azure 存储 SDK 版本 7.2.1（[WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) NuGet 包）。 如果引用不同版本的存储 SDK 并绑定到函数签名中的一种存储 SDK 类型，Functions 运行时可能报告无法绑定到该类型。 解决办法是确保项目引用 [WindowsAzure.Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1)。

