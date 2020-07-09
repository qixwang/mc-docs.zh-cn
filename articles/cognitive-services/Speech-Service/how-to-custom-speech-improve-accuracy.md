---
title: 改善自定义语音识别模型 - 语音服务
titleSuffix: Azure Cognitive Services
description: 特定类型的人为标记的脚本和相关文本可以根据说话场景改善语音转文本模型的识别准确性。
services: cognitive-services
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 05/20/2020
ms.date: 06/22/2020
ms.author: v-tawe
ms.openlocfilehash: 018765f5838241122b17a989e34d86db97641c12
ms.sourcegitcommit: d24e12d49708bbe78db450466eb4fccbc2eb5f99
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85706588"
---
# <a name="improve-custom-speech-accuracy"></a>提升自定义语音准确度

本文中，将了解如何通过添加音频、人为标记的脚本以及相关文本来提升自定义模型的质量。

<!-- ## Accuracy in different scenarios

Speech recognition scenarios vary by audio quality and language (vocabulary and speaking style). The following table examines four common scenarios:

| Scenario | Audio Quality | Vocabulary | Speaking Style |
|----------|---------------|------------|----------------|
| Call center | Low, 8 kHz, could be 2 humans on 1 audio channel, could be compressed | Narrow, unique to domain and products | Conversational, loosely structured |
| Voice assistant (such as Cortana, or a drive-through window) | High, 16 kHz | Entity heavy (song titles, products, locations) | Clearly stated words and phrases |
| Dictation (instant message, notes, search) | High, 16 kHz | Varied | Note-taking |
| Video closed captioning | Varied, including varied microphone use, added music | Varied, from meetings, recited speech, musical lyrics | Read, prepared, or loosely structured |

Different scenarios produce different quality outcomes. The following table examines how content from these four scenarios rates in the [word error rate (WER)](how-to-custom-speech-evaluate-data.md). The table shows which error types are most common in each scenario.

| Scenario | Speech Recognition Quality | Insertion Errors | Deletion Errors | Substitution Errors |
|----------|----------------------------|------------------|-----------------|---------------------|
| Call center | Medium (< 30% WER) | Low, except when other people talk in the background | Can be high. Call centers can be noisy, and overlapping speakers can confuse the model | Medium. Products and people's names can cause these errors |
| Voice assistant | High (can be < 10% WER) | Low | Low | Medium, due to song titles, product names, or locations |
| Dictation | High (can be < 10% WER) | Low | Low | High |
| Video closed captioning | Depends on video type (can be < 50% WER) | Low | Can be high due to music, noises, microphone quality | Jargon may cause these errors |

Determining the components of the WER (number of insertion, deletion, and substitution errors) helps determine what kind of data to add to improve the model. Use the [Custom Speech portal](https://speech.azure.cn/customspeech) to view the quality of a baseline model. The portal reports insertion, substitution, and deletion error rates that are combined in the WER quality rate. -->

## <a name="improve-model-recognition"></a>改善模型识别

可以通过在[自定义语音识别门户](https://speech.azure.cn/customspeech)中添加训练数据来减少识别错误。 

计划通过定期添加源材料来维护自定义模型。 自定义模型需要额外的训练来了解对实体的更改。 例如，可能需要更新的产品名称、歌曲名称或新的服务位置。

以下各节介绍了其他每种类型的训练数据如何减少错误。

### <a name="add-related-text-sentences"></a>添加相关的文本句子

其他相关文本句子通常可以通过在上下文中显示与错误识别常见字词和特定领域的字词相关的替换错误来减少它们。 特定领域的字词可能不太常见或者属于杜撰的字词，但其发音必须易于识别。

> [!NOTE]
> 避免相关的文本句子包含无法识别的字符或字词等干扰因素。

### <a name="add-audio-with-human-labeled-transcripts"></a>添加具有人为标记的脚本的音频

如果音频来自目标用例，则带有人为标记的脚本的音频可提供最大的准确性改进。 示例必须涵盖整个语音范围。 例如，零售商店的呼叫中心会在夏季接到大量有关泳装和太阳镜的电话。 确保示例包括要检测的整个语音范围。

考虑以下细节：

* 自定义语音识别只能捕获字词上下文来减少替换错误，而不是插入或删除错误。
* 避免使用包含脚本错误的示例，但使用包含各种音频质量的示例。
* 避免使用与问题领域无关的句子。 不相关的句子可能损坏模型。
* 当脚本质量参差不齐时，可以复制非常好的句子（例如包含关键短语的优秀脚本）以增加其权重。

### <a name="add-new-words-with-pronunciation"></a>添加具有发音的新字词

杜撰的字词或高度专业化的字词可能具有独特发音。 如果可以将这类字词分解为更小的字词，则可以识别这些字词。  例如，若要识别 Xbox，可以发音为 X box。 此方法不会提高总体准确性，但可以增加对这些关键字的识别度。

> [!NOTE]
> 此方法目前仅适用于某些语言。 有关详细信息，请参阅[语音转文本表](language-support.md)中的发音自定义。

<!-- ## Sources by scenario

The following table shows voice recognition scenarios and lists source materials to consider within the three training content categories listed above.

| Scenario | Related text sentences | Audio + human-labeled transcripts | New words with pronunciation |
|----------|------------------------|------------------------------|------------------------------|
| Call center             | marketing documents, website, product reviews related to call center activity | call center calls transcribed by humans | terms that have ambiguous pronunciations (see Xbox above) |
| Voice assistant         | list sentences using all combinations of commands and entities | record voices speaking commands into device, and transcribe into text | names (movies, songs, products) that have unique pronunciations |
| Dictation               | written input, like instant messages or emails | similar to above | similar to above |
| Video closed captioning | TV show scripts, movies, marketing content, video summaries | exact transcripts of videos | similar to above | -->

## <a name="next-steps"></a>后续步骤

- [训练模型](how-to-custom-speech-train-model.md)

## <a name="additional-resources"></a>其他资源

- [准备和测试数据](how-to-custom-speech-test-data.md)
- [检查数据](how-to-custom-speech-inspect-data.md)