---
title: 从 Azure 自动化更新管理中删除 VM
description: 本文介绍如何删除使用“更新管理”管理的计算机。
services: automation
ms.topic: conceptual
origin.date: 07/28/2020
ms.date: 08/10/2020
ms.custom: mvc
ms.openlocfilehash: d2503dc643833a9ed317dceb367ba73104846b68
ms.sourcegitcommit: e6b216b180734783219378410e13192e314a4497
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87790537"
---
# <a name="remove-vms-from-update-management"></a>从“更新管理”中删除 VM

完成对环境中 VM 的更新管理后，可以停止通过[更新管理](update-mgmt-overview.md)功能管理 VM。

## <a name="sign-into-the-azure-portal"></a>登录到 Azure 门户

登录到 [Azure 门户](https://portal.azure.cn)。

## <a name="to-remove-your-vms"></a>删除 VM

1. 从自动化帐户中，选择“更新管理”下的“更新管理”。

2. 使用以下命令识别希望从管理中删除的计算机的 UUID。

    ```azurecli
    az vm show -g MyResourceGroup -n MyVm -d
    ```

3. 在 Log Analytics 工作区的“常规”下，访问针对范围配置 `MicrosoftDefaultScopeConfig-Updates` 保存的搜索。

4. 对于保存的搜索 `MicrosoftDefaultComputerGroup`，单击右侧的省略号，然后选择“编辑”。

5. 删除 VM 的 UUID。

6. 对要删除的任何其他 VM 重复上述步骤。

7. 编辑完保存的搜索后，再次保存。

>[!NOTE]
>取消注册后，系统仍会显示这些计算机，因为我们会报告在过去 24 小时内评估的所有计算机。 断开计算机的连接后，需要等待 24 小时，系统才不会再次列出这些计算机。

## <a name="next-steps"></a>后续步骤

若要重新启用对虚拟机的管理，请参阅[从 Azure VM 启用更新管理](update-mgmt-enable-vm.md)。