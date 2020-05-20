---
title: 使用 HDInsight .NET SDK 提交 MapReduce 作业 - Azure
description: 了解如何使用 HDInsight .NET SDK 将 MapReduce 作业提交到 Azure HDInsight Apache Hadoop。
ms.reviewer: jasonh
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: mumian
ms.assetid: c85e44b0-85fd-4185-ad1c-c34a9fe5ef44
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
origin.date: 01/15/2020
ms.date: 03/02/2020
ms.author: v-yiso
ms.openlocfilehash: 42cd262398a5ef6d4eaab84dcb6280551d3c6569
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77563503"
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>使用 HDInsight .NET SDK 运行 MapReduce 作业
[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

了解如何使用 HDInsight .NET SDK 提交 MapReduce 作业。 HDInsight 群集附带含有一些 MapReduce 示例的 jar 文件。 该 jar 文件位于 /example/jars/hadoop-mapreduce-examples.jar  。  其中一个示例是 wordcount  。 开发 C# 控制台应用程序，提交 wordcount 作业。  作业会读取 /example/data/gutenberg/davinci.txt  文件，并将结果输出到 /example/data/davinciwordcount  。  如果想要重新运行该应用程序，必须清理输出文件夹。

> [!NOTE]
> 必须从 Windows 客户端执行本文中的步骤。 有关使用 Linux、OS X 或 Unix 客户端来使用 Hive 的信息，请使用本文顶部显示的选项卡选择器。
> 
> 

## <a name="prerequisites"></a>必备条件

* HDInsight 中的 Apache Hadoop 群集。 请参阅[使用 Azure 门户创建 Apache Hadoop 群集](../hdinsight-hadoop-create-linux-clusters-portal.md)。

* [Visual Studio](https://visualstudio.microsoft.com/vs/community/)。

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>使用 HDInsight .NET SDK 提交 MapReduce 作业
HDInsight .NET SDK 提供 .NET 客户端库，可简化从 .NET 中使用 HDInsight 群集的操作。 

1. 启动 Visual Studio 并创建 C# 控制台应用程序。

1. 导航到“工具” > “NuGet 包管理器” > “包管理器控制台”，然后输入以下命令：

    ```   
    Install-Package Microsoft.Azure.Management.HDInsight.Job
    ```

1. 将以下代码复制到 **Program.cs** 中。 然后通过设置 `existingClusterName`、`existingClusterPassword`、`defaultStorageAccountName`、`defaultStorageAccountKey` 和 `defaultStorageContainerName` 的值来编辑代码。

    ```csharp
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;

                private const string existingClusterName = "<Your HDInsight Cluster Name>";
                private const string existingClusterUri = existingClusterName + ".azurehdinsight.cn";
                private const string existingClusterUsername = "<Cluster Username>";
                private const string existingClusterPassword = "<Cluster User Password>";

                private const string defaultStorageAccountName = "<Default Storage Account Name>"; //<StorageAccountName>.blob.core.chinacloudapi.cn
                private const string StorageAccountSuffix = "core.chinacloudapi.cn";
                private const string defaultStorageAccountKey = "<Default Storage Account Key>";
                private const string defaultStorageContainerName = "<Default Blob Container Name>";

                private const string sourceFile = "/example/data/gutenberg/davinci.txt";  
                private const string outputFolder = "/example/data/davinciwordcount";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = existingClusterUsername, Password = existingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(existingClusterUri, clusterCredentials);

                    SubmitMRJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitMRJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };

                    var paras = new MapReduceJobSubmissionParameters
                    {
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the MR job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitMapReduceJob(paras);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    System.Console.WriteLine("Job output is: ");
                    var storageAccess = new AzureStorageAccess(defaultStorageAccountName, defaultStorageAccountKey,
                        defaultStorageContainerName, StorageAccountSuffix);

                    if (jobDetail.ExitValue == 0)
                    {
                        // Create the storage account object
                        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=" + 
                            defaultStorageAccountName + 
                            ";AccountKey=" + defaultStorageAccountKey + "; Endsuffix=" + StorageAccountSuffix);

                        // Create the blob client.
                        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

                        // Retrieve reference to a previously created container.
                        CloudBlobContainer container = blobClient.GetContainerReference(defaultStorageContainerName);

                        CloudBlockBlob blockBlob = container.GetBlockBlobReference(outputFolder.Substring(1) + "/part-r-00000");

                        using (var stream = blockBlob.OpenRead())
                        {
                            using (StreamReader reader = new StreamReader(stream))
                            {
                                while (!reader.EndOfStream)
                                {
                                    System.Console.WriteLine(reader.ReadLine());
                                }
                            }
                        }
                    }
                    else
                    {
                        // fetch stderr output in case of failure
                        var output = _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); 

                        using (var reader = new StreamReader(output, Encoding.UTF8))
                        {
                            string value = reader.ReadToEnd();
                            System.Console.WriteLine(value);
                        }

                    }
                }
            }
        }
    ```
4. 按 **F5** 运行应用程序。

若要重新运行该作业，必须更改作业输出文件夹的名称，在此示例中，它是“/example/data/davinciwordcount”。

作业成功完成后，应用程序将打印输出文件的内容“part-r-00000”。

## <a name="next-steps"></a>后续步骤
在本文中，已经学习了几种创建 HDInsight 群集的方法。 若要了解详细信息，请参阅以下文章：

* 有关如何提交 Hive 作业，请参阅[使用 HDInsight .NET SDK 运行 Apache Hive 查询](apache-hadoop-use-hive-dotnet-sdk.md)。
* 有关如何创建 HDInsight 群集，请参阅[在 HDInsight 中创建基于 Linux 的 Apache Hadoop 群集](../hdinsight-hadoop-provision-linux-clusters.md)。
* 有关如何管理 HDInsight 群集，请参阅[在 HDInsight 中管理 Apache Hadoop 群集](../hdinsight-administer-use-portal-linux.md)。
* 请参阅 [HDInsight.NET SDK 参考资料](https://docs.azure.cn/zh-cn/dotnet/api/overview/hdinsight)，学习 HDInsight.NET SDK。
* 请参阅[创建非交互式身份验证 .NET HDInsight 应用程序](../hdinsight-create-non-interactive-authentication-dotnet-applications.md)，了解对 Azure 的非交互式身份验证。

