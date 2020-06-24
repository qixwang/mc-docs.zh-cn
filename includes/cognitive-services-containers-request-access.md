---
author: IEvangelist
ms.author: v-tawe
ms.service: cognitive-services
ms.topic: include
origin.date: 07/05/2019
ms.date: 01/13/2020
ms.openlocfilehash: fcc291dc5c427e848807376124ab250db513a923
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "84654933"
---
填写并提交[认知服务视觉容器请求表单](https://aka.ms/VisionContainersPreview)以请求访问容器。 通过该表单请求有关你、你的公司以及要使用该容器的用户方案的信息。 提交表单后，Azure 认知服务团队可以检查它，确保你满足访问专用容器注册表的条件。

> [!IMPORTANT]
> 必须使用与表单中的 Microsoft 帐户 (MSA) 或 Azure Active Directory (Azure AD) 帐户关联的电子邮件地址。

如果请求获得批准，则你会收到一封电子邮件，其中说明了如何获取凭据和访问专用容器注册表。

## <a name="log-in-to-the-private-container-registry"></a>登录到专用容器注册表

有几种方法可以使用认知服务容器的专用容器注册表进行身份验证。 建议通过使用 [Docker CLI](https://docs.docker.com/engine/reference/commandline/cli/) 来使用命令行方法。

使用 [docker login](https://docs.docker.com/engine/reference/commandline/login/) 命令（如以下示例所示）登录到 `containerpreview.azurecr.io`，即认知服务容器的专用容器注册表。 将 \<username\> 替换为用户名，将 \<password\> 替换为从 Azure 认知服务团队收到的凭据中提供的密码** **。

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

如果已在文本文件中保护了凭据，可以将该文本文件的内容连接到 `docker login` 命令。 使用 `cat` 命令，如以下示例所示。 将 \<passwordFile\> 替换为包含密码的文本文件的路径和名称**。 将 \<username\> 替换为凭据中提供的用户名**。

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

