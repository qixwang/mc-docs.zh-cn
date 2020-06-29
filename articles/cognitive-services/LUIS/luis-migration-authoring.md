---
title: 迁移到 Azure 资源以进行创作
titleSuffix: Azure Cognitive Services
description: 迁移到 Azure 创作资源密钥。
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 02/28/2020
ms.author: v-tawe
ms.openlocfilehash: 9ec7caaae471d0b10e78a4def4a75f12ab8f43e6
ms.sourcegitcommit: 48b5ae0164f278f2fff626ee60db86802837b0b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85102066"
---
# <a name="migrate-to-an-azure-resource-authoring-key"></a>迁移到 Azure 资源创作密钥

语言理解 (LUIS) 创作身份验证已从电子邮件帐户更改为 Azure 资源。 虽然目前还不需要转换，但是将来会强制转换为 Azure 资源。

## <a name="why-migrate"></a>为何要迁移？

使用 Azure 资源进行创作允许你作为资源的所有者控制对创作的访问。 你可以创建并命名创作资源，管理不同的创建者组。

例如，你是两个 LUIS 应用的所有者，并且你有分别是各应用的协作者的不同成员。 你可以创建两个不同的创作资源，并将每个应用分配给每个单独的资源。 然后，根据每个成员所参与协作的应用，将每个成员作为参与者分配到适当的创作资源。 由 Azure 创作资源控制授权。

> [!Note]
> 在迁移之前，共同创建者在 LUIS 应用级别上称为协作者。 迁移后，将使用 Azure 的参与者角色来实现相同的功能，但是在 Azure 资源级别上实现。

## <a name="what-is-migrating"></a>什么是迁移？

迁移包括：

* LUIS 的所有用户、所有者和参与者。
* “所有”应用。
* “单向”迁移。

所有者无法选择要迁移的应用子集，且此过程不可逆。

迁移不是：

* 汇集协作者并自动移动或添加到 Azure 创作资源的过程。 应用所有者需要完成此步骤。 此步骤需要具有相应资源的权限才能完成。
* 创建和分配“预测运行时”资源的过程。 如果你需要一个预测运行时资源，这是一个[单独的进程](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal)，并且保持不变。

## <a name="how-are-the-apps-migrating"></a>应用如何迁移？

[LUIS 门户](https://luis.azure.cn)提供迁移过程。

如果存在以下情况，则需要迁移：

* 在电子邮件身份验证系统上有用于创作的应用。
* 你是应用所有者。

你可以通过取消窗口来延迟迁移过程。 系统会定期通知你进行迁移，直到已过迁移截止日期为止。 可以从顶部导航栏的锁定图标开始迁移过程。

## <a name="migration-for-the-app-owner"></a>应用所有者的迁移

### <a name="before-you-migrate"></a>迁移之前

* 必需：需要具备 [Azure 订阅](https://wd.azure.cn/pricing/1rmb-trial-full)。 订阅过程中需要帐单信息。 但是，如果使用 LUIS，可以使用免费 (`F0`) 定价层。
* 可选：通过导出每个应用或使用导出 [API](https://dev.cognitive.azure.cn/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40)，对 LUIS 门户的应用列表中的应用进行备份。
* 可选：保存每个应用的协作者列表。 此电子邮件列表作为迁移过程的一部分提供。


LUIS 应用的创作是免费的，从 `F0` 层便可知这一点。 了解[有关定价层的更多信息](luis-limits.md#key-limits)。

如果没有 Azure 订阅，请进行[注册](https://wd.azure.cn/pricing/1rmb-trial-full)。

### <a name="migration-steps"></a>迁移步骤

遵循[这些迁移步骤](luis-migration-authoring-steps.md)。

### <a name="after-you-migrate"></a>迁移后

迁移过程完成后，所有 LUIS 应用现已分配到单个 LUIS 创作资源。

可以从“LUIS 门户”中的“管理 -> Azure资源”页面创建更多创作资源并进行分配。

可以从该资源的访问控制 (IAM) 页上的 Azure 门户向创作资源添加参与者。 有关详细信息，请参阅[添加参与者访问](luis-migration-authoring-steps.md#after-the-migration-process-add-contributors-to-your-authoring-resource)。

|门户|目的|
|--|--|
|[Azure](https://wd.azure.cn/pricing/1rmb-trial-full)|* 创建预测和创作资源。<br>* 分配参与者。|
|[LUIS](https://luis.azure.cn)|* 迁移到新的创作资源。<br>* 从“管理 -> Azure 资源”页面为应用分配或取消分配预测和创作资源。|

## <a name="migration-for-the-app-contributor"></a>应用参与者的迁移

LUIS 的每个用户都需要迁移，包括协作者/参与者。 协作者必须迁移才能访问应用。

> [!Note]
> 如果 LUIS 应用的所有者将协作者迁移到并添加为 Azure 资源上的参与者，协作者将仍无法访问该应用，除非他们自己也进行迁移。

### <a name="before-the-app-is-migrated"></a>迁移应用之前

可以选择导出你是其协作者的应用，然后将该应用导入回 LUIS。 导入过程将创建一个具有新应用 ID 的新应用，且你是该应用的所有者。

### <a name="after-the-app-is-migrated"></a>迁移应用之后

应用所有者需要[作为协作者将你的电子邮件添加到 Azure 创作资源](luis-how-to-collaborate.md#add-contributor-to-azure-authoring-resource)。

迁移过程完成后，你所拥有的任何应用都可以在 LUIS 门户的“我的应用”页面上使用。

## <a name="troubleshooting-the-migration-process-for-luis-authoring"></a>对 LUIS 创作的迁移过程进行故障排除

* 只有当迁移过程完成后，才可在 LUIS 门户中看见 LUIS 创作密钥。 如果你使用（比如）LUIS CLI 创建创作密钥，用户仍需要在 LUIS 门户中完成迁移过程。
* 如果已迁移的用户将未迁移的用户作为参与者添加到其 azure 资源中，除非未迁移的用户也进行迁移，否则无权访问应用。
* 如果未迁移的用户不是任何应用的所有者，而是由其他用户拥有的其他应用的协作者，且相应的所有者已进行了迁移，则该用户需要迁移才能访问这些应用。
* 如果未迁移的用户将另一个迁移的用户作为协作者添加到其应用中，会发生错误，因为无法将迁移的用户作为协作者添加到应用中。 未迁移的用户必须完成迁移过程并创建一个 azure 资源，然后将迁移的用户作为参与者添加到该资源中。

如果出现下列情况，在迁移过程中会收到错误：
* 订阅未授权你创建认知服务资源
* 迁移会对任何应用程序运行时产生负面影响。 迁移时，将从应用中删除所有协作者，并将你作为协作者从其他应用中删除。 此过程意味着，分配的密钥也会删除。 如果已在其他应用中分配了密钥，迁移将被阻止。 迁移之前，请安全删除分配的密钥。 如果你知道分配的密钥未在运行时使用，需要将其删除，才能继续进行迁移过程。

使用以下 URL 格式访问应用的 Azure 资源列表：

`https://luis.azure.cn/applications/REPLACE-WITH-YOUR-APP-ID/versions/REPLACE-WITH-YOUR-VERSION-ID/manage/resources`

## <a name="next-steps"></a>后续步骤

* [如何将应用迁移到创作资源](luis-migration-authoring-steps.md)
