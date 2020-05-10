---
title: include 文件
description: include 文件
author: tfitzmac
ms.service: governance
ms.topic: include
origin.date: 03/26/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.custom: include file
ms.openlocfilehash: c8e0eba83f7480ead145aeb091e1f27abc39bb2e
ms.sourcegitcommit: 95efd248f5ee3701f671dbd5cfe0aec9c9959a24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82596204"
---
| 资源 | 限制 |
| --- | --- |
| 每个目录的管理组数 | 10,000 |
| 每个管理组的订阅数 | 不受限制。 |
| 管理组层次结构的级别数 | 根级别加上 6 个级别<sup>1</sup> |
| 每个管理组的直接父管理组数 | 一种 |
| 每个位置的[管理组级别部署数](../articles/azure-resource-manager/templates/deploy-to-management-group.md) | 800<sup>2</sup> |

<sup>1</sup>6 个级别不包括订阅级别。

<sup>2</sup>如果达到 800 个部署的限制，则会从历史记录中删除不再需要的部署。 若要删除管理组级别部署，请使用 [Remove-AzManagementGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/Remove-AzManagementGroupDeployment) 或 [az deployment mg delete](/cli/deployment/mg?view=azure-cli-latest#az-deployment-mg-delete)。
