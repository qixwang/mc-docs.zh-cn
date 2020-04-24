---
title: 在 IoT 中心和设备预配服务 (DPS) 中弃用 TLS 1.0 和 1.1 | Microsoft Docs
description: 关于在 IoT 中心和 DPS 中弃用 TLS 1.0 和 1.1 以及支持的密码的准则。
author: rezas
ms.author: v-yiso
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
origin.date: 12/16/2019
ms.date: 01/13/2020
ms.openlocfilehash: c700a6e0b2c5340559abad4d50ebbbdcfb1ac111
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75631222"
---
# <a name="tls-10-and-11-deprecation-in-iot-hub-and-device-provisioning-service"></a>在 IoT 中心和设备预配服务中弃用 TLS 1.0 和 1.1

为了提供同类最佳的加密，IoT 中心和设备预配服务 (DPS) 将迁移到传输层安全性 (TLS) 1.2，这是 IoT 设备和服务的首选加密机制。 因此，我们将在 **2020 年 7 月 1日**弃用对 TLS 1.0 和 TLS 1.1 以及多个非建议旧密码的传统支持。


## <a name="impact"></a>影响
根据客户的特定环境和配置，弃用 TLS 1.0 和 1.1 以及不建议使用的旧式密码可能会对通过 IoT 中心或 DPS 进行的 IoT 设备和服务通信造成很大影响。 在某些情况下，与这些更改不兼容的设备和服务将无法在上述截止日期之后连接到 IoT 中心或 DPS。


## <a name="supported-ciphers"></a>支持的密码

TLS 握手期间只允许以下密码：

* TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
* TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
* TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384


## <a name="customer-feedback"></a>客户反馈

尽管 TLS 1.2 强制是行业范围内的最佳加密选择，并将按计划启用，但我们仍希望了解客户有关其特定部署的反馈以及在采用 TLS 1.2 时遇到哪些困难。 为此，你可以将评论发送到 [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com)。