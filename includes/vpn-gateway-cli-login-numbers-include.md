---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 03/21/2018
ms.date: 05/27/2019
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 21c199daeb6e399b33b8562f11c68da2f0a076c9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "68912295"
---
1. 使用 [az login](/cli/) 命令登录到 Azure 订阅，并按照屏幕上的说明进行操作。 有关登录的详细信息，请参阅 [Azure CLI 入门](/cli/get-started-with-azure-cli)。

   ```azurecli
   az cloud set -n AzureChinaCloud
   az login
   ```
2. 如果有多个 Azure 订阅，请列出该帐户的订阅。

   ```azurecli
   az account list --all
   ```
3. 指定要使用的订阅。

   ```azurecli
   az account set --subscription <replace_with_your_subscription_id>
   ```
