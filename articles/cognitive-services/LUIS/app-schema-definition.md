---
title: 应用架构定义
description: LUIS 应用使用 `.json` 或 `.lu` 表示，并且包括所有意向、实体、示例言语、功能和设置。
ms.topic: reference
author: Johnnytechn
ms.date: 08/07/2020
ms.author: v-johya
ms.openlocfilehash: 95544ccd7f47104943baf29c0df648bf8a8910bd
ms.sourcegitcommit: caa18677adb51b5321ad32ae62afcf92ac00b40b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/08/2020
ms.locfileid: "88023694"
---
# <a name="app-schema-definition"></a>应用架构定义

LUIS 应用使用 `.json` 或 `.lu` 表示，并且包括所有意向、实体、示例言语、功能和设置。

## <a name="format"></a>格式

导入和导出应用时，请选择 `.json` 或 `.lu`。

|格式|信息|
|--|--|
|`.json`| 标准编程格式|
|`.lu`|受 Bot Framework 的 [Bot Builder 工具](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Ludown/docs/lu-file-format.md)支持。|

## <a name="version-7x"></a>版本 7.x

* 迁移到版本 7.x，使用嵌套式机器学习实体来表示实体。
* 支持在以下创作 API 上利用 `enableNestedChildren` 属性创作嵌套式计算机学习实体：
    * [添加标签](https://dev.cognitive.azure.cn/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c08)
    * [添加批处理标签](https://dev.cognitive.azure.cn/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c09)
    * [评审标签](https://dev.cognitive.azure.cn/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c0a)
    * [建议用于实体的终结点查询](https://dev.cognitive.azure.cn/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c2e)
    * [建议用于意向的终结点查询](https://dev.cognitive.azure.cn/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c2d)

```json
{
  "luis_schema_version": "7.0.0",
  "intents": [
    {
      "name": "None",
      "features": []
    }
  ],
  "entities": [],
  "hierarchicals": [],
  "composites": [],
  "closedLists": [],
  "prebuiltEntities": [],
  "utterances": [],
  "versionId": "0.1",
  "name": "example-app",
  "desc": "",
  "culture": "en-us",
  "tokenizerVersion": "1.0.0",
  "patternAnyEntities": [],
  "regex_entities": [],
  "phraselists": [],
  "regex_features": [],
  "patterns": [],
  "settings": []
}
```

## <a name="version-6x"></a>版本 6.x

* 迁移到版本 6.x，使用新的[机器学习实体](reference-entity-machine-learned-entity.md)来表示实体。

```json
{
  "luis_schema_version": "6.0.0",
  "intents": [
    {
      "name": "None",
      "features": []
    }
  ],
  "entities": [],
  "hierarchicals": [],
  "composites": [],
  "closedLists": [],
  "prebuiltEntities": [],
  "utterances": [],
  "versionId": "0.1",
  "name": "example-app",
  "desc": "",
  "culture": "en-us",
  "tokenizerVersion": "1.0.0",
  "patternAnyEntities": [],
  "regex_entities": [],
  "phraselists": [],
  "regex_features": [],
  "patterns": [],
  "settings": []
}
```

## <a name="version-4x"></a>版本 4.x

```json
{
  "luis_schema_version": "4.0.0",
  "versionId": "0.1",
  "name": "example-app",
  "desc": "",
  "culture": "en-us",
  "tokenizerVersion": "1.0.0",
  "intents": [
    {
      "name": "None"
    }
  ],
  "entities": [],
  "composites": [],
  "closedLists": [],
  "patternAnyEntities": [],
  "regex_entities": [],
  "prebuiltEntities": [],
  "model_features": [],
  "regex_features": [],
  "patterns": [],
  "utterances": [],
  "settings": []
}
```

## <a name="next-steps"></a>后续步骤

* 迁移到 [V3 创作 API](luis-migration-authoring-entities.md)

