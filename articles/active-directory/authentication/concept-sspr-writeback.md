---
title: 使用自助式密码重置实现本地密码写回 - Azure Active Directory
description: 了解 Azure Active Directory 中的密码更改或重置事件如何写回到本地目录环境
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: v-junlch
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 200a81a1dc9f9aa19af6e15c5099c95029dedbca
ms.sourcegitcommit: 92b9b1387314b60661f5f62db4451c9ff2c49500
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86165015"
---
# <a name="how-does-self-service-password-reset-writeback-work-in-azure-active-directory"></a>自助式密码重置写回在 Azure Active Directory 中的工作原理。

Azure Active Directory (Azure AD) 自助式密码重置 (SSPR) 允许用户在云中重置其密码，但大多数公司还具有其用户所在的本地 Active Directory 域服务 (AD DS) 环境。 密码写回是使用 [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) 启用的功能，可将云中的密码更改实时写回到现有的本地目录。 在此配置中，当用户在云中使用 SSPR 更改或重置其密码时，更新后的密码也将写回到本地 AD DS 环境

使用以下混合标识模型的环境支持密码写回：

* [密码哈希同步](../hybrid/how-to-connect-password-hash-synchronization.md)

* [Active Directory 联合身份验证服务](../hybrid/how-to-connect-fed-management.md)

密码写回提供以下功能：

* 本地 Active Directory 域服务 (AD DS) 密码策略的实施：如果用户重置密码，系统会检查此请求，以确保它符合本地 AD DS 策略要求，然后再将请求提交到相应目录。 此评审包括检查历史记录、复杂性、期限、密码筛选器，以及在 AD DS 中定义的其他任何密码限制。
* **零延迟反馈**：密码写回是一项同步操作。 如果用户的密码不符合策略或因任何原因而无法重置或更改，用户会立即收到通知。
* **支持从访问面板和 Office 365 更改密码**：如果联合用户或密码哈希同步用户更改已过期或未过期的密码，这些密码会写回到 AD DS。
* **支持当管理员在 Azure 门户中重置密码时写回密码**：当管理员在 [Azure 门户](https://portal.azure.cn)中重置用户密码时，如果该用户是联合用户或密码哈希同步用户，则密码会写回到本地。 Office 管理门户暂不支持此功能。
* 不需要任何入站防火墙规则：密码写回服务使用 Azure 服务总线中继作为基础信道。 所有通信都是通过端口 443 进行的出站通信。

> [!NOTE]
> 本地 AD 中受保护组内的管理员帐户可与密码写回一起使用。 管理员可以在云中更改其密码，但不能使用密码重置来重置遗忘的密码。 有关受保护组的详细信息，请参阅 [Active Directory 中的受保护帐户和组](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-c--protected-accounts-and-groups-in-active-directory)。

## <a name="how-password-writeback-works"></a>密码写回的工作原理

当联合或密码哈希同步用户尝试在云中重置或更改其密码时，将执行以下操作：

1. 执行检查，以确定用户具有何种类型的密码。 如果密码在本地管理：
   * 执行检查，以确定写回服务是否在正常运行。 如果是，则用户可以继续操作。
   * 如果写回服务已关闭，则告知用户暂不能重置密码。
1. 接下来，用户通过相应的身份验证入口，到达“重置密码”页。
1. 用户选择一个新密码并进行确认。
1. 如果用户选择“提交”，则使用写回设置过程中创建的对称密钥来加密纯文本密码。
1. 加密的密码将包含在一个有效负载中，该负载通过 HTTPS 通道发送到租户特定的服务总线中继（已在写回设置过程中设置此中继）。 此中继受随机生成的密码保护，只有本地安装知道该密码。
1. 在消息到达服务总线后，密码重置终结点便自动唤醒，并看到有待处理的重置请求。
1. 然后，服务使用云定位点属性查找用户。 若要成功完成此查找，必须符合以下条件：

   * Active Directory 连接器空间中必须有用户对象。
   * 用户对象必须链接到相应的 metaverse (MV) 对象。
   * 用户对象必须链接到相应的 Azure Active Directory 连接器对象。
   * Active Directory 连接器对象与 MV 的链接必须设有同步规则 `Microsoft.InfromADUserAccountEnabled.xxx`。

   当云中有调用发出时，同步引擎使用 cloudAnchor 属性，查找 Azure Active Directory 连接器空间对象。 然后，它依次链接回 MV 对象和 Active Directory 对象。 由于同一用户可能有多个 Active Directory 对象（多林），因此同步引擎依赖 `Microsoft.InfromADUserAccountEnabled.xxx` 链接选取正确的对象。

1. 找到用户帐户后，将尝试直接在相应的 Active Directory 林中重置密码。
1. 如果密码设置操作成功，将告知用户其密码已更改。

   > [!NOTE]
   > 如果用户密码哈希已使用密码哈希同步功能同步到 Azure AD，本地密码策略可能会弱于云密码策略。 在这种情况下，将实施本地策略。 此策略可确保在云中强制实施本地策略，无论使用密码哈希同步还是联合身份验证来提供单一登录，都不例外。

1. 如果密码设置操作失败，错误消息会提示用户重试。 操作失败的原因如下：
    * 服务已关闭。
    * 用户选择的密码不符合组织策略。
    * 在本地 Active Directory 中找不到用户。

   错误消息会向用户提供指导，让他们尝试解决问题，而无需管理员的干预。

## <a name="password-writeback-security"></a>密码写回安全性

密码写回是高度安全的服务。 为确保信息受到保护，我们启用了四层安全模型，如下所述：

* **租户特定的服务总线中继**
   * 当你设置服务时，我们会设置租户特定的服务总线中继，此中继受随机生成的强密码保护，而 Microsoft 永远无法访问此密码。
* **锁定的加密强密码加密密钥**
   * 创建服务总线中继后，将创建强对称密钥，用于在密码通过线路时加密密码。 此密钥仅驻留在公司在云中的密钥存储内，会被牢牢锁定并接受审核，就像目录中的其他任何密码一样。
* **行业标准传输层安全性 (TLS)**
   1. 云中发生密码重置或更改操作时，我们会使用公钥来加密纯文本密码。
   1. 加密密码将放入到使用 Microsoft TLS/SSL 证书通过加密通道发送到服务总线中继的 HTTPS 消息中。
   1. 此消息到达服务总线后，本地代理便会唤醒，并使用先前生成的强密码对服务总线进行身份验证。
   1. 本地代理选取加密的消息，并使用私钥解密消息。
   1. 本地代理尝试通过 AD DS SetPassword API 设置密码。 执行此步骤可在云中实施 Active Directory 本地密码策略（例如复杂性、期限、历史记录和筛选器）。
* **消息过期策略**
   * 如果由于本地服务关闭而导致消息位于服务总线中，消息会超时并在几分钟后遭到删除。 消息超时和删除进一步提高了安全性。

### <a name="password-writeback-encryption-details"></a>密码写回加密详细信息

在用户提交密码重置请求后，重置请求会先经历多个加密步骤，然后才会到达本地环境。 这些加密步骤可确保实现最高的服务可靠性和安全性。 这些步骤如下所述：

1. 使用 2048 位 RSA 密钥加密密码：在用户提交要写回本地的密码后，提交的密码本身会使用 2048 位 RSA 密钥进行加密。
1. 使用 AES-GCM 进行包级加密：使用 AES-GCM 加密整个包（密码及所需的元数据）。 此加密可防止任何可直接访问基础服务总线通道的人员查看或篡改内容。
1. 所有通信都是通过 TLS/SSL 进行的：与服务总线的所有通信都是在 SSL/TLS 通道中发生。 此加密可保护内容不被未经授权的第三方查看/篡改。
1. **每隔六个月自动滚动更新密钥**：每隔六个月，或者每当在 Azure AD Connect 中禁用再重新启用密码写回时，滚动更新所有密钥，确保最高的服务安全性与可靠性。

### <a name="password-writeback-bandwidth-usage"></a>密码写回带宽用量

密码写回服务是低带宽服务，只有在以下情况下，才会将请求发送回本地代理：

* 通过 Azure AD Connect 启用或禁用此功能时，发送两条消息。
* 在服务运行的持续时间内，如果服务有检测信号，则每隔 5 分钟发送一条消息。
* 每当提交新密码时，发送两条消息：
   * 第一条消息是操作执行请求。
   * 第二条消息包含操作结果，在以下情况下发送：
      * 每次在用户自助重置密码期间提交新密码时。
      * 每次在用户执行密码更改操作期间提交新密码时。
      * 每当在管理员发起的用户密码重置操作期间提交新密码时（仅限通过 Azure 管理门户）。

#### <a name="message-size-and-bandwidth-considerations"></a>消息大小和带宽注意事项

上述每条消息的大小通常小于 1KB。 即使是在承受极高负载的情况下，密码写回服务本身占用的带宽也就是每秒几个 KB。 由于每条消息是只在密码更新操作要求时才实时发送的，并且消息很小，因此密码写回功能的带宽用量微不足道，产生的影响可以忽略不计。

## <a name="supported-writeback-operations"></a>支持的写回操作

在以下所有情况下，都会写回密码：

* **支持的最终用户操作**
   * 最终用户以自助形式执行的任何自愿更改密码操作。
   * 任何最终用户自助强制更改密码操作（例如，密码到期）。
   * 源自[密码重置门户](https://passwordreset.activedirectory.windowsazure.cn)的任何最终用户自助密码重置操作。

* **支持的管理员操作**
   * 管理员以自助形式执行的任何自愿更改密码操作。
   * 任何管理员自助强制更改密码操作（例如，密码到期）。
   * 源自[密码重置门户](https://passwordreset.activedirectory.windowsazure.cn)的任何管理员自助密码重置操作。
   * 任何管理员通过 [Azure 门户](https://portal.azure.cn)发起的任何最终用户密码重置操作。
   * 任何管理员通过 [Microsoft Graph API beta](https://docs.microsoft.com/graph/api/passwordauthenticationmethod-resetpassword?view=graph-rest-beta&tabs=http) 发起的任何最终用户密码重置操作。

## <a name="unsupported-writeback-operations"></a>不支持的写回操作

在以下任意情况下，都不会写回密码：

* **不支持的最终用户操作**
   * 任何最终用户使用 PowerShell 版本 1、版本 2 或 Microsoft Graph API 重置自己的密码。
* **不支持的管理员操作**
   * 任何由管理员通过 PowerShell 版本 1、版本 2 或 Microsoft Graph API（支持 [Microsoft Graph API beta](https://docs.microsoft.com/graph/api/passwordauthenticationmethod-resetpassword?view=graph-rest-beta&tabs=http)）发起的最终用户密码重置操作。
   * 管理员通过 Microsoft 365 管理中心发起的任何最终用户密码重置。

> [!WARNING]
> 在本地 AD DS 管理工具（如 Active Directory 用户和计算机或 Active Directory 管理中心）中使用“用户下次登录时必须更改密码”复选框已作为 Azure AD Connect 的预览功能受到支持。 有关详细信息，请参阅[使用 Azure AD Connect 同步实现密码哈希同步](../hybrid/how-to-connect-password-hash-synchronization.md)。

## <a name="next-steps"></a>后续步骤

若要开始 SSPR 写回，请完成以下教程：

> [!div class="nextstepaction"]
> [教程：启用自助式密码重置 (SSPR) 写回](tutorial-enable-sspr-writeback.md)

