---
title: 从另一个组中添加或删除组 - Azure AD
description: 有关如何使用 Azure Active Directory 在组中添加或删除另一个组的说明。
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: v-junlch
ms.custom: it-pro, seodec18
ms.reviewer: krbain
ms.collection: M365-identity-device-management
ms.openlocfilehash: 03ba04c86d95a1a99b757a716a8f4813759f7987
ms.sourcegitcommit: 1bc154c816a5dff47ee051c431cd94826e57aa60
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75777017"
---
# <a name="add-or-remove-a-group-from-another-group-using-azure-active-directory"></a>使用 Azure Active Directory 在组中添加或删除其他组
本文帮助你使用 Azure Active Directory 在组中添加和删除其他组。

>[!Note]
>如果尝试删除父组，请参阅[如何更新或删除组及其成员](active-directory-groups-delete-group.md)。

## <a name="add-a-group-to-another-group"></a>将组添加到另一个组
可以将现有安全组添加到其他现有安全组（也称为“嵌套组”），以创建成员组（子组）和父组。 成员组继承父组的特性和属性，节省了配置时间。

>[!Important]
>当前不支持：<ul><li>将组添加到与本地 Active Directory 同步的组。</li><li>将安全组添加到 Office 365 组。</li><li>将 Office 365 组添加到安全组或其他 Office 365 组。</li><li>将应用分配到嵌套组。</li><li>将许可证应用于嵌套组。</li><li>在嵌套方案中添加通讯组。</li></ul>

### <a name="to-add-a-group-as-a-member-of-another-group"></a>若要将组作为成员添加到其他组

1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.cn)。

2. 依次选择“Azure Active Directory”、“组”。  

3. 在“组 - 所有组”页面上，搜索并选择要成为另一个组的成员的组。  对于本练习，我们将使用“MDM 策略 - 西部”  组。

    >[!Note]
    >一次只能将你的组作为成员添加到一个组。 另外，“选择组”  框会通过将输入内容与用户或设备名称的任何部分进行匹配来筛选显示内容。 但是，不支持通配符字符。

    ![“组 - 所有组”页面，其中选择了“MDM 策略 - 西部”组](./media/active-directory-groups-membership-azure-portal/group-all-groups-screen.png)

4. 在“MDM 策略 - 西部 - 组成员身份”  页面上，选择“组成员身份”  ，选择“添加”  ，找到要使你的组成为其成员的组，然后选择“选择”  。 对于本练习，我们将使用“MDM 策略 - 所有组织”  组。

    “MDM 策略 - 西部”  组现在是“MDM 策略 - 所有组织”  组的一个成员，继承了“MDM 策略 - 所有组织”组的所有属性和配置。

    ![通过将组添加到另一个组中创建组成员身份](./media/active-directory-groups-membership-azure-portal/group-add-group-membership.png)

5. 查看“MDM 策略 - 西部 - 组成员身份”  页面来查看组和成员身份。

6. 若要获得组和成员身份的详细视图，请选择组名称（“MDM 策略 - 所有组织”  ）并查看“MDM 策略 - 西部”  页面详细信息。

## <a name="remove-a-group-from-another-group"></a>从另一个组中删除组
可以从其他安全组中删除现有安全组。 但是，删除组也会删除其成员的所有继承特性和属性。

### <a name="to-remove-a-member-group-from-another-group"></a>删除组中的成员组
1. 在“组 - 所有组”页面上，搜索并选择要删除的属于另一个组的成员的组。  对于本练习，我们再次使用“MDM 策略 - 西部”  组。

2. 在“MDM 策略 - 西部”概述页面上  ，选择“组成员身份”。 

3. 从“MDM 策略 - 西部 - 组成员身份”  页面上选择“MDM 策略 - 所有组织”  ，然后从“MDM 策略 - 西部”页面详细信息中选择“删除”。  

    ![组成员身份页面，其中显示了成员和组详细信息](./media/active-directory-groups-membership-azure-portal/group-membership-remove.png)

## <a name="additional-information"></a>其他信息
这些文章提供了有关 Azure Active Directory 的更多信息。

- [查看组和成员](active-directory-groups-view-azure-portal.md)

- [创建基本组并添加成员](active-directory-groups-create-azure-portal.md)

- [从组中添加或删除成员](active-directory-groups-members-azure-portal.md)

- [编辑组设置](active-directory-groups-settings-azure-portal.md)

<!-- Update_Description: wording update -->