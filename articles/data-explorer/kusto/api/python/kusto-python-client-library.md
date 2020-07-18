---
title: Azure 数据资源管理器 Python SDK - Azure 数据资源管理器
description: 本文介绍 Azure 数据资源管理器中的 Python SDK。
services: data-explorer
author: orspod
ms.author: v-tawe
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
origin.date: 10/30/2019
ms.date: 07/01/2020
ms.openlocfilehash: 7f6348a9b0f3bf3918b976c796254761af06e161
ms.sourcegitcommit: 9bc3e55f01e0999f05e7b4ebaea95f3ac91d32eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226337"
---
# <a name="azure-data-explorer-python-sdk"></a>Azure 数据资源管理器 Python SDK

Azure 数据资源管理器 Kusto Python 客户端库允许使用 Python 查询 Azure 数据资源管理器群集。 该库与 Python 2.x/3.x 兼容。 它通过 Python DB API 接口支持所有数据类型。

例如，可以从附加到 Spark 群集（包括但不限于 [Azure Databricks](https://azure.microsoft.com/services/databricks/) 实例）的 [Jupyter Notebook](https://jupyter.org/) 使用该库。

Kusto Python 引入客户端是一个 python 库，用于将引入数据发送到 Azure 数据资源管理器服务。

## <a name="next-steps"></a>后续步骤

* [如何安装包](https://github.com/Azure/azure-kusto-python#install)

* [Kusto 查询示例](https://github.com/Azure/azure-kusto-python/blob/master/azure-kusto-data/tests/sample.py)

* [数据引入示例](https://github.com/Azure/azure-kusto-python/blob/master/azure-kusto-ingest/tests/sample.py)

* [GitHub 存储库](https://github.com/Azure/azure-kusto-python)

    [![替换文字](https://travis-ci.org/Azure/azure-kusto-python.svg?branch=master "azure-kusto-python")](https://travis-ci.org/Azure/azure-kusto-python)

* Pypi 包：

    * [azure-kusto-data](https://pypi.org/project/azure-kusto-data/)
    [![PyPI 版本](https://badge.fury.io/py/azure-kusto-data.svg)](https://badge.fury.io/py/azure-kusto-data)
    * [azure-kusto-ingest](https://pypi.org/project/azure-kusto-ingest/)
    [![PyPI 版本](https://badge.fury.io/py/azure-kusto-ingest.svg)](https://badge.fury.io/py/azure-kusto-ingest)
    * [azure-mgmt-kusto](https://pypi.org/project/azure-mgmt-kusto/)
    [![PyPI 版本](https://badge.fury.io/py/azure-mgmt-kusto.svg)](https://badge.fury.io/py/azure-mgmt-kusto)
