---
author: rockboyfor
ms.topic: include
origin.date: 11/15/2019
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: c1562cd8599add29e5de332e08e85e815e4ad66d
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79290701"
---
## <a name="download-and-install-the-istio-istioctl-client-binary"></a>下载并安装 Istio istioctl 客户端二进制文件

在 Windows 上基于 PowerShell 的 shell 中，使用 `Invoke-WebRequest` 下载 Istio 发行版，然后使用 `Expand-Archive` 进行解压缩，如下所示：

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.4.0"

# Enforce TLS 1.2
[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

`istioctl` 客户端二进制文件在客户端计算机上运行，用来与 Istio 服务网格交互。 在 Windows 上基于 PowerShell 的 shell 中使用以下命令安装 Istio `istioctl` 客户端二进制文件。 这些命令可将 `istioctl` 客户端二进制文件复制到某个 Istio 文件夹，然后你就可以通过 `PATH` 将其设置为立即可用（在当前 shell 中）或永久可用（跨 shell 重启）。 不需要提升的（管理员）特权即可运行这些命令，不需重启 shell。

```powershell
# Copy istioctl.exe to C:\Istio
cd istio-$ISTIO_VERSION
New-Item -ItemType Directory -Force -Path "C:\Istio"
Copy-Item -Path .\bin\istioctl.exe -Destination "C:\Istio\"

# Add C:\Istio to PATH. 
# Make the new PATH permanently available for the current User, and also immediately available in the current shell.
$PATH = [environment]::GetEnvironmentVariable("PATH", "User") + "; C:\Istio\"
[environment]::SetEnvironmentVariable("PATH", $PATH, "User") 
[environment]::SetEnvironmentVariable("PATH", $PATH)
```

<!-- Update_Description: update meta properties, wording update, update link -->