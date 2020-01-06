---
title: 将 MapReduce 与 Hadoop on HDInsight 配合使用 | Azure
description: 学习如何在 HDInsight 群集中的 Hadoop 上运行 MapReduce 作业。
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7f321501-d62c-4ffc-b5d6-102ecba6dd76
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
origin.date: 12/06/2019
ms.date: 01/13/2020
ms.author: v-yiso
ms.openlocfilehash: a0b9ab9774e43f5b97211abbc0f103c194a2b514
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75630848"
---
# <a name="use-mapreduce-in-apache-hadoop-on-hdinsight"></a>在 Apache Hadoop on HDInsight 中使用 MapReduce

了解如何在 HDInsight 群集上运行 MapReduce 作业。

## <a id="data"></a>示例数据

HDInsight 提供存储在 `/example/data` 和 `/HdiSamples` 目录中的各种示例数据集。 这些目录位于群集的默认存储中。 在本文档中，我们使用 `/example/data/gutenberg/davinci.txt` 文件。 此文件包含 Leonardo Da Vinci 的笔记本。

## <a id="job"></a>MapReduce 示例

MapReduce 单词计数应用程序示例包含在 HDInsight 群集中。 此示例位于群集默认存储的 `/example/jars/hadoop-mapreduce-examples.jar` 中。

以下 Java 代码是包含在 `hadoop-mapreduce-examples.jar` 文件中的 MapReduce 应用程序的源代码：

```java
package org.apache.hadoop.examples;

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

    public static class TokenizerMapper
        extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
        StringTokenizer itr = new StringTokenizer(value.toString());
        while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
    }
    }

    public static class IntSumReducer
        extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                        Context context
                        ) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
        sum += val.get();
        }
        result.set(sum);
        context.write(key, result);
    }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
        System.err.println("Usage: wordcount <in> <out>");
        System.exit(2);
    }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

有关编写自己的 MapReduce 应用程序的说明，请参阅以下文档：

* [为 HDInsight 开发 Java MapReduce 应用程序](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

## <a id="run"></a>运行 MapReduce

HDInsight 可以使用各种方法运行 HiveQL 作业。 使用下表来确定哪种方法最适合用户，并访问此链接进行演练。

| **使用此方法**... | **...实现此目的** |  ...从此 **客户端操作系统** |
|:--- |:--- |:--- |:--- |
| [SSH](apache-hadoop-use-mapreduce-ssh.md) |通过 **SSH** |Linux、Unix、Mac OS X 或 Windows |
| [Curl](apache-hadoop-use-mapreduce-curl.md) |使用 **REST** |Linux、Unix、Mac OS X 或 Windows |
| [Windows PowerShell](apache-hadoop-use-mapreduce-powershell.md) |使用 **Windows PowerShell**  |Windows |

## <a id="nextsteps"></a>后续步骤

若要了解如何使用 HDInsight 中的数据的详细信息，请参阅以下文档：

* [为 HDInsight 开发 Java MapReduce 程序](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

* [将 Apache Hive 和 HDInsight 配合使用][hdinsight-use-hive]




[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]:../hdinsight-use-hive.md
[hdinsight-use-pig]:hdinsight-use-pig.md

[powershell-install-configure]: https://docs.microsoft.com/powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif

<!--Update_Description: update meta data-->