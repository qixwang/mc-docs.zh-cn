---
title: include 文件
description: include 文件
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 09/28/2019
ms.date: 05/11/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: cd338025476823fa7d76790fea38043ba6d09f86
ms.sourcegitcommit: 95efd248f5ee3701f671dbd5cfe0aec9c9959a24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82507688"
---
**不支持**以 0.0.0.0/0 为目标的用户定义路由和 GatewaySubnet 上的 NSG。 使用此配置创建的网关将被阻止创建。 网关需要访问管理控制器才能正常工作。 GatewaySubnet 上的 [BGP 路由传播](/virtual-network/virtual-networks-udr-overview#border-gateway-protocol)应设置为“已启用”，以确保网关可用。 如果此项设置为“已禁用”，则网关将不起作用。
