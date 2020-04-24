---
title: include 文件
description: include 文件
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 08/23/2019
ms.openlocfilehash: 17606d0c69959d2581dc6875bb6fb43acc8faee6
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78849863"
---
| 计算目标 | 用于 | GPU 支持 | FPGA 支持 | 说明 |
| ----- | ----- | ----- | ----- | ----- |
| [本地&nbsp;web&nbsp;服务](../articles/machine-learning/how-to-deploy-and-where.md#local) | 测试/调试 | &nbsp; | &nbsp; | 用于有限的测试和故障排除。 硬件加速依赖于本地系统中库的使用情况。
| [Azure 机器学习计算实例&nbsp;web&nbsp;服务](../articles/machine-learning/how-to-deploy-and-where.md#notebookvm) | 测试/调试 | &nbsp; | &nbsp; | 用于有限的测试和故障排除。
| [Azure 容器实例](../articles/machine-learning/how-to-deploy-and-where.md#aci) | 测试或开发 | &nbsp;  | &nbsp; | 用于需要小于 48 GB RAM 的基于 CPU 的小规模工作负载。 |
| [Azure 机器学习计算群集](../articles/machine-learning/how-to-use-parallel-run-step.md) | （预览）批处理&nbsp;推理 | [是](../articles/machine-learning/how-to-use-parallel-run-step.md)（机器学习管道） | &nbsp;  | 对无服务器计算运行批量评分。 支持普通 VM 和低优先级 VM。 |
| [Azure Functions](../articles/machine-learning/how-to-deploy-functions.md) | （预览）实时推理 | &nbsp; | &nbsp; | &nbsp; |
| [Azure IoT Edge](../articles/machine-learning/how-to-deploy-and-where.md#iotedge) | （预览）IoT&nbsp;模块 |  &nbsp; | &nbsp; | 在 IoT 设备上部署和提供 ML 模型。 |

> [!NOTE]
> 尽管计算目标（例如本地、Azure 机器学习计算实例和 Azure 机器学习计算群集）支持用于训练和试验的 GPU，但在__部署为 Web 服务__时，使用 GPU 进行推理仅在 Azure Kubernetes 服务中受支持。
>
> 只有在 Azure 机器学习计算上，才能__在通过机器学习管道评分时__使用 GPU 进行推理。