---
title: 管理 Azure 自动化中的 Python 2 程序包
description: 本文介绍了如何管理 Azure 自动化中的 Python 2 程序包。
services: automation
ms.subservice: process-automation
origin.date: 02/25/2019
ms.date: 05/25/2020
ms.topic: conceptual
ms.openlocfilehash: d983b54c63a04731cf9db6b52c0fe698fb576340
ms.sourcegitcommit: 981a75a78f8cf74ab5a76f9e6b0dc5978387be4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2020
ms.locfileid: "83801294"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>管理 Azure 自动化中的 Python 2 程序包

通过 Azure 自动化，可以在 Azure 和 Linux 混合 Runbook 辅助角色上运行 Python 2 runbook。 为了帮助简化 runbook，可以使用 Python 程序包导入所需的模块。 本文介绍了如何在 Azure 自动化中管理和使用 Python 程序包。

## <a name="import-packages"></a>导入程序包

在自动化帐户中的“共享资源”下选择“Python 2 程序包”。 单击“+ 添加 Python 2 程序包”。

![添加 Python 程序包](media/python-packages/add-python-package.png)

在“添加 Python 2 程序包”页面上，选择要上传的本地程序包。 程序包可以是 **.whl** 或 **.tar.gz** 文件。 在选择程序包后，单击“确定”以上传程序包。

![添加 Python 程序包](media/python-packages/upload-package.png)

导入程序包之后，该程序包将在自动化帐户的“Python 2 程序包”页中列出。 如果需要删除某个程序包，请选择该程序包并单击“删除”。

![程序包列表](media/python-packages/package-list.png)

## <a name="import-packages-with-dependencies"></a>导入带依赖项的包

Azure 自动化不在导入过程中解析 Python 包的依赖项。 可以通过两种方式导入包及其所有依赖项。 只需使用下述步骤之一将包导入自动化帐户中。

### <a name="manually-download"></a>手动下载

在安装了 [Python2.7](https://www.python.org/downloads/release/latest/python2) 和 [pip](https://pip.pypa.io/en/stable/) 的 Windows 64 位计算机上运行以下命令，以便下载包及其所有依赖项：

```cmd
C:\Python27\Scripts\pip2.7.exe download -d <output dir> <package name>
```

等到这些包下载以后，即可将其导入自动化帐户中。

### <a name="runbook"></a>Runbook

 若要获取 runbook，请按 [import Python 2 packages from pypi into Azure Automation account](https://gallery.technet.microsoft.com/scriptcenter/Import-Python-2-packages-57f7d509)（将 Python 2 程序包从 pypi 导入到 Azure 自动化帐户中）中的要求操作，将其从库导入到你的自动化帐户中。 请确保将“运行设置”设定为 **Azure** 并使用参数启动 runbook。 Runbook 需要一个运行方式帐户，否则自动化帐户无法运行。 对于每个参数，请确保使用以下列表和图像中所示的开关来启动它：

* -s \<subscriptionId\>
* -g \<resourceGroup\>
* -a \<automationAccount\>
* -m \<modulePackage\>

![程序包列表](media/python-packages/import-python-runbook.png)

Runbook 允许你指定要下载的程序包。 例如，使用 `Azure` 参数会下载所有 Azure 模块和所有依赖项（大约 105 个）。

完成 runbook 后，可以在自动化帐户中检查“共享资源”下的“Python 2 程序包”，以便验证是否已正确导入程序包。

## <a name="use-a-package-in-a-runbook"></a>在 runbook 中使用程序包

导入程序包后，可以在 runbook 中使用它。 下面的示例使用了 [ Azure 自动化实用程序包](https://github.com/WenJason/azure_automation_utility)。 有了此程序包，可以更轻松地通过 Azure 自动化使用 Python。 若要使用此程序包，请按照 GitHub 存储库中的说明进行操作，并将其添加到 runbook。 例如，可以使用 `from azure_automation_utility import get_automation_runas_credential` 导入用于检索运行方式帐户的函数。

```python
import azure.mgmt.resource
import automationassets
from azure_automation_utility import get_automation_runas_credential

# Authenticate to Azure using the Azure Automation RunAs service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential()

# Intialize the resource management client with the RunAs credential and subscription
resource_client = azure.mgmt.resource.ResourceManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"]),
    "2017-05-10",
    "https://management.chinacloudapi.cn")

# Get list of resource groups and print them out
groups = resource_client.resource_groups.list()
for group in groups:
    print group.name
```

## <a name="develop-and-test-runbooks-offline"></a>脱机开发和测试 runbook

若要脱机开发和测试 Python 2 runbook，可以使用 GitHub 上的 [Azure 自动化 Python 模拟资产](https://github.com/WenJason/python_emulated_assets)模块。 此模块允许你引用共享资源，例如凭据、变量、连接和证书。

## <a name="next-steps"></a>后续步骤

若要开始使用 Python 2 runbook，请参阅[我的第一个 Python 2 runbook](automation-first-runbook-textual-python2.md)。