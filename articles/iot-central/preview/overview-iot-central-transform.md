---
title: 转换概述
description: 了解如何执行转换
author: viv-liu
ms.author: v-yiso
origin.date: 10/11/2019
ms.date: 12/16/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: 8e4240a096eac0eb1461df75842582d56fcbad7b
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74883264"
---
# <a name="transform-your-iot-data-preview-features"></a>转换 IoT 数据（预览版功能）

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

作为一个应用程序平台，IoT Central 提供几项关键功能来帮助你将 IoT 数据转换为业务见解，并从中促成可行的结果。 IoT Central 可让你将 IoT 数据扩展到外部系统，以便与业务线应用程序和自定义应用程序建立集成。 若要监视设备的运行状况和操作，可以创建规则来通过移动服务通知技术人员。 可以通过将原始遥测数据导出到 Azure 中的服务，使用自定义分析和机器学习生成具体的业务见解。 可以使用公共 API 创建用于监视和控制设备以及管理 IoT Central 应用的服务与工具。 

![IoT Central 中的转换概述](media/overview-iot-central-transform/transform.png)

## <a name="monitor-device-health-and-operations-using-rules"></a>使用规则监视设备运行状况和操作
连接计算机并发送数据后，可以识别哪些计算机遇到了问题或发送了错误消息，以便可以在最大限度地减少停机时间的情况下修复这些计算机，使之恢复正常运行。 可以在 IoT Central 应用中生成规则，以监视设备发送的遥测数据，并在超过阈值或者发送了特定的事件消息时收到警报。 可为规则配置电子邮件等操作和 Webhook，以通知适当的人员和适当的下游系统。 在此处详细了解规则。

## <a name="run-custom-analytics-and-processing-on-your-exported-data"></a>对导出的数据运行自定义分析和处理
可以通过生成自定义分析管道来处理原始 IoT 遥测并存储最终结果，来生成高度自定义的业务见解，例如，确定机器的效率趋势，或预测工厂车间的未来能耗。 可以在 IoT Central 应用中创建数据导出，以将遥测数据、设备属性和生命周期以及设备模板更改信息导出到其他 Azure 服务，从而在最有用的工具中分析、存储和可视化数据。 在此处详细了解数据导出。

## <a name="build-custom-iot-solutions-and-integrations-with-apis"></a>使用 API 生成自定义 IoT 解决方案和集成
可以生成 IoT 解决方案（例如，可以遥控设备和设置新设备的手机伴侣应用），或者生成与现有业务线应用程序的自定义集成，以便与针对企业定制的 IoT 设备和数据进行交互。 可以使用 IoT Central 作为设备建模、加入、管理和数据访问的主干。 在此学习模块中详细了解公共 API。
