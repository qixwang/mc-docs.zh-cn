---
author: IEvangelist
ms.author: v-tawe
origin.date: 06/25/2019
ms.date: 01/13/2020
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: ddf7f43e1458aceb5a7eab23465b1ff61037b0b9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75631698"
---
### <a name="run-multiple-containers-on-the-same-host"></a>在同一主机上运行多个容器

若要使用公开端口运行多个容器，请确保在运行每个容器时使用不同的公开端口。 例如，在端口 5000 上运行第一个容器，在端口 5001 上运行第二个容器。

可以让此容器和其他 Azure 认知服务容器一起运行在该主机上。 此外，还可以让同一认知服务容器的多个容器一起运行。
