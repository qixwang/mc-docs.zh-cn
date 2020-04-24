---
author: rockboyfor
ms.service: azure-resource-manager
ms.topic: include
origin.date: 11/25/2018
ms.date: 01/21/2019
ms.author: v-yeche
ms.openlocfilehash: cc1855054b0b9d06455651e382e4ff94ae2587a9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "63827685"
---
1. 在要锁定的资源、资源组或订阅的“设置”边栏选项卡中，选择“锁定”  。

      ![选择锁](./media/resource-manager-lock-resources/select-lock.png)
2. 若要添加锁，请选择“添加”  。 如果要在父级别创建锁，请选择父级。 当前选定的资源将从父级继承锁。 例如，可以锁定资源组，以便向其所有资源应用锁。

      ![添加锁](./media/resource-manager-lock-resources/add-lock.png) 
3. 为该锁提供名称和锁级别。 （可选）可以添加注释来描述该锁。

      ![设置锁](./media/resource-manager-lock-resources/set-lock.png) 
4. 若要删除锁，请从可用选项中选择省略号中的“删除”  。

      ![删除锁](./media/resource-manager-lock-resources/delete-lock.png)

<!-- Update_Description: wording update -->