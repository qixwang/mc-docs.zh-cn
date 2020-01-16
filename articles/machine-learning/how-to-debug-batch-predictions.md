---
title: 对 ParallelRunStep 进行调试和故障排除
titleSuffix: Azure Machine Learning
description: 通过适用于 Python 的 Azure 机器学习 SDK 对机器学习管道进行调试和故障排除。 了解通过管道进行开发的常见缺陷，以及有助于在远程执行之前和期间调试你的脚本的技巧。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye, jmartens, larryfr, vaidyas
ms.author: trmccorm
author: tmccrmck
ms.date: 11/21/2019
ms.openlocfilehash: 1813b9b07969e4c117ebdb19208688303d7a6995
ms.sourcegitcommit: 623d64ef33e80d5f84b6dcf6d1ef4120fe4b8c08
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75599608"
---
# <a name="debug-and-troubleshoot-using-parallelrun"></a>使用 ParallelRun 进行调试和故障排除
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

本文介绍了如何通过 [Azure 机器学习 SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)对 [ParallelRunStep](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallel_run_step.parallelrunstep?view=azure-ml-py) 类进行调试和故障排除。

## <a name="testing-scripts-locally"></a>在本地测试脚本

有关机器学习管道，请参阅[“在本地测试脚本”部分](how-to-debug-pipelines.md#testing-scripts-locally)。 ParallelRunStep 作为机器学习管道中的一个步骤运行，因此，同一答案同时适用于两者。

## <a name="debugging-scripts-from-remote-context"></a>从远程上下文调试脚本

从在本地调试评分脚本转变为在实际管道中调试评分脚本可能是一个困难的飞跃。 若要了解如何在门户中查找日志，请参阅[有关从远程上下文调试脚本的机器学习管道部分](how-to-debug-pipelines.md#debugging-scripts-from-remote-context)。 该部分中的信息也适用于批量推理运行。

例如，日志文件 `70_driver_log.txt` 还包含： 

* 脚本执行过程中输出的所有语句。
* 脚本的堆栈跟踪。

由于批量推理作业是分布式的，因此会存在来自多个不同来源的日志。 但是，会创建两个合并的文件来提供概要信息：

- `~/logs/overview.txt`：此文件提供目前为止已创建的微型批处理（也称为任务）数目和目前为止已处理的微型批处理数目的概要信息。 在该文件的末尾，它显示作业的结果。 如果作业失败，它将显示错误消息以及从何处开始排除故障。

- `~/logs/master.txt`：此文件提供运行中作业的主节点（也称为业务流程协调程序）视图。 包括任务创建、进度监视和运行结果。

当需要全面了解每个节点如何执行了评分脚本时，请查看每个节点各自的进程日志。 可在 `worker` 文件夹中找到进程日志，它们按工作器节点分组：

- `~/logs/worker/<ip_address>/Process-*.txt`：此文件提供有关每个微型批处理在工作器拾取或完成它时的详细信息。 对于每个微型批处理，此文件包括：

    - 工作进程的 IP 地址和 PID。 
    - 总项数和已成功处理的项数。 
    - 按时钟时间计量的开始时间和结束时间（`start1` 和 `end1`）。 
    - 按花费的处理器时间计量的开始时间和结束时间（`start2` 和 `end2`）。 

你还可以找到每个工作器进程的资源使用情况的相关信息。 此信息采用 CSV 格式，位于 `~/logs/performance/<ip_address>/`。 例如，检查资源利用率时，请查看以下文件：

- `process_resource_monitor_<ip>_<pid>.csv`：每个工作进程的资源使用情况。 
- `sys_resource_monitor_<ip>.csv`：每个节点的日志。

### <a name="how-do-i-log-from-my-user-script-from-a-remote-context"></a>如何从远程上下文通过我的用户脚本记录日志？
你可以使用以下步骤设置记录器，以使日志显示在门户中的  “日志/用户”文件夹中：
1. 将下面的第一个代码部分保存到文件 entry_script_helper py 中，并将该文件放在与入口脚本相同的文件夹中。 此类获取 AmlCompute 内的路径。 对于本地测试，你可以更改 `get_working_dir()` 来返回一个本地文件夹。
2. 在 `init()` 方法中配置一个记录器，然后使用它。 下面的第二个代码部分是一个示例。 

**entry_script_helper.py：**
```python
"""
This module provides helper features for entry script.
This file should be in Python search paths or in the same folder as the entry script.
"""
import os
import socket
import logging
import time
from multiprocessing import current_process
from azureml.core import Run


class EntryScriptHelper:
    """ A helper to provide common features for entry script."""

    LOG_CONFIGED = False

    def get_logger(self, name="EntryScript"):
        """ Return a logger.
            The logger will write to the 'users' folder and show up in azure portal.
        """
        return logging.getLogger(name)

    def config(self, name="EntryScript", level="INFO"):
        """ Config a logger. This should be called in init() in score module.
            Config the logger one time if not configed.
            The logger will write to the 'users' folder and show up in azure portal.
        """
        logger = logging.getLogger(name)

        formatter = logging.Formatter(
            "%(asctime)s|%(name)s|%(levelname)s|%(process)d|%(thread)d|%(funcName)s()|%(message)s"
        )
        formatter.converter = time.gmtime

        logger.setLevel(level)

        handler = logging.FileHandler(self.get_log_file_path())
        handler.setLevel(level)
        handler.setFormatter(formatter)
        logger.addHandler(handler)

        return logger

    def get_log_file_path(self):
        """ Get the log file path for users.
            Each process has its own log file, so there is not race issue among multiple processes.
        """
        ip_address = socket.gethostbyname(socket.gethostname())
        log_dir = os.path.join(self.get_log_dir(), "user", ip_address)
        os.makedirs(log_dir, exist_ok=True)
        return os.path.join(log_dir, f"{current_process().name}.txt")

    def get_log_dir(self):
        """ Return the folder for logs.
            Files and folders in it will be uploaded and show up in run detail page in the azure portal.
        """
        log_dir = os.path.join(self.get_working_dir(), "logs")
        os.makedirs(log_dir, exist_ok=True)
        return log_dir

    def get_working_dir(self):
        """ Return the working directory."""
        return os.path.join(os.environ.get("AZ_BATCHAI_INPUT_AZUREML", ""), self.get_run().id)

    def get_temp_dir(self):
        """ Return local temp directory."""
        local_temp_dir = os.path.join(
            os.environ.get("AZ_BATCHAI_JOB_TEMP", ""), "azureml-bi", str(os.getpid())
        )
        os.makedirs(local_temp_dir, exist_ok=True)
        return local_temp_dir

    def get_run(self):
        """ Return the Run from the context."""
        return Run.get_context(allow_offline=False)

```

**使用了记录器的一个示例入口脚本：**
```python
"""
This is a sample scoring module.

This module provides a sample which passes the input back without any change.
"""
import os
import logging
from entry_script_helper import EntryScriptHelper

LOG_NAME = "score_file_list"


def init():
    """ Init """
    EntryScriptHelper().config(LOG_NAME)
    logger = logging.getLogger(LOG_NAME)
    output_folder = os.path.join(os.environ.get("AZ_BATCHAI_INPUT_AZUREML", ""), "temp/output")
    logger.info(f"{__file__}.output_folder:{output_folder}")
    logger.info("init()")
    os.makedirs(output_folder, exist_ok=True)


def run(mini_batch):
    """ Accept and return the list back."""
    logger = logging.getLogger(LOG_NAME)
    logger.info(f"{__file__}, run({mini_batch})")

    output_folder = os.path.join(os.environ.get("AZ_BATCHAI_INPUT_AZUREML", ""), "temp/output")
    for file_name in mini_batch:
        with open(file_name, "r") as file:
            lines = file.readlines()
        base_name = os.path.basename(file_name)
        name = os.path.join(output_folder, base_name)
        logger.info(f"{__file__}: {name}")
        with open(name, "w") as file:
            file.write(f"ouput file {name} from {__file__}:\n")
            for line in lines:
                file.write(line)

    return mini_batch
```

## <a name="next-steps"></a>后续步骤

* 参阅 SDK 参考来获得 [azureml-contrib-pipeline-step](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps?view=azure-ml-py) 包的帮助信息，参阅此[文档](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallelrunstep?view=azure-ml-py)来了解 ParallelRunStep 类。

* 按照有关使用管道进行批量评分的[高级教程](tutorial-pipeline-batch-scoring-classification.md)进行操作。
