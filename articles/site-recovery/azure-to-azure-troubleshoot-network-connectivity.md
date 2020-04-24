---
title: 使用 Azure Site Recovery 对 Azure 到 Azure 灾难恢复的连接进行故障排除
description: 排查 Azure VM 灾难恢复中的连接问题
author: rockboyfor
manager: digimobile
ms.topic: how-to
origin.date: 08/05/2019
ms.date: 02/24/2020
ms.author: v-yeche
ms.openlocfilehash: bdc554c81f78eadad510f47e4d15b02aed9a0700
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77611203"
---
# <a name="troubleshoot-azure-to-azure-vm-network-connectivity-issues"></a>排查 Azure 到 Azure VM 网络连接性问题

本文介绍了在不同区域之间复制和恢复 Azure 虚拟机时与网络连接相关的常见问题。 有关网络要求的详细信息，请参阅[复制 Azure VM 的连接性要求](azure-to-azure-about-networking.md)。

要使 Site Recovery 复制正常运行，必须具有从 VM 到特定 URL 或 IP 范围的出站连接。 如果 VM 位于防火墙后或使用网络安全组 (NSG) 规则来控制出站连接，则可能会遇到以下问题之一。

**URL** | **详细信息**  
--- | ---
*.blob.core.chinacloudapi.cn | 必需，以便从 VM 将数据写入到源区域中的缓存存储帐户。 如果你知道 VM 的所有缓存存储帐户，则可以将特定存储帐户 URL（例如：cache1.blob.core.chinacloudapi.cn 和 cache2.blob.core.chinacloudapi.cn）而不是 *.blob.core.chinacloudapi.cn 加入允许列表
login.chinacloudapi.cn | 对于 Site Recovery 服务 URL 的授权和身份验证而言是必需的。
*.hypervrecoverymanager.windowsazure.cn | 必需，以便从 VM 进行 Site Recovery 服务通信。 如果防火墙代理支持 IP，则可以使用相应的“Site Recovery IP”。
*.servicebus.chinacloudapi.cn | 必需，以便从 VM 写入 Site Recovery 监视和诊断数据。 如果防火墙代理支持 IP，则可以使用相应的“Site Recovery 监视 IP”。

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Site Recovery URL 或 IP范围的出站连接（错误代码 151037 或 151072）

<a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>
## <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br-"></a>问题 1：未能向 Site Recovery 注册 Azure 虚拟机 (151195) <br />
- 可能的原因  <br />
    - 由于 DNS 解析失败而无法建立到 Site Recovery 终结点的连接。
    - 在重新保护期间，对虚拟机进行故障转移但无法从 DR 区域访问 DNS 服务器时经常会出现此问题。

- **解决方法**
    - 如果你使用的是自定义 DNS，请确保可以从灾难恢复区域访问 DNS 服务器。 若要检查你是否具有自定义 DNS，请转到“VM”>“灾难恢复网络”>“DNS 服务器”。 尝试从虚拟机访问 DNS 服务器。 如果它无法访问，请通过对 DNS 服务器进行故障转移或创建 DR 网络与 DNS 之间站点的行来使其可访问。

    ![com-error](./media/azure-to-azure-troubleshoot-errors/custom_dns.png)

## <a name="issue-2-site-recovery-configuration-failed-151196"></a>问题 2：Site Recovery 配置失败 (151196)

> [!NOTE]
> 如果虚拟机位于**标准**内部负载均衡器之后，则默认情况下无法访问 O365 IP（即 login.chinacloudapi.cn）。 请将其更改为**基本**内部负载均衡器类型或创建[此文](../load-balancer/configure-load-balancer-outbound-cli.md)中提到的出站访问权限。

- 可能的原因  <br />
    - 无法建立到 Office 365 身份验证和标识 IP4 终结点的连接。

- **解决方法**
    - Azure Site Recovery 需要具有对 Office 365 IP 范围的访问权限来进行身份验证。
        如果你使用 Azure 网络安全组 (NSG) 规则/防火墙代理控制 VM 上的出站网络连接，请确保允许到 O365 IP 范围的通信。 创建一个基于 [Azure Active Directory (Azure AD) 服务标记](../virtual-network/security-overview.md#service-tags)的 NSG 规则以允许访问与 Azure AD 对应的所有 IP 地址 - 如果以后向 Azure AD 添加了新地址，只需创建新的 NSG 规则。

### <a name="example-nsg-configuration"></a>NSG 配置示例

此示例演示如何为要复制的 VM 配置 NSG 规则。

- 如果使用 NSG 规则控制出站连接，请对所有必需的 IP 地址范围使用端口 443 的“允许 HTTPS 出站”规则。
- 此示例假设 VM 源位置是“中国东部”，目标位置是“中国中部”。

### <a name="nsg-rules---china-east"></a>NSG 规则 - 中国东部

<!--MOONCAKE: CUSTOMIZE ON "Storage"， Not Available on .ChinaEast -->

1. 在 NSG 上为“Storage”创建出站 HTTPS (443) 安全规则，如以下屏幕截图所示。

    ![storage-tag](./media/azure-to-azure-about-networking/storage-tag.png)

2. 基于 NSG 规则为“AzureActiveDirectory”创建出站 HTTPS (443) 安全规则，如以下屏幕截图所示。

    ![aad-tag](./media/azure-to-azure-about-networking/aad-tag.png)

3. 为对应于目标位置的 Site Recovery IP 创建出站 HTTPS (443) 规则：

    <!--MOONCAKE: CUSTIMIZTION ON China North-->
    
    **位置** | **Site Recovery IP 地址** |  **Site Recovery 监视 IP 地址**
    --- | --- | ---
    中国北部 | 40.125.202.254  | 42.159.4.151 

    <!--MOONCAKE: CUSTIMIZTION ON China North-->

### <a name="nsg-rules---china-north"></a>NSG 规则 - 中国北部

必须创建这些规则，才能在故障转移后启用从目标区域到源区域的复制：

<!--MOONCAKE: CUSTOMIZE ON "Storage"， Not Available on .ChinaEast -->

1. 基于 NSG 为“存储”创建出站 HTTPS (443) 安全规则。

2. 基于 NSG 规则为“AzureActiveDirectory”创建出站 HTTPS (443) 安全规则。

3. 为对应于源位置的 Site Recovery IP 创建出站 HTTPS (443) 规则：
    
    <!--MOONCAKE: CUSTIMIZTION ON China East-->
    
    **位置** | **Site Recovery IP 地址** |  **Site Recovery 监视 IP 地址**
    --- | --- | ---
    中国东部 | 42.159.205.45 | 42.159.132.40
    
    <!--MOONCAKE: CUSTIMIZTION ON China East-->

## <a name="issue-3-site-recovery-configuration-failed-151197"></a>问题 3：Site Recovery 配置失败 (151197)
- 可能的原因  <br />
    - 无法建立到 Azure Site Recovery 服务终结点的连接。

- **解决方法**
    - Azure Site Recovery 需要根据区域访问 [Site Recovery IP 范围](/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges)。 请确保可以从虚拟机访问所需的 IP 范围。

## <a name="issue-4-a2a-replication-failed-when-the-network-traffic-goes-through-on-premises-proxy-server-151072"></a>问题 4：当网络流量通过本地代理服务器时 A2A 复制失败 (151072)
- 可能的原因  <br />
    - 自定义代理设置无效，并且 Azure Site Recovery 移动服务代理未在 IE 中自动检测到代理设置

- **解决方法**
    1. 移动服务代理通过 Windows 上的 IE 和 Linux 上的 /etc/environment 检测代理设置。
    2. 如果只想对 Azure Site Recovery 移动服务设置代理，可在位于以下路径的 ProxyInfo.conf 中提供代理详细信息：<br />
        - ***Linux*** 上的 ``/usr/local/InMage/config/``
        - ***Windows*** 上的 ``C:\ProgramData\Microsoft Azure Site Recovery\Config``
    3. ProxyInfo.conf 应包含采用以下 INI 格式的代理设置。<br />
        
        *[proxy]*<br />
        *Address=http://1.2.3.4*<br />
        *Port=567*<br />
        
    4. Azure Site Recovery 移动服务代理仅支持未经身份验证的代理。

### <a name="fix-the-problem"></a>解决问题
若要允许[所需 URL](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) 或[所需 IP 范围](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges)，请按照[网络指南文档](site-recovery-azure-to-azure-networking-guidance.md)中的步骤进行操作。

## <a name="next-steps"></a>后续步骤
[复制 Azure 虚拟机](site-recovery-replicate-azure-to-azure.md)

<!-- Update_Description: update meta properties, wording update, update link -->