---
title: 新增功能 - 语言理解 (LUIS)
description: 本文会经常更新有关 Azure 认知服务语言理解 API 的新闻。
author: lingliw
manager: nitinme
ms.topic: conceptual
origin.date: 02/11/2020
ms.date: 02/21/2020
ms.author: diberry
ms.openlocfilehash: f2e0e12eb05847353621f6ae1ff1ace244b7d307
ms.sourcegitcommit: d202f6fe068455461c8756b50e52acd4caf2d095
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155044"
---
# <a name="whats-new-in-language-understanding"></a>语言理解中的新增功能

了解该服务中的新增功能。 这些项包括发行说明、视频、博客文章和其他类型的信息。 将此页添加为书签，以便及时了解该服务。

## <a name="release-notes"></a>发行说明

### <a name="november-4-2019---ignite"></a>2019 年 11 月 4 日 - Ignite

* 提高开发人员工作效率
    * [预测终结点 V3](luis-migration-api-v3.md) 公开发布。
    * 能够使用 .lu ([LUDown](https://github.com/microsoft/botbuilder-tools/tree/master/packages/Ludown)) 格式导入和导出应用。 这样就可以有效地进行 CI/CD 处理。
* 语言扩展
    * [阿拉伯语和印地语](luis-language-support.md)为公共预览版。
* 预生成的模型
    * [预生成的域](luis-reference-prebuilt-domains.md)现在已为正式版 (GA)
* 高级语言理解功能 - [构建复杂的语言模型](luis-concept-entity-types.md)更为轻松。
* 在模型级别定义机器学习功能，使模型能够用作其他模型的信号，例如，将实体用作意向和其他实体的功能。
* 新的扩展的[限制](luis-boundaries.md) - 提高了短语列表和短语总数的限制，使用新模型作为功能限制。
* 从深层次体系结构格式文本中提取信息，使聊天应用程序的功能更强大。

    ![机器学习实体映像](./media/whats-new/deep-entity-extraction-example.png)

### <a name="september-3-2019"></a>2019 年 9 月 3 日

* Azure 创作资源 - 现在迁移。
    * 每项 Azure 资源 500 个应用
    * 每个应用 100 个版本
* 预构建实体的土耳其语支持
* datetimeV2 的意大利语支持

### <a name="july-23-2019"></a>2019 年 7 月 23 日

* 将 [Recognizers-Text](https://github.com/microsoft/Recognizers-Text/releases/tag/dotnet-v1.2.3) 更新为 1.2.3
    * 意大利语中的年龄、温度、维度和货币识别器。
    * 改进了英语中的假期识别，以正确计算基于感恩节的日期。
    * 改进了法语日期/时间，以减少非日期和非时间实体的误报。
    * 在英文的日期范围中支持日历/学校/会计年度和首字母缩写。
    * 改进了中文和日语的 PhoneNumber 识别。
    * 改进了对英语 NumberRange 的支持。
    * 性能改进。

### <a name="june-24-2019"></a>2019 年 6 月 24 日

* 序号 V2 预生成实体，以支持排序，如下一个、上一个和最后一个。 仅限英语区域性。

### <a name="may-6-2019---build-conference"></a>2019 年 5 月 6 日 - //Build 会议

以下功能是在 Build 2019 大会上发布的：

* [V3 API 预览版迁移指南](luis-migration-api-v3.md)
* [改进的分析仪表板](luis-how-to-use-dashboard.md)
* [改进的预生成域](luis-reference-prebuilt-domains.md) 
* [动态列表实体](luis-migration-api-v3.md#dynamic-lists-passed-in-at-prediction-time)
* [外部实体](luis-migration-api-v3.md#external-entities-passed-in-at-prediction-time)


## <a name="service-updates"></a>服务更新

[有关认知服务的 Azure 更新公告](https://www.azure.cn/zh-cn/what-is-new/)