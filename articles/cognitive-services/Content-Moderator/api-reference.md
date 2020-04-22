---
title: API 参考 - 内容审查器
titlesuffix: Azure Cognitive Services
description: 了解多种内容审查器的内容审核和评审 API。
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: reference
origin.date: 05/29/2019
ms.date: 07/10/2019
ms.author: v-junlch
ms.openlocfilehash: 0883b3daa9414602e0cc96176b8ae29153a847de
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "67844943"
---
# <a name="content-moderator-api-reference"></a>内容审查器 API 参考

可以从 Azure 门户中的 Azure 内容审查器 API 着手：[订阅内容审查器 API](https://portal.azure.cn/#create/Microsoft.CognitiveServicesContentModerator)。

## <a name="moderation-apis"></a>审查 API

可以使用以下内容审查器 API 设置审查后工作流。

| 说明 | 参考 |
| -------------------- |-------------|
| **图像审查 API**<br /><br />通过使用标记、置信度分数和其他提取的信息扫描图像并检测潜在的成人和不雅内容。 <br /><br />使用此信息发布、拒绝或查看审查后工作流中的内容。 <br /><br />| [图像审查 API 参考](https://dev.cognitive.azure.cn/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "图像审查 API 参考")   |
| **文本审查 API**<br /><br />扫描文本内容。 返回不雅用语和个人数据。 <br /><br />使用此信息发布、拒绝或查看审查后工作流中的内容。<br /><br /> | [文本审查 API 参考](https://dev.cognitive.azure.cn/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f "文本审查 API 参考")   |
| **视频审查 API**<br /><br />扫描视频并检测潜在的成人和不雅内容。 <br /><br />使用此信息发布、拒绝或查看审查后工作流中的内容。<br /><br /> | [视频审查 API 概述](video-moderation-api.md "视频审查 API 概述")   |
| **列表管理 API**<br /><br />创建并管理图像和文本的自定义排除或包含列表。 如果启用，则“图像 - 匹配”  和“文本 - 屏幕”  操作会将提交的内容与你的自定义列表进行模糊匹配。 <br /><br />为了提高效率，可以跳过基于机器学习的审查步骤。<br /><br /> | [列表管理 API 参考](https://dev.cognitive.azure.cn/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "列表管理 API 参考")   |

## <a name="review-apis"></a>查看 API

评审 API 包含以下组件：

| 说明 | 参考 |
| -------------------- |-------------|
| **作业**<br /><br /> 为图像和文本内容启动扫描并评审审查工作流。 审查作业通过使用图像审查 API 和文本审查 API 扫描你的内容。 审查作业使用已定义和默认的工作流来生成评审。 <br /><br />在人工审查器审查了自动分配的标记和预测数据并提交了内容审核决定后，评审 API 会将所有信息提交到 API 终结点。<br /><br /> | [作业参考](https://dev.cognitive.azure.cn/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "作业参考")   |
| **评审**<br /><br />| [评审参考](https://dev.cognitive.azure.cn/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 "评审参考")   |
| **工作流**<br /><br /> | [工作流参考](https://dev.cognitive.azure.cn/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59 "工作流参考")   |



<!-- Update_Description: wording update -->