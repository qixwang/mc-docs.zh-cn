---
author: cephalin
ms.service: app-service
ms.topic: include
origin.date: 11/03/2016
ms.date: 09/10/2019
ms.author: v-tawe
ms.openlocfilehash: ba6c5c992b386a0a4fb6d95f229b6e3c7a4572fe
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "71059592"
---
使用 Azure 资源管理器，可以在部署模板时为要使用的值定义参数。 模板包括 `parameters` 节，其中包含所有参数值。 模板使用每个参数值定义要部署的资源。

> [!NOTE]
> 不要为始终保持不变的值定义参数。 仅为随着要部署的项目或要部署到的环境而变化的值定义参数。

定义参数时：

* 若要指定用户可以在部署过程中提供的允许值，请使用 **allowedValues** 字段。

* 若要在部署过程中未提供值的情况下为参数分配默认值，请使用 **defaultValue** 字段。 
