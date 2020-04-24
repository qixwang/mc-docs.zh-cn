---
title: include 文件
description: include 文件
services: storage
author: WenJason
ms.service: storage
ms.topic: include
origin.date: 12/26/2019
ms.date: 03/09/2020
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 7997f9c35b62a1eec41e075bed728adfcaec1156
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78411250"
---
Azure 文件共享将部署到存储帐户。存储帐户是代表存储共享池的顶级对象。  此存储池可用于部署多个文件共享和其他存储资源，例如 Blob 容器、队列或表。 部署到存储帐户中的所有存储资源共享应用于该存储帐户的限制。 若要查看存储帐户的当前限制，请参阅 [Azure 文件存储的可伸缩性和性能目标](../articles/storage/files/storage-files-scale-targets.md)。

对于 Azure 文件存储部署，将使用主要类型的存储帐户：**常规用途版本 2 (GPv2) 存储帐户**：使用 GPv2 存储帐户可以在标准的/基于硬盘（基于 HDD）的硬件上部署 Azure 文件共享。 除了存储 Azure 文件共享以外，GPv2 存储帐户还可以存储其他存储资源，例如 Blob 容器、队列或表。 

在 Azure 门户、PowerShell 或 CLI 中，可能会遇到一些其他的存储帐户类型。 一个存储帐户类型（BlobStorage 存储帐户）不能包含 Azure 文件共享。 可能会看到的其他两个存储帐户类型为常规用途版本 1 (GPv1) 和经典存储帐户，二者都可以包含 Azure 文件共享。 尽管 GPv1 和经典存储帐户可以包含 Azure 文件共享，但 Azure 文件存储的大多数新功能只能在 GPv2 和 FileStorage 存储帐户中使用。 因此，我们建议仅将 GPv2 存储帐户用于新部署，而升级 GPv1 和经典存储帐户的前提是它们已经存在于环境中。  