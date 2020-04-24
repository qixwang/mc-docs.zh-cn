---
author: rockboyfor
ms.service: ansible
ms.topic: include
origin.date: 04/22/2019
ms.date: 05/20/2019
ms.author: v-yeche
ms.openlocfilehash: cbdda6d6ddbac228567e3c48f823e5c574ba3cd9
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67457234"
---
<!--Verify successfully-->
1. 在 Azure 本地 Shell 中，创建名为 `rg.yml` 的文件。

    ```bash
    code rg.yml
    ```

1. 在编辑器中粘贴以下代码：

    ```yaml
    ---
    - hosts: localhost
     connection: local
     tasks:
       - name: Create resource group
         azure_rm_resourcegroup:
           name: ansible-rg
           location: chinaeast
         register: rg
       - debug:
           var: rg
    ```

1. 保存文件并退出编辑器。

1. 使用 `ansible-playbook` 命令运行 playbook：

    ```bash
    ansible-playbook rg.yml
    ```

运行 playbook 后，可看到类似于以下结果的输出：

```output
PLAY [localhost] *********************************************************************************

TASK [Gathering Facts] ***************************************************************************
ok: [localhost]

TASK [Create resource group] *********************************************************************
changed: [localhost]

TASK [debug] *************************************************************************************
ok: [localhost] => {
    "rg": {
        "changed": true,
        "contains_resources": false,
        "failed": false,
        "state": {
            "id": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/ansible-rg",
            "location": "chinaeast",
            "name": "ansible-rg",
            "provisioning_state": "Succeeded",
            "tags": null
        }
    }
}

PLAY RECAP ***************************************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0
```

<!-- Update_Description: wording update -->

