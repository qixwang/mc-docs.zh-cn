---
title: 使用用于 Visual Studio 的 Data Lake (Hadoop) 工具运行 Hive 查询 - Azure HDInsight | Azure
description: 了解如何使用用于 Visual Studio 的 Data Lake 工具对 Azure HDInsight 上的 Apache Hadoop 运行 Apache Hive 查询。
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 2b3e672a-1195-4fa5-afb7-b7b73937bfbe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 11/27/2019
ms.date: 01/13/2020
ms.author: v-yiso
ms.openlocfilehash: 9a337c4e5571499397c6c2d33f865cd05876f679
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75630854"
---
# <a name="run-apache-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>使用针对 Visual Studio 的 Data Lake 工具运行 Apache Hive 查询

了解如何使用用于 Visual Studio 的 Data Lake 工具查询 Apache Hive。 使用 Data Lake 工具，可以轻松创建 Hive 查询，将其提交到 Azure HDInsight 上的 Apache Hadoop 并进行监视。

## <a name="prerequisites"></a>先决条件

* HDInsight 中的 Apache Hadoop 群集。 有关创建此项的信息，请参阅[使用资源管理器模板在 Azure HDInsight 中创建 Apache Hadoop 群集](./apache-hadoop-linux-tutorial-get-started.md)。

* [Visual Studio](https://visualstudio.microsoft.com/vs/)。 本文中的步骤使用 Visual Studio 2019。

* 用于 Visual Studio 的 HDInsight 工具或用于 Visual Studio 的 Azure Data Lake 工具。 有关安装和配置工具的信息，请参阅[安装适用于 Visual Studio 的 Data Lake 工具](apache-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio)。

## <a name="run-apache-hive-queries-using-the-visual-studio"></a>使用 Visual Studio 运行 Apache Hive 查询

可以使用两个选项来创建并运行 Hive 查询：

* 创建临时查询。
* 创建 Hive 应用程序。

### <a name="create-an-ad-hoc-hive-query"></a>创建临时 Hive 查询。

即席查询可以**批处理**或**交互式**模式执行。

1. 启动 **Visual Studio** 并选择“继续但无需代码”  。

2. 在服务器资源管理器中右键单击“Azure”并选择“连接到 Microsoft Azure 订阅...”，然后完成登录过程。   

3. 展开“HDInsight”，右键单击要运行查询的群集，然后选择“编写 Hive 查询”   。

4. 输入以下 Hive 查询：

    ```hql
    SELECT * FROM hivesampletable;
    ```

5. 选择“执行”  。 执行模式默认为“交互式”。 

    ![执行交互式 Hive 查询，Visual Studio](./media/apache-hadoop-use-hive-visual-studio/vs-execute-hive-query.png)

6. 若要以**批处理**模式下运行同一查询，请将下拉列表从“交互式”  切换到“批处理”  。 执行按钮将从“执行”  更改为“提交”  。

    ![提交批处理 Hive 查询，Visual Studio](./media/apache-hadoop-use-hive-visual-studio/visual-studio-batch-query.png)

    Hive 编辑器支持 IntelliSense。 用于 Visual Studio 的 Data Lake 工具支持在编辑 Hive 脚本时加载远程元数据。 例如，如果键入 `SELECT * FROM`，则 IntelliSense 会列出所有建议的表名称。 在指定表名称后，IntelliSense 会列出列名称。 这些工具支持大多数 Hive DML 语句、子查询和内置 UDF。 IntelliSense 只建议 HDInsight 工具栏中所选群集的元数据。

7. 在查询工具栏（查询选项卡下面以及查询文本上面的区域）中，选择“提交”，或者选择“提交”旁边的下拉箭头并从下拉列表中选择“高级”。    如果选择后一个选项，

8. 即选择高级提交选项，请在“提交脚本”对话框中配置“作业名称”、“参数”、“其他配置”和“状态目录”。      然后选择“提交”。 

    ![“提交脚本”对话框，HDInsight Hadoop Hive 查询](./media/apache-hadoop-use-hive-visual-studio/vs-tools-submit-jobs-advanced.png)

### <a name="create-a-hive-application"></a>创建 Hive 应用程序

若要通过创建 Hive 应用程序来运行 Hive 查询，请执行以下步骤：

1. 打开 **Visual Studio**。

2. 在“开始”窗口中，选择“创建新项目”。  

3. 在“创建新项目”窗口中的“搜索模板”框内，输入 *Hive*。   然后依次选择“Hive 应用程序”、“下一步”。  

4. 在“配置新项目”窗口中输入一个**项目名称**，选择或创建新项目的**位置**，然后选择“创建”。  

5. 打开在创建此项目时产生的 **Script.hql** 文件，并在其中粘贴以下 HiveQL 语句：

    ```hql
    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    这些语句执行以下操作：

    * `DROP TABLE`：如果表存在，将删除该表。

    * `CREATE EXTERNAL TABLE`：在 Hive 中创建一个新的“外部”表。 外部表只会在 Hive 中存储表定义。 （数据保留在原始位置。）

        > [!NOTE]  
        > 如果你预期基础数据会由外部源（例如 MapReduce 作业或 Azure 服务）更新，则应使用外部表。
        >
        > 删除外部表 **不会** 删除数据，只会删除表定义。
    
    * `ROW FORMAT`：让 Hive 知道数据的格式已如何进行了设置。 在此情况下，每个日志中的字段以空格分隔。

    * `STORED AS TEXTFILE LOCATION`：告知 Hive 数据已以文本形式存储在 *example/data* 目录中。

    * `SELECT`：选择 `t4` 列包含值 `[ERROR]` 的所有行计数。 此语句会返回值 `3`，因为有三个行包含此值。

    * `INPUT__FILE__NAME LIKE '%.log'`：告知 Hive 只会返回以 .log 结尾的文件中的数据。 此子句将搜索限定为包含数据的 *sample.log* 文件。

6. 在查询文件工具栏（其外观与临时查询工具栏类似）中，选择要用于此查询的 HDInsight 群集。 然后，将“交互式”更改为“批处理”（如果需要），并选择“提交”以 Hive 作业形式运行语句。   

   “Hive 作业摘要”  将会出现并显示有关正在运行的作业的信息。 在“作业状态”  更改为“已完成”  之前，使用“刷新”  链接刷新作业信息。

   ![已完成 Hive 作业摘要，Hive 应用程序，Visual Studio](./media/apache-hadoop-use-hive-visual-studio/hdinsight-job-summary.png)

7. 选择“作业输出”查看此作业的输出。  `[ERROR] 3`，这是此查询返回的值。

### <a name="additional-example"></a>其他示例

以下示例依赖于在前一过程[创建 Hive 应用程序](#create-a-hive-application)中创建的 `log4jLogs` 表。

1. 在“服务器资源管理器”  中，右键单击群集，然后选择“编写 Hive 查询”  。

2. 输入以下 Hive 查询：

    ```hql
    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    这些语句执行以下操作：

    * `CREATE TABLE IF NOT EXISTS`：如果表尚不存在，将创建表。 由于未使用 `EXTERNAL` 关键字，因此此语句会创建内部表。 内部表存储在 Hive 数据仓库中，并由 Hive 托管。

        > [!NOTE]  
        > 与 `EXTERNAL` 表不同，删除内部表会同时删除基础数据。

    * `STORED AS ORC`：以优化的行纵栏式 (ORC) 格式存储数据。  ORC 是高度优化且有效的 Hive 数据存储格式。
    
    * `INSERT OVERWRITE ... SELECT`：从包含 `[ERROR]` 的 `log4jLogs` 表中选择行，然后将数据插入 `errorLogs` 表中。

3. 根据需要将“交互式”更改为“批处理”，然后选择“提交”。   

4. 若要验证该作业是否已创建表，请转到“服务器资源管理器”并展开“Azure” > “HDInsight”。    展开 HDInsight 群集，然后展开“Hive 数据库” > “默认”。   此时会列出 **errorLogs** 表和 **log4jLogs** 表。

## <a name="next-steps"></a>后续步骤

如你所见，用于 Visual Studio 的 HDInsight 工具提供了在 HDInsight 上使用 Hive 查询的轻松方式。

有关 HDInsight 中的 Hive 的一般信息：

* [什么是 Azure HDInsight 中的 Apache Hive 和 HiveQL？](hdinsight-use-hive.md)

有关 HDInsight 上 Hadoop 的其他使用方法的信息：

* [在 Apache Hadoop on HDInsight 中使用 MapReduce](hdinsight-use-mapreduce.md)

有关适用于 Visual Studio 的 HDInsight 工具的详细信息：

* [使用适用于 Visual Studio 的 Data Lake 工具连接到 Azure HDInsight 并运行 Apache Hive 查询](apache-hadoop-visual-studio-tools-get-started.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: https://www.azure.cn/pricing/overview/
[azure-member-offers]: https://www.azure.cn/pricing/member-offers/
[azure-trial]: https://www.azure.cn/pricing/1rmb-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: apache-hadoop-connect-excel-power-query.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md

[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png

<!--Update_Description: update meta data-->