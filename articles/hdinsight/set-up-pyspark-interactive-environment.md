---
title: Azure HDInsight 工具 - 为 Visual Studio Code 设置 PySpark 交互式环境 | Microsoft Docs
description: 了解如何使用用于 Visual Studio Code 的 Azure HDInsight 工具来创建、提交查询和脚本。
Keywords: VScode,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,交互式 Hive,交互式查询
services: HDInsight
documentationcenter: ''
author: jejiang
manager: ''
editor: ''
tags: azure-portal
ms.assetid: ''
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 06/13/2019
ms.date: 10/21/2019
ms.author: v-yiso
ms.openlocfilehash: 9389d0f8d2fef1cfbbff61c2cd205ded5fd12f12
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "72292548"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>为 Visual Studio Code 设置 PySpark 交互式环境

以下步骤显示如何在 VS Code 中设置 PySpark 交互环境。

我们使用 python/pip 命令在 Home 路径中生成虚拟环境  。 如果要使用其他版本，则需要手动更改默认版的 python/pip 命令  。 有关详细信息，请参阅[更新替代项](https://linux.die.net/man/8/update-alternatives)。

1. 安装 [Python](https://www.python.org/downloads/) 和 [pip](https://pip.pypa.io/en/stable/installing/).
   
   + 从 [https://www.python.org/downloads/](https://www.python.org/downloads/) 安装 Python。
   + 从 [https://pip.pypa.io/en/stable/installing](https://pip.pypa.io/en/stable/installing/) 安装 pip（如果未从“Python 安装”进行安装）。
   + 使用以下命令验证 Python 和 pip 是否成功安装。 (可选)
 
     ![Python pip 版本](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

     > [!NOTE]
     > 建议手动而不是使用 MacOS 默认版本安装 Python。


2. 通过运行以下命令，安装 virtualenv  。
   
   ```
   pip install virtualenv
   ```

## <a name="other-packages"></a>其他包

如果遇到错误消息，请通过运行以下命令安装所需的包：

   ![libkrb5 包](./media/set-up-pyspark-interactive-environment/install-libkrb5-package.png)

```
sudo apt-get install libkrb5-dev
```

   ```
   sudo apt-get install python-dev
   ```

4. 重新启动 VS Code 并回到运行“HDInsight: PySpark Interactive”的脚本编辑器。 

## <a name="next-steps"></a>后续步骤

### <a name="demo"></a>演示
* 用于 VS Code 的 HDInsight：[视频](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>工具和扩展
* [使用用于 Visual Studio Code 的 Azure HDInsight 工具](hdinsight-for-vscode.md)
* [使用 Azure Toolkit for IntelliJ 创建和提交 Apache Spark Scala 应用程序](spark/apache-spark-intellij-tool-plugin.md)
* [使用 Azure Toolkit for IntelliJ 通过 SSH 远程调试 Apache Spark 应用程序](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [使用 Azure Toolkit for IntelliJ 通过 VPN 远程调试 Apache Spark 应用程序](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [使用 Azure Toolkit for Eclipse 中的 HDInsight 工具创建 Apache Spark 应用程序](spark/apache-spark-eclipse-tool-plugin.md)
* [在 HDInsight 上的 Apache Spark 群集中使用 Apache Zeppelin 笔记本](spark/apache-spark-zeppelin-notebook.md)
* [在 HDInsight 的 Apache Spark 群集中可用于 Jupyter Notebook 的内核](spark/apache-spark-jupyter-notebook-kernels.md)
* [Use external packages with Jupyter notebooks（将外部包与 Jupyter 笔记本配合使用）](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Install Jupyter on your computer and connect to an HDInsight Spark cluster（在计算机上安装 Jupyter 并连接到 HDInsight Spark 群集）](spark/apache-spark-jupyter-notebook-install-locally.md)
* [在 Azure HDInsight 中使用 Microsoft Power BI 直观显示 Apache Hive 数据](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [在 Azure HDInsight 中使用 Apache Zeppelin 运行 Apache Hive 查询](./interactive-query/hdinsight-connect-hive-zeppelin.md)
